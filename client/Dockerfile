# tag build phase with name. temporary container to create build directory
FROM node:alpine as builder 
WORKDIR '/app'
COPY package.json ./
RUN npm install
# dont need volumes since wont be making changes to code on the fly in production
#can copy all files over to working directory
COPY . .
RUN npm run build
# outputs build folder to /app/build <-- stuff we care about serving in production

#run phase. get just what you need (build directory) from previous container
#any block can only have single FROM statement
FROM nginx
#hey expose port 3000 when running this container. elastic beanstalk will look for EXPOSE instruction for container port to expose to incoming traffic
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
 # copy something over from a previous phase
COPY --from=builder /app/build /usr/share/nginx/html
#nginx image comes with default startup command to start nginx so we dont have to include one