ARG BASE=mbentley/omada-controller:5.5
FROM ${BASE}


COPY entrypoint.sh install.sh healthcheck.sh log4j_patch.sh /

RUN chmod +x /*.sh

ARG ARCH=armv7l
ARG INSTALL_VER="5.7"

RUN /install.sh && \
    /log4j_patch.sh &&\
    rm /install.sh /log4j_patch.sh

# Copy configuration properties
COPY omada.properties log4j2.properties /opt/tplink/EAPController/properties/

WORKDIR /opt/tplink/EAPController/lib
EXPOSE 8088 8043 8843 29810/udp 29811 29812 29813 29814
HEALTHCHECK --start-period=5m CMD /healthcheck.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["/usr/bin/java","-server","-Xms128m","-Xmx1024m","-XX:MaxHeapFreeRatio=60","-XX:MinHeapFreeRatio=30","-XX:+HeapDumpOnOutOfMemoryError","-XX:HeapDumpPath=/opt/tplink/EAPController/logs/java_heapdump.hprof","-Djava.awt.headless=true","-cp","/opt/tplink/EAPController/lib/*::/opt/tplink/EAPController/properties:","com.tplink.smb.omada.starter.OmadaLinuxMain"]
