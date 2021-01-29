# ---------- Source ---------- #
FROM alpine AS source
RUN apk add git curl

RUN curl -O http://downloadarchive.documentfoundation.org/libreoffice/old/5.3.7.2/rpm/x86_64/LibreOffice_5.3.7.2_Linux_x86-64_rpm.tar.gz
RUN curl -O https://download.tanukisoftware.com/wrapper/3.5.29/wrapper-linux-x86-64-3.5.29.tar.gz
RUN tar -xvf LibreOffice_5.3.7.2_Linux_x86-64_rpm.tar.gz
RUN tar -xvf wrapper-linux-x86-64-3.5.29.tar.gz
RUN git clone --depth 1 --branch 9.1 https://github.com/doculibre/constellio.git source

# ---------- Build ---------- #
FROM gradle:4.7.0-jdk8-alpine AS build
COPY --from=source --chown=gradle:gradle /source /home/gradle/constellio

WORKDIR /home/gradle

RUN mv constellio/gradle.properties.main gradle.properties
RUN mv constellio/build.gradle.main build.gradle
RUN mv constellio/settings.gradle.main settings.gradle
COPY version version

RUN gradle war --no-daemon
RUN mkdir webapp && mv constellio/build/libs/constellio-*.war webapp
RUN (cd webapp && unzip constellio-*.war && rm -f constellio-*.war)

# ---------- Package ---------- #
FROM centos:8
WORKDIR /opt
RUN yum install -y java-1.8.0-openjdk unzip libXinerama cairo cups

# Install libreoffice
COPY --from=source /LibreOffice_5.3.7.2_Linux_x86-64_rpm/RPMS Libreoffice_rpms
RUN rpm -ivh Libreoffice_rpms/*.rpm
RUN rm -rf Libreoffice_rpms
RUN ln -s /opt/libreoffice* /opt/libreoffice

# Install wrapper

COPY lib /opt/constellio/
COPY --from=source /wrapper-linux-x86-64-3.5.29/bin/wrapper /opt/constellio/
COPY --from=source /wrapper-linux-x86-64-3.5.29/lib/libwrapper.so /opt/constellio/lib/
COPY --from=source /wrapper-linux-x86-64-3.5.29/lib/wrapper.jar /opt/constellio/lib/
COPY --from=build /home/gradle/webapp /opt/constellio/webapp/

RUN mkdir -p /opt/solr/bin/
RUN echo 'SOLR_JAVA_MEM="-Xms512m -Xmx4096m"' > /opt/solr/bin/solr.in.sh

RUN ["chmod", "+x", "/opt/constellio/docker-environment"]
RUN ["chmod", "u+x", "/opt/constellio/startup"]

EXPOSE 8080
ENTRYPOINT ["/opt/constellio/docker-environment"]
CMD ["/opt/constellio/startup", "start"]
