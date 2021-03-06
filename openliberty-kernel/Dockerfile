FROM airhacks/java
LABEL maintainer="Tobias N. Sasse, @tnsasse and Adam Bien, adam-bien.com" description="OpenLiberty Kernel"

ENV VERSION 17.0.0.4
ENV SHA 8709c75cf905051e043b8c63e25640b464e58e39
ENV INSTALL_DIR /opt/ibm

# Download OpenLiberty
ENV OPENLIBERTY_HOME ${INSTALL_DIR}/openliberty-${VERSION}
ENV DEPLOYMENT_DIR ${OPENLIBERTY_HOME}/standalone/deployments/
RUN curl -O https://repo1.maven.org/maven2/io/openliberty/openliberty-runtime/${VERSION}/openliberty-runtime-${VERSION}.zip \
    && echo "$SHA  openliberty-runtime-${VERSION}.zip" > openliberty-runtime-${VERSION}.zip.sha1 \
    && sha1sum -c openliberty-runtime-${VERSION}.zip.sha1 \
    && unzip -q openliberty-runtime-${VERSION}.zip -d ${INSTALL_DIR} \
    && rm openliberty-runtime-${VERSION}.zip \
    && rm openliberty-runtime-${VERSION}.zip.sha1

ENV PATH=/opt/ibm/wlp/bin:$PATH

# Set Path Shortcuts
ENV LOG_DIR=/logs \
    WLP_OUTPUT_DIR=/opt/ibm/wlp/output

RUN mkdir /logs \
    && ln -s $WLP_OUTPUT_DIR/defaultServer /output \
    && ln -s /opt/ibm/wlp/usr/servers/defaultServer /config

# Configure WebSphere Liberty
RUN /opt/ibm/wlp/bin/server create \
    && rm -rf $WLP_OUTPUT_DIR/.classCache /output/workarea
COPY docker-server /opt/ibm/docker/
RUN chmod a+x /opt/ibm/docker/docker-server

EXPOSE 9080 9443

CMD ["/opt/ibm/docker/docker-server", "run", "defaultServer"]
