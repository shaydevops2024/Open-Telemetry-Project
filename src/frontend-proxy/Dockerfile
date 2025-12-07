FROM envoyproxy/envoy:v1.32.2

RUN apt-get update && apt-get install -y --no-install-recommends \
    gettext-base \
    && rm -rf /var/lib/apt/lists/*

USER envoy
WORKDIR /home/envoy

COPY env_vars.yaml .

ENTRYPOINT ["sh", "-c", "envsubst < env_vars.yaml > envoy.yaml && envoy -c envoy.yaml"]

