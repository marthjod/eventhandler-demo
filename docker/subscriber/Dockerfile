FROM eventhandler/base

COPY context/config.yaml /etc/eventhandler/
CMD /opt/eventhandler \
  --log.level debug \
  --config /etc/eventhandler/config.yaml \
  subscribe
