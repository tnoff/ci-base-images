FROM rust:1

# Full set of system libs required to compile a Tauri + WebKit2GTK app on
# Debian. libappindicator3-dev pulls many GTK libs transitively.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        libwebkit2gtk-4.1-dev \
        libappindicator3-dev \
        librsvg2-dev \
        patchelf \
        libdbus-1-dev \
        libssl-dev \
        pkg-config && \
    apt-get upgrade -y --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

RUN rustup component add clippy
