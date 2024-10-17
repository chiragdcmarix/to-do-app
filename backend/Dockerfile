# Use a specific version of Node.js
FROM node:14-alpine3.12

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json for dependency installation
COPY backend/package*.json ./

# Install dependencies
RUN npm install --production

# Copy the rest of the application code
COPY . .

# Expose the application port
EXPOSE 6000

# Command to run the application
CMD ["node", "index.js"]
