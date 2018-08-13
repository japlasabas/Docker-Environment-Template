This is a simple set of instructions on how you can create your docker environment for your local development:
1. First remove the git link from your root directory (go to your git bash and inside your docker environment direcotry and type rm -rf .git)
2. Clone your service from a repository inside the root directory and create a Dockerfile then copy the contents of the Dockerfile inside the "service-folder" folder and replace the marked placeholder values with your service name.
3. Remember that the folder directory of your cloned repository shall be your service name defined inside the docker-compose.yml
4. Create a vhost for your newly cloned service by creating a new vhost.conf file (name should be prefixed with the service folder name e.g. f3-api-vhost.conf) and save it in ".nginx" folder directory
5. Copy the contents from "service-template-vhost.conf" to your newly created vhost config file and replace all the values of the marked placeholder (all {{SERVICE_NAME}} variables) with the appropriate service name
6. Modify the Dockerfile inside ".nginx" directory and follow the instructions stated inside (The instructions are to copy the created vhost file from your local machine to the container machine upon running)
7. Modify your docker-compose.yml and replace all marked placeholder values (all {{SERVICE_NAME}} variables) with the appropriate service name
8. Once done with the above steps, open your terminal and go to the directory of your docker environment and run "docker-compose up --build -d"


Notes: 
 - Please make sure to replace all placeholder variables, it will also be easier to name your folder the same with the service name defined in your docker-compose.yml as a convention.
 - Inside your docker-compose.yml file, you'll see a block of code inside "environment", this is where you'll defined the values of your .env file
 - To access your running container, type in "docker ps" and copy the CONTAINER_ID of the container that you want to access the type "docker exec -it {{CONTAINER_ID}} bash" (this is where you can run commands for your phpunit and composer updates)
