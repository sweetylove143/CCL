# Practical 3: News feed app on EC2 (Node.js + React + MongoDB)

## Goal

Deploy the provided MERN news app on Ubuntu EC2, install MongoDB on the same instance, and connect the app to the database.

## Prerequisites

- AWS account
- EC2 Ubuntu instance with public IP
- Security group inbound rules:
  - SSH (22)
  - Custom TCP (5000) from your IP (or 0.0.0.0/0 for demo only)
  - Custom TCP (5173) if you run the Vite dev server
- MobaXterm on Windows
- News API key from https://newsapi.org

## Step 1: Connect to EC2

```
ssh -i /path/to/news-key.pem ubuntu@PUBLIC_IP
```

## Step 2: Install Node.js, tools, and unzip

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl unzip git
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs build-essential
node -v
npm -v
```

## Step 3: Install MongoDB on the instance

```
sudo apt install -y gnupg
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
sudo apt update
sudo apt install -y mongodb-org
sudo systemctl enable --now mongod
sudo systemctl status mongod
```

## Step 4: Upload the project to EC2

Use MobaXterm SFTP (left file panel) to drag and drop either the zip file or the full folder into `/home/ubuntu`.

On EC2, if you uploaded the zip:

```
unzip ~/NewsAppAWS.zip -d ~/
cd ~/NewsAppAWS
```

If you uploaded the full folder, just run:

```
cd ~/NewsAppAWS
```

## Step 5: Install app dependencies

From the project root:

```
npm run install-all
```

## Step 6: Configure environment variables

Backend environment (in the backend folder):

```
cd ~/NewsAppAWS/backend
nano .env
```

Paste and save (CTRL + O, ENTER, CTRL + X):

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/newsapp
NEWS_API_KEY=YOUR_NEWSAPI_KEY
NEWS_API_URL=https://newsapi.org/v2
```

Frontend environment (in the frontend folder):

```
cd ~/NewsAppAWS/frontend
nano .env
```

Paste and save (CTRL + O, ENTER, CTRL + X):

- If you run the Vite dev server: set
  ```
  VITE_API_URL=http://PUBLIC_IP:5000/api
  ```
- If you serve the frontend from the backend (recommended): set
  ```
  VITE_API_URL=/api
  ```

## Step 7: Run the app

### Option A: Dev mode (two servers)

```
cd ~/NewsAppAWS
npm run dev
```

If you prefer manual but still one terminal:

```
cd ~/NewsAppAWS
npm run dev-backend &
npm run dev-frontend
```

Open:

```
http://PUBLIC_IP:5173
```

### Option B: Production-like (single server)

```
npm run build-frontend
npm run start-backend
```

Open:

```
http://PUBLIC_IP:5000
```

## Step 8: Verify API and features

Health check:

```
curl http://PUBLIC_IP:5000/api/health
```

Try the UI: search, filter by category, and save articles. Saved articles should appear from MongoDB.

## Notes

- Keep the backend running (use another terminal or a process manager later).
- Do not commit secrets; keep keys in environment files only.
