FROM busybox:latest
RUN mkdir /www 
COPY ./public-html /www
EXPOSE 9080
CMD ["httpd", "-f", "-p", "9080", "-h", "/www"]