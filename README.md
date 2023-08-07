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
 - Create a .env file ```py -3.9 -m venv venv```
 - Activate ```source venv/scripts/activate```
   
 - Fill it with your data

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

8. Add location settings to the server section or copy already working config from [taski-docker app](https://github.com/maxuver/taski-docker/blob/main/README.md):

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

### pytest

Install pytest in virtual enviroment ```pip install pytest```

Run ```pytest``` and you'll see output

 ```==================================================================================== test session starts =====================================================================================
platform win32 -- Python 3.11.0, pytest-7.4.0, pluggy-1.2.0 -- C:\Users\Honor\OneDrive\Рабочий стол\uver files\yandex_practicum_python\DJango backend\15 sprint Docker\kittygram_finai\kittygram_final\venv\Scripts\python.exe
rootdir: C:\Users\Honor\OneDrive\Рабочий стол\uver files\yandex_practicum_python\DJango backend\15 sprint Docker\kittygram_final
configfile: pytest.ini
testpaths: tests/
collected 12 items

tests/test_connection.py::test_link_connection[taski_domain] PASSED                                                                                                                     [  8%]
tests/test_connection.py::test_link_connection[kittygram_domain] PASSED                                                                                                                 [ 16%]
tests/test_connection.py::test_projects_on_same_ip PASSED                                                                                                                               [ 25%]
tests/test_connection.py::test_kittygram_static_is_available PASSED                                                                                                                     [ 33%]
tests/test_connection.py::test_kittygram_api_available PASSED                                                                                                                           [ 41%]
tests/test_dockerhub_images.py::test_dockerhub_images_exist PASSED                                                                                                                      [ 50%]
tests/test_files.py::test_infra_files_exist PASSED                                                                                                                                      [ 58%]
tests/test_files.py::test_deploy_info_file_content PASSED                                                                                                                               [ 66%]
tests/test_files.py::test_backend_dockerfile_exists PASSED                                                                                                                              [ 75%]
tests/test_files.py::test_backend_dokerfile_content PASSED                                                                                                                              [ 83%]
tests/test_files.py::test_workflow_file PASSED                                                                                                                                          [ 91%]
tests/test_files.py::test_requirements_location PASSED                                                                                                                                  [100%] 

=============================================================================== 12 passed, 2 warnings in 4.93s ===============================================================================
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
- Pytest
- Yandex Cloud

Author of the project | My website
------------- | -------------
[maxuver](https://github.com/maxuver) | [maximpatsyuk.com](https://maximpatsyuk.com)
