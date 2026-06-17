import rclpy
from rclpy.node import Node
from std_msgs.msg import String
import socket
import threading
import struct
import json

class RobotinoTcpRos2Node(Node):
    def __init__(self):
        super().__init__('robotino_tcp_ros2_node')
        self.get_logger().info('Iniciando nó Robotino TCP ROS2')

        self.robotino_ip = '172.26.1.100'
        self.robotino_port = 5000

        self.publisher_ = self.create_publisher(String, 'robotino_data', 10)
        self.subscription = self.create_subscription(
            String,
            'robotino_commands',
            self.command_callback,
            10)

        self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.sock.settimeout(5)

        try:
            self.sock.connect((self.robotino_ip, self.robotino_port))
            self.get_logger().info(f'Conectado ao Robotino em {self.robotino_ip}:{self.robotino_port}')
        except Exception as e:
            self.get_logger().error(f'Erro ao conectar ao Robotino: {e}')
            return

        self.receive_thread = threading.Thread(target=self.receive_data)
        self.receive_thread.daemon = True
        self.receive_thread.start()

    def command_callback(self, msg):
        """
        Espera msg.data como JSON string com campos vx, vy, omega em m/s e rad/s.
        Exemplo: '{"vx":0.1,"vy":0.0,"omega":0.0}'
        Envia mensagem binária CMDV para o Robotino.
        """
        try:
            cmd = json.loads(msg.data)
            vx = float(cmd.get('vx', 0.0))
            vy = float(cmd.get('vy', 0.0))
            omega = float(cmd.get('omega', 0.0))

            # Monta mensagem binária: "CMDV" + 3 doubles (big endian)
            data = b'CMDV' + struct.pack('!3d', vx, vy, omega)
            self.sock.sendall(data)
            self.get_logger().info(f'Enviado comando ao Robotino: vx={vx}, vy={vy}, omega={omega}')
        except Exception as e:
            self.get_logger().error(f'Erro ao enviar comando: {e}')

    def receive_data(self):
        """
        Recebe dados do Robotino, interpreta mensagens ODOM e publica no tópico robotino_data.
        """
        buffer = b''
        while rclpy.ok():
            try:
                data = self.sock.recv(1024)
                if not data:
                    self.get_logger().warn('Conexão fechada pelo Robotino')
                    break
                buffer += data

                # Processa todas as mensagens completas no buffer
                while len(buffer) >= 52:
                    if buffer[:4] == b'ODOM':
                        # Mensagem ODOM tem 52 bytes
                        msg_bytes = buffer[:52]
                        buffer = buffer[52:]

                        # Desempacota 6 doubles big endian
                        x, y, phi, vx, vy, omega = struct.unpack('!6d', msg_bytes[4:])
                        odom_dict = {
                            'x': x,
                            'y': y,
                            'phi': phi,
                            'vx': vx,
                            'vy': vy,
                            'omega': omega
                        }
                        msg = String()
                        msg.data = json.dumps(odom_dict)
                        self.publisher_.publish(msg)
                        self.get_logger().info(f'Recebido odometria: {msg.data}')
                    else:
                        # Se não começa com ODOM, descarta primeiro byte e tenta novamente
                        buffer = buffer[1:]

            except socket.timeout:
                continue
            except Exception as e:
                self.get_logger().error(f'Erro ao receber dados: {e}')
                break

def main(args=None):
    rclpy.init(args=args)
    node = RobotinoTcpRos2Node()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()