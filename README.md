
GeoAdmin - Basic Docker Tomcat Example 
===========================

This is the most basic Docker Tomcat example to demonstrate how an image is built using a Dockerfile that copies a sample Java WAR file. Once you build your own Tomcat image, you can push it to your Docker Hub repository and then run a container using this image on any Linux host that is Docker-enabled.

### Clone this project

You can clone this project:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
git clone https://github.com/procrastinatio/docker-tomcat-print.git
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Build an image using the cloned project

cd into the directory of the cloned GitHub project

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker build -t <your-username>/docker-tomcat-print:latest .
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Sign up for an account on Docker Hub and create a public tomcat repository

-   Sign up for a free account on Docker Hub – <a href="https://hub.docker.com/">***https://hub.docker.com/***</a>

-   Create a public repository called “tomcat”

### Push the created image to your Docker Hub repository

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
docker login
docker push <your-username>/docker-tomcat-print:latest
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
curl 'http://localhost:9090/service-print-main/pdf/create.json' -H 'Origin: http://service-print.dev.bgdi.ch' -H 'Content-Type: application/json'  -H 'Referer: http://service-print.dev.bgdi.ch/service-print-main/' --data @data.json
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With `data.json`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 {
     layout: '1 A4 landscape',
     title: 'A simple example',
     srs: 'EPSG:4326',
     units: 'dd',
     outputFilename: 'mapfish-print',
     outputFormat: 'pdf',
     layers: [{
         type: 'WMS',
         format: 'image/png',
         layers: ['ch.swisstopo.geologie-eiszeit-lgm'],
         baseURL: 'http://wms.geo.admin.ch'
     }],
     pages: [{
         center: [7.5, 46.5],
         scale: 1000000.0,
         dpi: 150,
         rotation: 0,
         qrcodeurl: "https://api3.geo.admin.ch/qrcodegenerator?url=https%3A%2F%2Fmap.geo.admin.ch%2F%3Ftopic%3Dech%26lang%3Dfr%26bgLayer%3Dch.swisstopo.pixelkarte-farbe%26layers%3Dch.swisstopo.zeitreihen%2Cch.bfs.gebaeude_wohnungs_register%2Cch.bav.haltestellen-oev%2Cch.swisstopo.swisstlm3d-wanderwege%26layers_visibility%3Dfalse%2Cfalse%2Cfalse%2Cfalse%26layers_timestamp%3D18641231%2C%2C%2C",
         dataOwner: "swisstopo",
         mapTitle: "First map",
         comment: "The \"routes\" layer is not shown (the scale is too small)",
         data: [{
             id: 1,
             name: 'blah',
             icon: 'icon_pan'
         }, {
             id: 2,
             name: 'blip',
             icon: 'icon_zoomin'
         }]
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

