FROM logstash:5.5.0-alpine

RUN mkdir -p /log/volumes
COPY logstash.conf /etc/logstash.conf

ENTRYPOINT ["logstash"]
CMD ["-f", "/etc/logstash.conf"]
