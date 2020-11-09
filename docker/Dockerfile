FROM alpine:3.12
RUN apk --no-cache add haproxy dumb-init
WORKDIR /app
COPY ./tunnelto/target/x86_64-unknown-linux-musl/release/tunnelto_server tunnelto/
COPY ./haproxy/haproxy.cfg haproxy/
ENV RUST_LOG=tunnelto_server=debug
ENV RUST_BACKTRACE=1
ENV ALLOW_UNKNOWN_CLIENTS=1
ENTRYPOINT ["dumb-init"]
CMD ["sh", "-c", "haproxy -f haproxy/haproxy.cfg & PORT=${TUNNELTO_TUNNEL_PORT} tunnelto/tunnelto_server"]
