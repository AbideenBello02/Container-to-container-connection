In this exercise the app is hosted on a container and the mongodb is hosted on another container.
The database container was started using "docker run -d --name mondodb mongo" from cli.
The mongodb container ip address was referenced in the app.js file.

Another approach instead of referencing the container ip is by creating a container network and  
during runtime e.g 
1. crewate network: "$docker network create my-network".
2. then run: 
"$docker run -d --name myapp --network my-network myapp_image" to run the application container. 
"$docker run -d --name mongo-database --network my-network mongo" to run mongodb container.
note: Since containers in the same network are able to communicate, we just need to adjust connnection
line in the app.js from:
    mongoose.connect(
      'mongodb://172.17.0.2:27017/swfavorites',
to: 
    mongoose.connect(
  'mongodb://mongo-database:27017/swfavorites', #replacing the ip address with the container name.

Address resolution will be handled automatically by docker.