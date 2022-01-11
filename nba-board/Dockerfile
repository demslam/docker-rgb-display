# syntax=docker/dockerfile:1
FROM debian:11.1-slim
RUN apt-get update
#RUN apt-get install python3 python3-dev python3-pip git build-essential python3-pillow cron apt-utils -y --fix-missing
RUN apt-get install python3-pip git cron -y --fix-missing
RUN pip3 install bs4 requests
RUN mkdir -p /home/pi/Documents
RUN apt-get install python3-dev python3-pillow -y
RUN cd /home/pi && git clone https://github.com/hzeller/rpi-rgb-led-matrix.git
RUN cd /home/pi/rpi-rgb-led-matrix && make build-python PYTHON=$(which python3)
RUN cd /home/pi/rpi-rgb-led-matrix && make install-python PYTHON=$(which python3)
RUN cd /home/pi && git clone https://github.com/bder222/NBA-scoreboard
RUN echo "0 11 * * * root python3 /home/pi/NBA-scoreboard/Spreads_New_Day.py" >> /etc/crontab
RUN echo "0 */2 * * * root python3 /home/pi/NBA-scoreboard/Spreads_Update.py" >> /etc/crontab
RUN echo "#!/bin/bash" >> /start.sh
RUN echo "python3 /home/pi/NBA-scoreboard/Spreads_New_Day.py" >> /start.sh
RUN echo "python3 /home/pi/NBA-scoreboard/Spreads_Update.py" >> /start.sh
RUN echo "service cron start" >> /start.sh
RUN echo "python3 /home/pi/NBA-scoreboard/NBA_Data.py & wait 5 & python3 /home/pi/NBA-scoreboard/NBA_Render.py" >> /start.sh
RUN chmod +x /start.sh
RUN apt-get clean && apt-get autoclean
ENTRYPOINT "/start.sh"



 
