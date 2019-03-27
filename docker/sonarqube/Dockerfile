FROM sonarqube:7.7-community

ENV GLOWROOT_VERSION=0.13.2

# grab glowroot and install it
RUN set -x \
    && cd /opt/sonarqube \ 
    && curl -o glowroot-dist.zip -fSL https://github.com/glowroot/glowroot/releases/download/v$GLOWROOT_VERSION/glowroot-$GLOWROOT_VERSION-dist.zip \
    && unzip glowroot-dist.zip \
    && rm -f glowroot-dist.zip \
    && mkdir -p glowroot/conf \
    && mkdir -p glowroot/logs \
    && mkdir -p glowroot/tmp                       


 
