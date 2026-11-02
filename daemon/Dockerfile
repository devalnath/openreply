FROM node:22-bookworm-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates git openssl \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# The build context contains only this folder. Fetch the clean public source
# at the verified dependency-hardening commit, never local runtime data.
# Update this pin when deploying a newer, verified worker implementation.
ARG OPENREPLY_SOURCE_REF=6d603128a038c018b9ece402c716861c688236d1
RUN git init . \
    && git remote add origin https://github.com/devalnath/openreply.git \
    && git fetch --depth=1 origin "$OPENREPLY_SOURCE_REF" \
    && git checkout --detach FETCH_HEAD

RUN npm ci --include=dev \
    && npm run db:generate

ENV NODE_ENV=production
ENV NODE_OPTIONS=--max-old-space-size=512

CMD ["npm", "run", "worker"]
