# This is an example Docker File
#  Command to build it
# docker built -t <image name > .
FROM palroboticssl/tiago_base_family_tutorials:melodic

RUN apt-get clean
RUN apt update
RUN apt upgrade -y
RUN apt-get install -y git && apt-get install -y python3-pip
RUN echo "git and pip Installed"
RUN apt install -y gedit
RUN apt install -y gedit-plugins
RUN echo "gedit Installed"
RUN apt install nautilus -y
RUN apt install gnome-terminal -y
RUN apt install filemanager-actions -y
RUN echo "Nautilus File manager Installed"

RUN cd /home/

RUN echo "ALL Done"
