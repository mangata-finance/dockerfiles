FROM rust:1.65-buster

# metadata
LABEL summary="Image for Rust and WASM compilation." \
	name="mangatasolutions/node-builder" \
	maintainer="admin@mangata.finance" \
	version="1.0" \
	description="libssl-dev, clang, libclang-dev, lld, cmake, make, git, pkg-config \
curl, time, jq, lsof, rhash, rust stable, rust nightly, sccache" \
	io.parity.image.vendor="Mangata Finance" \
	io.parity.image.source="https://github.com/mangata-finance/dockerfiles/blob/main/\
node-builder/Dockerfile" \
	io.parity.image.documentation="https://github.com/mangata-finance/dockerfiles/blob/main/\
node-builder/Dockerfile" 

WORKDIR /builds

# install tools and dependencies
RUN set -eux && \
	# Install mold linker
	curl -L https://github.com/rui314/mold/releases/download/v1.7.0/mold-1.7.0-$(uname -m)-linux.tar.gz \
	| tar -C /usr/local --strip-components=1 -xzf - && ln -sf /usr/local/bin/mold "$(realpath /usr/bin/ld)" && \
	# Install sccache
	curl -L https://github.com/mozilla/sccache/releases/download/v0.3.1/sccache-v0.3.1-$(uname -m)-unknown-linux-musl.tar.gz -O && \
	tar -C /usr/local/bin --strip-components=1 --wildcards "*sccache" -xzf sccache-v0.3.1-$(uname -m)-unknown-linux-musl.tar.gz && \
	chmod +x /usr/local/bin/sccache && rm -rf sccache-v0.3.1-$(uname -m)-unknown-linux-musl.tar.gz

ENV RUST_TOOLCHAIN=nightly-2022-10-10

RUN rustup install $RUST_TOOLCHAIN && rustup default $RUST_TOOLCHAIN && \
	rustup target add wasm32-unknown-unknown


# cache handler
ENV	RUSTC_WRAPPER=sccache \
# show backtraces
  	RUST_BACKTRACE=1