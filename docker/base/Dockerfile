FROM golang:1.8

WORKDIR $GOPATH/src
RUN git clone https://github.com/zwopir/eventhandler.git

WORKDIR $GOPATH/src/eventhandler
RUN go get -v -t -u ./...
RUN go test -v -cover ./...
RUN go build -o /opt/eventhandler main.go
RUN mkdir /etc/eventhandler
