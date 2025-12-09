# Links Fetch Engine

A powerful web crawler and link extraction engine built with Node.js, TypeScript, and Express. This engine efficiently crawls websites and extracts all internal links using a depth-first search algorithm.

## 🚀 Features

- **Automated Link Extraction**: Crawls websites and extracts all links systematically
- **Depth-First Search (DFS) Algorithm**: Uses an efficient stack-based DFS approach for traversing web pages
- **RESTful API**: Simple HTTP API for easy integration
- **TypeScript**: Built with TypeScript for type safety and better developer experience
- **Error Handling**: Robust error handling for failed requests and unreachable pages
- **Duplicate Prevention**: Automatically tracks visited links to prevent redundant crawling
- **CORS Enabled**: Can be accessed from any origin
- **Security**: Implements Helmet.js for basic security headers

## 📋 Table of Contents

- [How It Works](#how-it-works)
- [Algorithm Explanation](#algorithm-explanation)
- [Installation](#installation)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Development](#development)

## 🔍 How It Works

The Links Fetch Engine operates as a web crawler that systematically visits web pages and extracts links. Here's the high-level workflow:

1. **Start Point**: The crawler receives a root URL as input
2. **Fetching**: It fetches the HTML content of the page using HTTP requests
3. **Parsing**: The HTML is parsed using Cheerio (jQuery-like library) to extract all `<a>` tags
4. **Link Extraction**: From each anchor tag, the `href` attribute is extracted
5. **Traversal**: The crawler follows each discovered link recursively
6. **Tracking**: All visited and discovered links are tracked to avoid revisiting
7. **Result**: Returns a comprehensive list of all links found within the website

## 🧮 Algorithm Explanation

The engine uses a **Depth-First Search (DFS)** algorithm implemented with a stack data structure to crawl websites efficiently. Here's how it works:

### Algorithm Overview

```
1. Initialize two lists:
   - listOfLinks: Stores all discovered links
   - visitedLinks: Acts as a stack for links to be visited (DFS stack)

2. Start with the root URL:
   - Fetch the root page
   - Extract all links from the page
   - Add them to both listOfLinks and visitedLinks

3. While visitedLinks stack is not empty:
   - Pop a link from visitedLinks (LIFO - Last In First Out)
   - Visit that link
   - Extract all new links from the page
   - Add new links to both lists
   - Continue until no new links are discovered

4. Return the complete listOfLinks
```

### Algorithm Characteristics

- **Type**: Depth-First Search (DFS)
- **Data Structure**: Stack (using array with pop operation)
- **Time Complexity**: O(V + E) where V is the number of pages and E is the number of links
- **Space Complexity**: O(V) for storing visited links
- **Traversal Order**: Goes deep into each branch before backtracking

### Why DFS?

The DFS algorithm is chosen because:
- **Memory Efficient**: Only needs to store the current path in the stack
- **Complete**: Will eventually visit all reachable pages
- **Simple**: Easy to implement and understand
- **Works Well for Web Crawling**: Naturally follows the link structure of websites

### Example Flow

```
Start: https://example.com

Step 1: Visit https://example.com
Found: [/about, /contact, /blog]
Stack: [/blog, /contact, /about]

Step 2: Pop /about, Visit https://example.com/about
Found: [/team, /history]
Stack: [/history, /team, /blog, /contact]

Step 3: Pop /history, Visit https://example.com/about/history
Found: [/timeline]
Stack: [/timeline, /team, /blog, /contact]

... continues until stack is empty
```

## 📦 Installation

### Prerequisites

- Node.js (v12 or higher)
- npm or yarn

### Steps

1. Clone the repository:
```bash
git clone https://github.com/theQuarky/links-fetch-engine.git
cd links-fetch-engine
```

2. Install dependencies:
```bash
npm install
```

3. Build the TypeScript code:
```bash
npm run build
```

## 🎯 Usage

### Development Mode

Run the server in development mode with auto-reload:
```bash
npm run dev
```

### Production Mode

Build and run in production:
```bash
npm run prod
```

The server will start on port `4000` by default.

## 📡 API Documentation

### Base URL
```
http://localhost:4000/v1
```

### Endpoints

#### 1. Get Links

Crawl a website and extract all links.

**Endpoint**: `GET /v1/getlinks`

**Query Parameters**:
- `root` (required): The root URL of the website to crawl

**Example Request**:
```bash
curl "http://localhost:4000/v1/getlinks?root=https://example.com"
```

**Example Response**:
```json
{
  "links": [
    "/",
    "/about",
    "/contact",
    "/blog",
    "/about/team",
    "/about/history",
    "/blog/post-1",
    "/blog/post-2"
  ]
}
```

#### 2. Welcome Endpoint

Check if the API is running.

**Endpoint**: `GET /v1/`

**Example Request**:
```bash
curl http://localhost:4000/v1/
```

**Example Response**:
```json
{
  "links": "Welcome to web link crawller api"
}
```

## 📁 Project Structure

```
links-fetch-engine/
├── src/
│   ├── api/
│   │   └── index.ts          # API routes and crawling logic
│   ├── config/
│   │   └── config.ts         # Configuration settings
│   ├── helpers/
│   │   └── errorHandler.ts  # Error handling middleware
│   ├── App.ts                # Express app setup
│   └── index.ts              # Entry point
├── dist/                     # Compiled JavaScript (generated)
├── package.json              # Project dependencies
├── tsconfig.json             # TypeScript configuration
└── README.md                 # This file
```

## 🛠️ Technologies Used

- **Node.js**: JavaScript runtime
- **TypeScript**: Type-safe JavaScript
- **Express.js**: Web framework
- **Axios**: HTTP client for making requests
- **Cheerio**: HTML parsing and manipulation (jQuery-like)
- **Helmet**: Security middleware
- **CORS**: Cross-Origin Resource Sharing
- **Morgan**: HTTP request logger

## 💻 Development

### Available Scripts

- `npm run dev` - Run in development mode with hot reload
- `npm run build` - Compile TypeScript to JavaScript
- `npm run start` - Start the compiled application
- `npm run prod` - Build and start in production mode
- `npm run clean` - Remove generated files and dependencies

### Configuration

The server configuration can be modified in `src/config/config.ts`:

```typescript
const CONFIG = {
    PORT: 4000  // Change this to use a different port
}
```

## ⚠️ Important Notes

- The crawler respects the structure of the website but does not implement robots.txt checking
- Be mindful of rate limiting when crawling large websites
- The current implementation focuses on extracting links, not the content of pages
- External links (links to other domains) are included in the results but not recursively crawled

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

## 📝 License

ISC

## 👨‍💻 Author

theQuarky

---

**Note**: This is a web crawler for educational and development purposes. Always respect website terms of service and robots.txt files when crawling websites.
