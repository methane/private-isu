#FROM unit:1.34.0-python3.12  # リリースされたけどdocker hubにはアップされてない
FROM unit:1.33.0-python3.12

COPY --from=ghcr.io/astral-sh/uv:bookworm /usr/local/bin/uv /usr/local/bin/uvx /usr/local/bin/
RUN \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  --mount=type=cache,target=/var/cache/apt,sharing=locked \
  apt-get update -qq && apt-get install -y build-essential default-libmysqlclient-dev

RUN mkdir -p /home/webapp
WORKDIR /home/webapp

COPY pyproject.toml uv.lock .
RUN --mount=type=cache,target=/root/.cache/uv \
  uv sync --compile-bytecode

COPY unit_config.json /docker-entrypoint.d/
COPY . .

#CMD [ "/bin/bash", "-l" ]
# unit の ENTRYPOINT を利用する
