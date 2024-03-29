# Node Builder image to be used in CI workflows of `mangata-node` repository
This image is being built for multiple platform so that it can be used on ARM-based local development machines.

## Pre-requisites
On the host where you build you'll need to run next commands before running build:
```
docker run --privileged --rm tonistiigi/binfmt --install all
docker buildx create --use --name custom-builder
```

## Build command
```
# Latest:
docker buildx build --platform linux/arm64,linux/amd64 -t mangatasolutions/node-builder:multi --push .
# Specific toolchain:
export RUST_TOOLCHAIN=nightly-2023-05-22
docker buildx build --platform linux/arm64,linux/amd64 --build-arg RUST_TOOLCHAIN=${RUST_TOOLCHAIN} -t mangatasolutions/node-builder:multi-${RUST_TOOLCHAIN} --push .
```

## Some caveats
This image requires ~40Gb RAM to be available to build because of the cargo compilations for the different platforms for SubAlfred.
If you'll experience issues with building it, getting errors during compilation next links may be helpful:
- https://github.com/docker/buildx/issues/495#issuecomment-761562905