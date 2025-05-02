# node-docker-counter

A simple Node.js app that tracks the number of visits to a web page using Redis. This project was built to help me learn how to containerize Node.js applications and use multi-container Docker environments with Docker Compose.

---

## 🧠 What You'll Learn

- How to containerize a Node.js app
- How to use Redis as a data store
- How to use Docker Compose to orchestrate services

---

## 🗂️ Project Structure

```
node-docker-counter/
├── index.js               # Main Node.js application
├── package.json           # Dependencies
├── Dockerfile             # Instructions to build the container
├── docker-compose.yml     # Docker services config
└── README.md              # This file
```

---

## 🛠️ Prerequisites

Make sure you have the following installed:

- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/)

---

## 🚀 Getting Started

### 1. Clone the repository (or create the files manually)

```bash
git clone https://github.com/Kaylin98/node-docker-counter.git
cd node-docker-counter
```

Or manually create these files in a directory:

### 2. index.js

```js
const express = require('express');
const redis = require('redis');

const app = express();
const client = redis.createClient({
    host: 'redis-server',
    port: 6379
});

client.set('visits', 0);

app.get('/', (req, res) => {
    client.get('visits', (err, visits) => {
        res.send('Number of visits is ' + visits);
        client.set('visits', parseInt(visits) + 1);
    });
});

app.listen(8081, () => {
    console.log('Listening on port 8081');
});
```

---

### 3. package.json

```json
{
  "name": "node-docker-counter",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "redis": "^4.6.7"
  }
}
```

---

### 4. Dockerfile

```Dockerfile
FROM node:alpine

WORKDIR /app

COPY package.json .
RUN npm install

COPY . .

CMD ["node", "index.js"]
```

---

### 5. docker-compose.yml

```yaml
version: '3'
services:
  redis-server:
    image: 'redis'
  node-app:
    restart: always
    build: .
    ports:
      - "4001:8081"
```

---

## ▶️ Running the App

Run this from the root directory:

```bash
docker-compose up --build
```

Then open your browser to:  
[http://localhost:4001](http://localhost:4001)

Each refresh should increment the visit count.

---

## ✅ Expected Output

In your browser:

```
Number of visits is 1
```

On refresh:

```
Number of visits is 2
```

And so on...

---

## 📝 License

MIT
