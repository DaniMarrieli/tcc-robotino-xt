O codigo a seguir é do cliente TCP que está no raspberry de nome robotino_tcp_ros2_node.py 

import rclpy
from rclpy.node import Node
from std_msgs.msg import String
import socket
import threading

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

      self.receive_thread = threading.Thread(target=self.receive_data)
      self.receive_thread.daemon = True
      self.receive_thread.start()


    def command_callback(self, msg):
        try:
            data = msg.data.encode('utf-8')
            self.sock.sendall(data)
            self.get_logger().info(f'Enviado comando ao Robotino: {msg.data}')
        except Exception as e:
            self.get_logger().error(f'Erro ao enviar comando: {e}')

    def receive_data(self):
        while rclpy.ok():
            try:
                data = self.sock.recv(1024)
                if data:
                    msg = String()
                    msg.data = data.decode('utf-8')
                    self.publisher_.publish(msg)
                    self.get_logger().info(f'Recebido do Robotino: {msg.data}')
                else:
                    self.get_logger().warn('Conexão fechada pelo Robotino')
                    break
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


