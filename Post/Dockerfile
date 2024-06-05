# Use an official Node runtime as a parent image
FROM node:14

# Set the working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# Expose the port the app runs on
EXPOSE 6210

# Define command to run the app
CMD ["node", "index.js"]
