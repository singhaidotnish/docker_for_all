**To run GUI application with docker mandatory is Xserver.**

build image using dockerfile

FROM ubuntu:latest

RUN apt install -y apt-transport-https

RUN apt update && DEBIAN_FRONTEND=noninteractive apt install -y ubuntu-desktop lightdm

RUN rm /run/reboot-required*
RUN echo "/usr/sbin/lightdm" > /etc/X11/default-display-manager
RUN echo "\
[LightDM]\n\
[Seat:*]\n\
type=xremote\n\
xserver-hostname=host.docker.internal\n\
xserver-display-number=0\n\
autologin-user=root\n\
autologin-user-timeout=0\n\
" > /etc/lightdm/lightdm.conf.d/lightdm.conf

ENV DISPLAY=host.docker.internal:0.0

CMD service dbus start ; /usr/lib/systemd/systemd-logind & service lightdm start


**If any issue in building clear cache then try again. **

docker builder prune

To save docker image - docker save -o <path for generated tar file> <image name>

To load docker image - docker load -i <path to image tar file>

Configuration of xserver is 

1. Multiple windows
2. Start no client
3. Check all options
4. save configuration

Then run docker


