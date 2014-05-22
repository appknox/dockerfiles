FROM ksoichiro/android

MAINTAINER ksoichiro "soichiro.kashima@gmail.com"

RUN echo y | android update sdk --filter platform-tools,build-tools-19.0.3,sysimg-17,android-17,extra-android-support --no-ui --force

# Set up and run emulator
RUN echo no | android create avd --force -n test -t android-17
# Avoid emulator assumes HOME as '/'.
ENV HOME /root
ADD wait-for-emulator /usr/local/bin/
ADD start-emulator /usr/local/bin/

RUN mkdir -p /opt/tmp && android create project -g -v 0.9.+ -a MainActivity -k com.example.example -t android-17 -p /opt/tmp
RUN cd /opt/tmp && ./gradlew tasks
RUN rm -rf /opt/tmp


RUN     apt-get update

# Install vnc, xvfb in order to create a 'fake' display and firefox
RUN     apt-get install -y x11vnc xvfb
RUN     mkdir ~/.vnc
# Setup a password
RUN     x11vnc -storepasswd 1234 ~/.vnc/passwd
EXPOSE 5900
#Do a launch script
RUN     bash -c 'echo "#!/bin/bash
Xvfb :0 -extension GLX -screen 0 1024x780x24& DISPLAY=:0 emulator -avd &
x11vnc -usepw -display :1
exit 0" >> ~/launch.sh'
