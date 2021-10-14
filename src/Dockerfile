FROM registry-k8s.yesbank.in/npmdependency:node as react-built

WORKDIR /app
ARG nexus_repo
RUN npm config set registry $nexus_repo
RUN echo $nexus_repo
RUN npm config list

COPY package.json .
#COPY package-lock.json .
RUN npm install --silent
# RUN npm install react-scripts@3.4.1 -g --silent
COPY . .
RUN npm run-script build

FROM registry-k8s.yesbank.in/nginx:tested-alpine-hardened
LABEL author="Mandeep"
COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf
WORKDIR /usr/share/nginx/html
COPY --from=react-built /app/build/ .
RUN ls -l
