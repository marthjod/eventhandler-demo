FROM eventhandler/base

COPY context/config.yaml /etc/eventhandler/
ENTRYPOINT ["/opt/eventhandler", "--log.level", "debug", "--config", "/etc/eventhandler/config.yaml", "publish"]
