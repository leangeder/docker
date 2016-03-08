FROM alpine
RUN apk -U add git py-pip
RUN git clone https://github.com/leangeder/website.git /var/www/
RUN pip install -r /var/www/requirements.txt
CMD python /var/www/app.py
