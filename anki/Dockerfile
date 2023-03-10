# docker build -t anki:latest -f Dockerfile .

FROM kasmweb/core-ubuntu-focal:1.12.0
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########
RUN apt-get update && apt-get install -y zstd xdg-utils firefox mpv

ENV ANKI_VERSION 2.1.56
ENV ANKI_FILEROOT anki-${ANKI_VERSION}-linux-qt6
ENV ANKI_FILENAME ${ANKI_FILEROOT}.tar.zst
ENV ANKI_TEMP_DIR /tmp

RUN cd ${ANKI_TEMP_DIR} && wget https://github.com/ankitects/anki/releases/download/${ANKI_VERSION}/${ANKI_FILENAME}
RUN cd ${ANKI_TEMP_DIR} && tar --use-compress-program=unzstd -xvf ${ANKI_TEMP_DIR}/${ANKI_FILENAME}
RUN cd ${ANKI_TEMP_DIR}/${ANKI_FILEROOT} && ./install.sh

RUN cp /usr/local/share/applications/anki.desktop $HOME/Desktop/ && chmod +x $HOME/Desktop/anki.desktop

# setup anki profile
# to create this: (from inside ~/.local/share)
# tar cvzf Anki2.tar.gz Anki2
COPY Anki2.tar.gz ${ANKI_TEMP_DIR}
RUN mkdir -p $HOME/.local/share && cd $HOME/.local/share && tar xvzf ${ANKI_TEMP_DIR}/Anki2.tar.gz

# download anki deck
RUN mkdir -p $HOME/AnkiDecks/ && wget https://cloud-language-tools-storage.nyc3.digitaloceanspaces.com/collection.colpkg -O $HOME/AnkiDecks/collection.colpkg

# change desktop background
RUN wget https://res.cloudinary.com/photozzap/image/upload/v1594529672/ankireview_prod/backgrounds/abstract_future/yjdfbxopgneezbgqc1di.png \
 -O /usr/share/extra/backgrounds/bg_default.png

######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME

USER 1000