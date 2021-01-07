FROM nginx:stable AS nginx_test
WORKDIR /usr/share/nginx/html
RUN echo -n "Long running process..." \
    && sleep 10 \
    && echo " done"
RUN echo "It works!" > index.html
