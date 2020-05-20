FROM openfaas/classic-watchdog:0.18.1 as watchdog

FROM node:alpine

RUN mkdir -p /data

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

# Add non root user
RUN addgroup -S app && adduser app -S -G app
RUN chown app /data

WORKDIR /data

USER app

COPY . .
RUN yarn install

# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="yarn start"
# Set to true to see request in function logs
ENV write_debug="false"

EXPOSE 1880
HEALTHCHECK --interval=3s CMD ["curl","localhost:1880/health"]

CMD ["fwatchdog"]
CMD ["yarn","start"]
