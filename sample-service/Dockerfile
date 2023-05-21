# Use the official Node.js LTS Alpine image as the base
FROM node:lts-alpine3.10

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install 

# Copy the rest of the application code to the working directory
COPY . .

# Expose port 80 for the HTTP service
EXPOSE 80

# Set the command to run when the container starts
CMD ["node", "index.js"]