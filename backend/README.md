# Pokédex Backend API

This is the backend server for the Pokédex application. It's built with Node.js and Express, and it handles all communication with the PokéAPI while providing a caching layer to improve performance.

## What It Does

The backend acts as a middleware between the frontend and PokéAPI. When you request information about a Pokémon, the server first checks if it has that data cached in memory. If it does and the cache entry hasn't expired, it returns the cached data immediately. If not, it fetches fresh data from PokéAPI, transforms it into a cleaner format, caches it for future requests, and then sends it back to you.

The cache uses an LRU algorithm, which means it keeps the 100 most recently accessed Pokémon in memory. Each entry expires after 10 minutes to ensure the data stays reasonably fresh. This approach significantly reduces the load on PokéAPI and makes repeated searches much faster.

## Installation

Navigate to the backend folder and install the required packages:

```bash
cd backend
npm install
```

## Running the Server

To start the server in production mode:

```bash
npm start
```

For development with automatic restarts when you change the code:
```bash
npm run dev
```

The server will start on port 3001. You should see a message confirming it's running and showing the cache configuration.

## API Endpoints

### Fetching Pokémon Data

Send a GET request to /api/pokemon/:name where :name is the Pokémon's name. The name should be lowercase, but the server will handle case conversion automatically.

Example using curl:
```bash
curl http://localhost:3001/api/pokemon/pikachu
```

The response includes the Pokémon's name, ID number, sprite images (official artwork, default, and shiny versions), types, abilities with indicators for hidden abilities, base stats, height and weight, base experience points, the first five moves, and a description from the Pokédex. There's also a "cached" field that tells you whether this data came from the cache or was freshly fetched.


## How the Cache Works

The cache implementation is in cache.js. It maintains a Map of Pokémon data with each entry containing the transformed data and an expiry timestamp. When you request a Pokémon, the cache checks if the entry exists and hasn't expired. If it has expired, it's automatically deleted.

When adding new entries, the cache checks if it's at maximum capacity (100 entries). If it is, it removes the least recently used entry before adding the new one. The Map's insertion order naturally maintains LRU ordering since we delete and re-add entries when they're accessed.

Each cached entry includes an expiry time calculated as the current time plus the TTL (10 minutes). This per-entry expiration allows for automatic cleanup without needing a background process.



