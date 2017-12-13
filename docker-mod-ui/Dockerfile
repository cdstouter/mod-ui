FROM moddevices/mod-plugin-builder

RUN sudo apt-get update
RUN sudo apt-get install -qy build-essential checkinstall libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev openssl
RUN mkdir -p $HOME/opt &&\
    cd $HOME/opt &&\
    curl -O https://www.python.org/ftp/python/3.4.3/Python-3.4.3.tgz &&\
    tar xzvf Python-3.4.3.tgz && cd Python-3.4.3 &&\
    ./configure --enable-shared --prefix=/usr LDFLAGS="-Wl,--rpath=/usr/lib" &&\
    sudo make altinstall
ADD requirements.txt /home/builder/mod-plugin-builder/requirements.txt
RUN sudo pip3.4 install -r requirements.txt
RUN echo "source /home/builder/mod-plugin-builder/local.env" >> $HOME/.bashrc
CMD ["bash"]