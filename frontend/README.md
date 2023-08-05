# Kittygram - a wall for posting cat photos.

### Project Description:

Kittygram allows users to share and show off photos of their beloved cats. Registered users can create, view, edit, and delete their posts.

### Project Setup:

- Clone the repository:

    ```bash
    git clone git@github.com:maxuver/kittygram_final.git
    ```
    ```bash
    cd kittygram
    ```
 - Create a .env file and fill it with your data:

    ```bash
   # Database Secrets
    POSTGRES_USER=[database_username]
    POSTGRES_PASSWORD=[database_password]
    POSTGRES_DB=[database_name]
    DB_PORT=[database_connection_port]
    DB_HOST=[db]

   # Django Secrets
   SECRET_KEY='SECRET_KEY'
   DEBUG=False
   ALLOWED_HOSTS='your_domain'
    ```

### Creating Docker Images:

1. Replace 'username' with your DockerHub login:

    ```bash
    cd frontend
    docker build -t username/kittygram_frontend .
    cd ../backend
    docker build -t username/kittygram_backend .
    cd ../nginx
    docker build -t username/kittygram_gateway . 
    ```

2. Push the images to DockerHub:

    ```bash
    docker push username/kittygram_frontend
    docker push username/kittygram_backend
    docker push username/kittygram_gateway
    ```
  
### Deployment on a Remote Server:

1. Connect to the remote server:

    ```bash
    ssh -i path_to_SSH/SSH_name username@server_ip 
    ```

2. Create a 'kittygram' directory on the server through the terminal:

    ```bash
    mkdir kittygram
    ```

3. Install docker-compose on the server:

    ```bash
    sudo apt update
    sudo apt install curl
    curl -fSL https://get.docker.com -o get-docker.sh
    sudo sh ./get-docker.sh
    sudo apt-get install docker-compose-plugin
    ```

4. Copy docker-compose.production.yml and .env files to the 'kittygram/' directory:

    ```bash
    scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
    ```

5. Run docker compose in daemon mode:

    ```bash
    sudo docker-compose -f docker-compose.production.yml up -d
    ```

6. Perform migrations, collect backend static files, and copy them to '/backend_static/static/':

    ```bash
    sudo docker-compose -f docker-compose.production.yml exec backend python manage.py migrate
    sudo docker-compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker-compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
    ```

7. Open the Nginx configuration file in the nano editor:

    ```bash
    sudo nano /etc/nginx/sites-enabled/default
   
    ```

8. Add location settings to the server section:

    ```bash
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
    ```

9. Test the configuration and reload Nginx:

    ```bash
    sudo nginx -t 
    sudo service nginx reload
    ```
 
### Link to the Deployed Application:

- #### https://kittygram.myvnc.com
 
### Technologies and Necessary Tools:

- Docker
- GitHub Actions
- Postgres
- Nginx
- Gunicorn
- Python 3.9
- Django (backend)
- React (frontend)
- Node.js
- Git

Author of the project | My website
------------- | -------------
[maxuver](https://github.com/maxuver) | [maximpatsyuk.com](https://maximpatsyuk.com)
