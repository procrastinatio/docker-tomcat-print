
GeoAdmin - Basic Docker Tomcat Example 
===========================

Simple Docker Tomcat example to host the legacy print-server-main.war.

### Clone this project

You can clone this project:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
git clone https://github.com/procrastinatio/docker-tomcat-print.git
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Build an image using the cloned project

cd into the directory of the cloned GitHub project

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker build -t procrastinatio/docker-tomcat-print:latest .
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Push the created image to your Docker Hub repository

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker login
docker push procrastinatio/docker-tomcat-print:latest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Get it from Docker GitHub

https://hub.docker.com/r/procrastinatio/docker-tomcat-print/

### Run the container on a Docker-enabled Linux host

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker run -p 9090:8080 -d --name tomcat  <your-username>/docker-tomcat-print:latest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Access the sample application

You can access the sample application on this URL:

    http://<host-ip>:9090/service-print-main/

### Get the print info

    http://<host-ip>:9090/service-print-main/pdf/info.json

### Create a PDF

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
curl 'http://localhost:9090/service-print-main/pdf/create.json' -H 'Content-Type: application/json' \
   -H 'Referer: http://service-print.dev.bgdi.ch/service-print-main/' --data @data.json
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With `data.json`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
{
    "layout": "1 A4 landscape",
    "title": "A simple example",
    "srs": "EPSG:21781",
    "units": "m",
    "outputFilename": "mapfish-print",
    "outputFormat": "pdf",
    "layers": [{
        "layer": "ch.swisstopo.pixelkarte-farbe",
        "opacity": 1,
        "type": "WMTS",
        "baseURL": "https://wmts.geo.admin.ch",
        "maxExtent": [420000, 30000, 900000, 350000],
        "tileOrigin": [420000, 350000],
        "tileSize": [256, 256],
        "resolutions": [4000, 3750, 3500, 3250, 3000, 2750, 2500, 2250, 2000, 1750, 1500, 1250, 1000, 750, 650, 500, 250, 100, 50, 20, 10, 5, 2.5, 2, 1.5],
        "zoomOffset": 0,
        "version": "1.0.0",
        "requestEncoding": "REST",
        "formatSuffix": "jpeg",
        "style": "default",
        "dimensions": ["TIME"],
        "params": {
            "TIME": "current"
        },
        "matrixSet": "21781"
    }, {
        "type": "WMS",
        "opacity": 0.5,
        "format": "image/png",
        "layers": ["ch.swisstopo.geologie-eiszeit-lgm"],
        "baseURL": "http://wms.geo.admin.ch"
    }],
    "pages": [{
        "center": [660000, 190000.00000000006],
        "bbox": [518536.1111111112, 96513.8888888889, 801463.8888888889, 283486.1111111112],
        "display": [802, 530],
        "scale": "1000000.0",
        "dpi": 150,
        "rotation": 0,
        "qrcodeurl": "https://api3.geo.admin.ch/qrcodegenerator?url=https%3A%2F%2Fmap.geo.admin.c…ity%3Dfalse%2Cfalse%2Cfalse%2Cfalse%26layers_timestamp%3D18641231%2C%2C%2C",
        "dataOwner": "© swisstopo,wms.geo.admin.ch",
        "mapTitle": "First map",
        "langfr": true,
        "thirdPartyDataOwner": false,
        "shortLink": "https://s.geo.admin.ch/6f8db9b085"
    }]
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Access the logs

You can use this simple command to check the catalina logs of the Tomcat container

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker logs tomcat
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Check the files inside the container

You can run this command to enter the container and check the files under the webapps directory

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker exec -it tomcat bash
ls -lrt /usr/local/tomcat/webapps
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

