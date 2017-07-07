FROM eventhandler/base

COPY context/config.yaml /etc/eventhandler/
CMD RECIPIENT=$HOSTNAME \
  /opt/eventhandler \
  --log.level debug \
  --config /etc/eventhandler/config.yaml \
  subscribe
