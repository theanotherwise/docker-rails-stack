FROM httpd:2.4.46

RUN echo 'ProxyPass "/" "http://rails:3000/"' >> conf/extra/proxy-html.conf
RUN echo 'ProxyPassReverse "/" "http://rails:3000/"' >> conf/extra/proxy-html.conf

RUN sed -i "s/#LoadModule proxy_module/LoadModule proxy_module/g" conf/httpd.conf
RUN sed -i "s/#LoadModule proxy_html_module/LoadModule proxy_html_module/g" conf/httpd.conf
RUN sed -i "s/#LoadModule proxy_http_module/LoadModule proxy_http_module/g" conf/httpd.conf