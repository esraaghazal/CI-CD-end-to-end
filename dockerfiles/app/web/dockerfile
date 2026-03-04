FROM nginx:latest

# remove default conf file and put our new configrations 
RUN rm -rf /etc/nginx/conf.d/default.conf
COPY ./nginvproapp /etc/nginx/conf.d/vproapp.conf