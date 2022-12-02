# syntax=docker/dockerfile:1
FROM --platform=linux/x86_64 golang:1.18
RUN HP=$HTTP_PROXY
RUN HPS=$HTTPS_PROXY
ENV HTTP_PROXY http://127.0.0.1:3128/
ENV HTTPS_PROXY http://127.0.0.1:3128/
WORKDIR /build
COPY . .
ENV CGO_ENABLED=0 
ENV GOOS=linux
ARG TARGETOS TARGETARCH

RUN cd cmd/agent;GOOS=$TARGETOS GOARCH=$TARGETARCH go build -o /runner -mod mod -a .

ENV HTTP_PROXY $HP
ENV HTTPS_PROXY $HPS

FROM alpine
RUN useradd -u 1001 nonrootuser
ENV HTTP_PROXY http://127.0.0.1:3128/
ENV HTTPS_PROXY http://127.0.0.1:3128/
RUN apk --no-cache add ca-certificates git
# WORKDIR /root/
COPY --from=0 /runner /bin/runner
ENV HTTP_PROXY $HP
ENV HTTPS_PROXY $HPS
USER 1001
ENTRYPOINT ["/bin/runner"]
