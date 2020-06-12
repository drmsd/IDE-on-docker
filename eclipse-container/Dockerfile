FROM openjdk:9

RUN  \
  apt-get update && apt-get install --no-install-recommends -y \
  gcc git openssh-client less \
  libxtst-dev libxext-dev libxrender-dev libfreetype6-dev \
  libfontconfig1 libgtk2.0-0 libxslt1.1 libxxf86vm1 \
  && rm -rf /var/lib/apt/lists/* \
  && useradd -ms /bin/bash developer

ARG eclipse_source=http://mirror.math.princeton.edu/pub/eclipse/technology/epp/downloads/release/photon/R/eclipse-jee-photon-R-linux-gtk-x86_64.tar.gz
ARG eclipse_local_dir=eclipse-workspace

WORKDIR /opt/eclipse

RUN wget -c $eclipse_source -O /opt/eclipse/installer.tgz \
  && tar --strip-components=1 -xvf installer.tgz \
  && rm installer.tgz


USER developer
ENV HOME /home/developer

RUN mkdir /home/developer/.eclipse \
  && ln -sf /home/developer/.eclipse /home/developer/$eclipse_local_dir

CMD ["/opt/eclipse/eclipse"]

