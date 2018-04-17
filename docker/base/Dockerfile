FROM golang:1.8 as builder

WORKDIR $GOPATH/src
RUN git clone https://github.com/zwopir/eventhandler.git

WORKDIR $GOPATH/src/eventhandler
RUN go get -v -t -u ./...
RUN go test -v -cover ./...
RUN go build -o /opt/eventhandler main.go

FROM alpine:latest
COPY --from=builder /opt/eventhandler /opt/
RUN mkdir /etc/eventhandler
