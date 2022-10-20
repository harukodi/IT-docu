# Dockerbuild example with nginx

## Create a directory for the dockerbuild file and subdirectories
```bash
mkdir nginx-webserver
```


## Cd in to the directory
```bash
cd nginx-webserver
```


## Create a dockerfile
```bash
nano Dockerfile
```


## Paste the following content in to the dockerfile
```Dockerfile
FROM nginx

COPY content /usr/share/nginx/html

COPY conf /etc/nginx
```


## Now create the subfolders content and conf
```bash
mkdir content && mkdir conf
```


## Build the docker image
```bash
Docker build -t nameOfImage .
```


## Lets run the conntainer with the following command
```bash
Docker run --name nameOfYourContainer -d -p 8080:80 nameOfImage
```


**Your index.html/style.css should be stored in the content folder if you want to manipulate the image build.**