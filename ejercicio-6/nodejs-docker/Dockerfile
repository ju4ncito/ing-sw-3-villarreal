FROM node:16.17.0-alpine
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
RUN npx create-react-app my-app
RUN cd my-app
WORKDIR /usr/src/app/my-app
RUN npm install
CMD ["npm","start"]
EXPOSE 3000