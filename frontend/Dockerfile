FROM denoland/deno:alpine-2.0.5@sha256:86448a9dbb0742997cd251ec94ad01b2ef65fca9429c871376b89a8c6781aee7

WORKDIR /app

COPY . .

RUN deno task build
RUN rm -rf $DENO_DIR
RUN deno install --allow-import -e main.ts

RUN timeout 2s deno run -A --cached-only main.ts || true

CMD ["run", "-A", "--cached-only", "main.ts"]
