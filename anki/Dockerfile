# docker build -t anki:latest -f Dockerfile .

FROM kasmweb/core-ubuntu-focal:1.12.0
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########
RUN apt-get update && apt-get install -y zstd

ENV ANKI_FILENAME anki-2.1.55-linux-qt6.tar.zst
ENV ANKI_TEMP_DIR /tmp
ENV ANKI_INSTALL_DIR /opt/anki

RUN cd ${ANKI_TEMP_DIR} && wget https://github.com/ankitects/anki/releases/download/2.1.55/${ANKI_FILENAME}
RUN mkdir ${ANKI_INSTALL_DIR} && cd ${ANKI_INSTALL_DIR} && tar --use-compress-program=unzstd -xvf ${ANKI_TEMP_DIR}/${ANKI_FILENAME}

RUN echo "08:57am" $HOME/Desktop/hello.txt


######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME

USER 1000