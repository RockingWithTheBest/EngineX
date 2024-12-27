# EngineX
 NGINX server


worker_processes  1;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;


    sendfile        on;
    keepalive_timeout  65;

    proxy_cache_path C:/Users/Administrator/Desktop/MERN/MERN-Application/Client/cache levels=1:2 keys_zone=my_cache:10m max_size=1g inactive=60m use_temp_path=off;    
 
    server {
        listen       80;
        server_name  localhost;


        location / {
            root   C:/Users/Administrator/Desktop/MERN/MERN-Application/Client/dist;
            index  index.html index.htm;
        }


        location /api {
            proxy_pass http://localhost:1000;  
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
