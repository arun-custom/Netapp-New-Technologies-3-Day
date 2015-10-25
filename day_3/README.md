# Docker

Sample Docker File:

```
FROM ubuntu:14.04

RUN apt-get update

RUN apt-get install -y nodejs npm git git-core

# Bundle app source
COPY . /src

# Install app dependencies
RUN cd /src; npm install

# Run app on specified port
EXPOSE 3000

CMD ["node", "/src/app.js"]
```