FROM fluent/fluentd:v1.16-debian-1
USER root
RUN apt update && apt install curl -y
#RUN gem uninstall -I elasticsearch && gem install elasticsearch -v 8.7.0
RUN ["gem", "install", "elasticsearch", "--no-document", "--version", "8.7.0"]
RUN ["gem", "install", "fluent-plugin-elasticsearch", "--no-document", "--version", "5.3.0"]
RUN ["gem" , "install" , "fluent-plugin-http-healthcheck"]
USER fluent
