FROM golang:latest AS builder
WORKDIR /app
COPY . .
RUN go build greetings.go

FROM ubuntu:latest AS production
WORKDIR /app
COPY --from=builder /app/greetings .
EXPOSE 80
CMD ["/app/greetings"]
