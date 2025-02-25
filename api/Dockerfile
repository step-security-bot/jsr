# This Dockerfile is adapted from https://whitfin.io/blog/speeding-up-rust-docker-builds/

FROM rust:1.83@sha256:a45bf1f5d9af0a23b26703b3500d70af1abff7f984a7abef5a104b42c02a292b as build

# create a new empty shell project
RUN USER=root cargo new --bin registry_api
WORKDIR /registry_api

# copy over your manifests
COPY ./Cargo.lock ./Cargo.lock
COPY ./Cargo.toml ./Cargo.toml

# this build step will cache your dependencies
RUN cargo build --release
RUN rm src/*.rs

# copy your source tree
COPY ./src ./src
COPY ./migrations ./migrations
COPY ./.sqlx ./.sqlx

# build for release
RUN rm ./target/release/deps/registry_api*
RUN cargo build --release

# our final base
FROM debian:bookworm-20230703-slim@sha256:9bd077d2f77c754f4f7f5ee9e6ded9ff1dff92c6dce877754da21b917c122c77

RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*

# copy the build artifact from the build stage
COPY --from=build /registry_api/target/release/registry_api .

ENV RUST_BACKTRACE=1

# set the startup command to run your binary
ENTRYPOINT ["./registry_api"]
