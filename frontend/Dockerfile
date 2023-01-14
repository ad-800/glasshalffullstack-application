FROM node:18-alpine3.16

COPY package.json package-lock.json ./

RUN npm install

COPY . .

RUN npm run build

# create the image:
#   docker build -t frontend .

# run the image (as shell instance):
#   docker run -it -p 3000:3000 frontend sh
#   # ...

# TODO: push container to Digital Ocean, so that our frontend is hosted there :)