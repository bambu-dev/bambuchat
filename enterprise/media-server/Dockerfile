FROM golang:1.22-alpine

RUN apk add --no-cache git gcc musl-dev ca-certificates wget

WORKDIR /build

COPY go.mod go.sum* ./
RUN if [ -f go.sum ]; then go mod download; fi

COPY . .
RUN go mod tidy && CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /usr/local/bin/chatwoot-media-server ./cmd/server/

# Crear carpeta de grabaciones
RUN mkdir -p /recordings

EXPOSE 4000
EXPOSE 10000-12000/udp

HEALTHCHECK --interval=10s --timeout=5s --retries=3 \
  CMD wget --spider -q http://localhost:4000/health || exit 1

ENTRYPOINT ["chatwoot-media-server"]
