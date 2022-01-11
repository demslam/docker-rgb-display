# syntax=docker/dockerfile:1
FROM debian:11.1-slim
RUN apt-get update && apt-get upgrade -y
RUN apt-get install python3 python3-dev python3-pip git build-essential python3-pillow -y
RUN cd /opt/ && git clone --recursive https://github.com/gidger/rpi-led-nhl-scoreboard.git
RUN cd /opt/rpi-led-nhl-scoreboard/submodules/rpi-rgb-led-matrix/ && make PYTHON=/usr/bin/python3
RUN cd /opt/rpi-led-nhl-scoreboard/submodules/rpi-rgb-led-matrix/ && make install-python PYTHON=/usr/bin/python3
RUN apt-get install python3-requests python3-asn1crypto python3-certifi python3-chardet python3-cryptography python3-entrypoints python3-idna python3-keyring python3-keyrings.alt python3-olefile python3-xdg python3-secretstorage python3-six python3-urllib3 -y
#adjust display to use adafruit-hat
RUN sed "s/adafruit-hat-pwm/adafruit-hat/" /opt/rpi-led-nhl-scoreboard/rpi-led-nhl-scoreboard.py > /opt/rpi-led-nhl-scoreboard/rpi-led-nhl-scoreboard-nopwm.py
RUN apt-get clean && apt-get autoclean
RUN echo "#!/bin/bash" >> /start.sh
RUN echo "cd /opt/rpi-led-nhl-scoreboard/" >> /start.sh
RUN echo "n=0" >> /start.sh
RUN echo "until [ $n -ge 10 ]" >> /start.sh
RUN echo "do" >> /start.sh
RUN echo "  python3 rpi-led-nhl-scoreboard-nopwm.py && break" >> /start.sh
RUN echo "  n=$[$n+1]" >> /start.sh
RUN echo "  sleep 10" >> /start.sh
RUN echo "done" >> /start.sh
RUN chmod +x /start.sh
ENTRYPOINT "/start.sh"

 
