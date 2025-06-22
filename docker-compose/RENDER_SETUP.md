# Setting Up Tiledesk on Render

This guide will help you set up Tiledesk on Render using Docker Compose.

## Prerequisites

1. A Render account (https://render.com)
2. A MongoDB database (you can use MongoDB Atlas free tier)
3. An OpenAI API key (if you want to use GPT actions)

## Step 1: Set Up MongoDB

Since Tiledesk requires MongoDB, you'll need to set up a MongoDB database. The easiest option is to use MongoDB Atlas:

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and create a free account
2. Create a new cluster (the free tier is sufficient for testing)
3. Set up a database user with a username and password
4. Configure network access (you can allow access from anywhere for testing)
5. Get your connection string, which will look like: `mongodb+srv://username:password@cluster.mongodb.net/tiledesk`

## Step 2: Configure Environment Variables

1. Copy the `.env.template` file to `.env`:
   ```bash
   cp .env.template .env
   ```

2. Edit the `.env` file and fill in your specific values:
   - `EXTERNAL_BASE_URL`: Your Render service URL (e.g., https://tiledesk.onrender.com)
   - `EXTERNAL_MQTT_BASE_URL`: Same as above but with wss:// protocol (e.g., wss://tiledesk.onrender.com)
   - `MONGODB_URI`: Your MongoDB connection string
   - `OPENAI_API_KEY`: Your OpenAI API key (if you want to use GPT actions)

## Step 3: Deploy to Render

1. Log in to your Render account
2. Create a new "Web Service"
3. Connect your GitHub repository or upload the files directly
4. Configure the service:
   - Name: tiledesk
   - Environment: Docker
   - Docker Command: `docker-compose -f docker-compose/render-docker-compose.yml up`
   - Add all the environment variables from your `.env` file

5. Click "Create Web Service"

## Step 4: Verify the Deployment

1. Once the deployment is complete, visit your Render service URL
2. You should see the Tiledesk login page
3. Sign in with the default admin credentials:
   - Email: admin@tiledesk.com
   - Password: superadmin

## Ports Used by Tiledesk

Tiledesk uses the following ports, which are automatically exposed by the Docker Compose configuration:

- 3000: Tiledesk Server API
- 4200: Web Widget
- 4500: Dashboard
- 8082: Ionic Chat
- 8004: Chat HTTP Server
- 8081: Proxy (main entry point)
- 5672, 15672, 1883, 15675: RabbitMQ
- 6379: Redis

## Troubleshooting

If you encounter issues:

1. Check the Render logs for any error messages
2. Verify that your MongoDB connection string is correct
3. Ensure all required environment variables are set
4. Check that the ports are not being blocked by any firewall

## Additional Configuration

For additional configuration options, refer to the main Tiledesk documentation:
https://github.com/Tiledesk/tiledesk/blob/master/docker-compose/README.md