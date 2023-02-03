FROM node:16-alpine as base

RUN apk update \
    # Install git
    && apk add --no-cache git

WORKDIR /var/www/html

ENV HOST 0.0.0.0

# Expose the Next.js port
EXPOSE 3000

# Export the Vite port
EXPOSE 24678

CMD ["npm", "run", "start"]

FROM base as dev

CMD ["npm", "run", "dev"]

