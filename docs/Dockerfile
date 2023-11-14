FROM sphinxdoc/sphinx as sphinx

# Install custom theme + build HTML
WORKDIR /opt/sphinx/
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
RUN make clean html

# Copy HTML to NGINX
FROM nginx
WORKDIR /usr/share/nginx/html/
RUN rm -rf ./*
COPY --from=sphinx /opt/sphinx/build/html .

