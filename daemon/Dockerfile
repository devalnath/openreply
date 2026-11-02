FROM node:22-bookworm-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates git openssl \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Vercel scopes the build context to this directory. Fetch only the clean,
# public source at this deployment's commit, never local runtime data.
ARG VERCEL_GIT_COMMIT_SHA
RUN test -n "$VERCEL_GIT_COMMIT_SHA" \
    && git init . \
    && git remote add origin https://github.com/devalnath/openreply.git \
    && git fetch --depth=1 origin "$VERCEL_GIT_COMMIT_SHA" \
    && git checkout --detach FETCH_HEAD

RUN npm ci --include=dev \
    && npm run db:generate

ENV NODE_ENV=production
ENV NODE_OPTIONS=--max-old-space-size=512

CMD ["npm", "run", "worker"]
