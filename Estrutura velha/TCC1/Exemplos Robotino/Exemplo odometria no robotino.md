abri o codigo de um exemplo: Exemplo odometry, nome do arquivo: main.cpp

Caminho: robotino@robotino:/usr/local/OpenRobotinoAPI/1/examples/c++/odometry$ ls
example_odometry  main.cpp  Makefile



//  Copyright (C) 2004-2008, Robotics Equipment Corporation GmbH

#define _USE_MATH_DEFINES
#include <cmath>
#include <iostream>

#include "rec/robotino/com/all.h"
#include "rec/core_lt/utils.h"
#include "rec/core_lt/Timer.h"
#include "rec/core_lt/Thread.h"

using namespace rec::robotino::com;

class MyInfo : public Info
{
public:
        MyInfo()
        {
        }

        void infoReceivedEvent( const char* text )
        {
                std::cout << text << std::endl;
        }
};

class MyCom : public Com
{
        public:
                MyCom()
                {
                }


                void errorEvent( Error error, const char* errorString )
                {
                        std::cerr << "Error: " << errorString << std::endl;
                }

                void connectedEvent()
                {
                        std::cout << "Connected." << std::endl;
                }

                void connectionClosedEvent()
                {
                        std::cout << "Connection closed." << std::endl;
                }
};

MyCom com;
OmniDrive omniDrive;
Bumper bumper;
Odometry odometry;
MyInfo info;

void init( const std::string& hostname )
{
        odometry.setComId( com.id() );

        omniDrive.setComId( com.id() );

        bumper.setComId( com.id() );

        info.setComId( com.id() );



  // Connect
  std::cout << "Connecting..." << std::endl;
  com.setAddress( hostname.c_str() );

        odometry.set( 200, 0, 0 );

        //com.setAutoUpdateEnabled( false );
        com.connect();

  std::cout << std::endl << "Connected" << std::endl;
}

void drive()
{
        rec::core_lt::Timer timer;
        timer.start();


        std::cout << odometry.x() << "  " << odometry.y() << "  " << odometry.phi() << std::endl;

        while( com.isConnected() )
  {
                //std::cout << odometry.x() << "  " << odometry.y() << "  " << odometry.phi() << std::endl;
                //omniDrive.setVelocity( 100, 0, 0 );
                //com.update();
                rec::core_lt::msleep( 20 );
  }
}

void destroy()
{
  com.disconnect();
}

class MyThread : public rec::core_lt::Thread
{
public:
        MyThread()
        {
        }

        void run()
        {
                        rec::core_lt::msleep( 100 );
                        destroy();
        }
};

int main( int argc, char **argv )

{
        std::string hostname = "172.26.1.1";
        if( argc > 1 )
        {
                hostname = argv[1];
        }

        MyThread my;

  try
  {
                for( int i=0; i<1000; ++ i )
                {
                        init( hostname );
                        my.start();
                        drive();

                        //rec::core_lt::msleep( 2000 );
                }

                my.stop();
  }
        catch( const std::exception& e )
  {
    std::cerr << "Error: " << e.what() << std::endl;
  }

  std::cout << "Press any key to exit..." << std::endl;
  rec::core_lt::waitForKey();
}