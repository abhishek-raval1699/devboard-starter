FROM golang:1.22-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o devboard-backend .

FROM alpine:3.20

RUN adduser -D -H -u 10001 appuser \
    && apk add --no-cache ca-certificates

WORKDIR /app

COPY --from=builder /app/devboard-backend ./devboard-backend

USER appuser

EXPOSE 8080

ENTRYPOINT ["./devboard-backend"]
