# Pokedex - Pokémon Search Application

This is a full-stack web application that lets you search for Pokémon and view detailed information about them. The project uses a Node.js backend with Express to handle API requests and a React frontend with Vite for the user interface. It includes smart caching to make repeated searches faster and has a clean, modern design that works on all devices.

## What This Project Does

The application connects to the PokéAPI to fetch Pokémon data and displays it in an easy-to-read format. When you search for a Pokémon, the backend retrieves the information and stores it temporarily in memory. If you search for the same Pokémon again within 10 minutes, it loads instantly from the cache instead of making another API call.

### Backend Features

The backend is built with Express and handles all the communication with PokéAPI. It includes a custom LRU (Least Recently Used) cache that stores up to 100 Pokémon at a time. Each cached entry expires after 10 minutes to keep the data fresh. The server also transforms the raw API data into a cleaner format that's easier to work with on the frontend. It handles errors gracefully, whether it's a network issue, an invalid Pokémon name, or the API being temporarily unavailable. CORS is enabled so the frontend can communicate with the backend, and request logging helps with debugging during development.


## Getting Started

Before you start, make sure you have Node.js version 16 or higher installed on your computer. You'll also need npm, which comes with Node.js.

### Running the Backend

First, navigate to the backend folder and install the dependencies. Then start the server.

```bash
cd backend
npm install
npm start
```

The backend server will start on port 3001. You should see a message confirming it's running.

### Running the Frontend

Open a new terminal window (keep the backend running in the first one). Navigate to the frontend folder, install its dependencies, and start the development server.

```bash
cd frontend
npm install
npm run dev
```

The frontend will start on port 5173. Open your web browser and go to http://localhost:5173 to use the application.


## How the API Works

The backend exposes a REST API that the frontend uses to get Pokémon data.

### Main Endpoint

To get information about a Pokémon, send a GET request to /api/pokemon/:name where :name is the Pokémon's name in lowercase.

Example using curl:
```bash
curl http://localhost:3001/api/pokemon/pikachu
```


There are also two utility endpoints. You can check the cache status at /api/cache/stats, which returns how many items are currently cached and the cache configuration. There's also a health check endpoint at /health to verify the server is running.




### Cache Implementation

The cache uses an LRU (Least Recently Used) algorithm, which means when it reaches its maximum capacity of 100 entries, it automatically removes the oldest unused entry to make room for new ones. Each entry stays in the cache for 10 minutes before expiring. This balances performance with data freshness since Pokémon data doesn't change frequently.

### Technologies Used

The backend runs on Node.js with Express handling the HTTP server and routing. It uses node-fetch to make requests to PokéAPI and includes CORS middleware to allow cross-origin requests from the frontend. Morgan provides request logging to help with debugging.

The frontend is built with React 18 and uses Vite as the build tool, which provides extremely fast hot module replacement during development. All styling is done with pure CSS, no framework required. The design includes smooth transitions and animations for a polished feel. The UI adapts to different screen sizes using responsive design techniques with CSS media queries.

### Design Inspiration

The UI design draws inspiration from modern card-based interfaces and clean dark/light theme implementations:
- CSS Variables for theming: https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties
- Modern card design patterns: https://www.smashingmagazine.com/2016/11/css-grids-flexbox-and-box-alignment-our-new-system-for-web-layout/

The color scheme and component styling were carefully chosen to provide good contrast and readability in both light and dark modes.

## Local Development

The frontend already includes hot module replacement by default when you run npm run dev, so any changes you make to the code will instantly appear in the browser without a full reload.



