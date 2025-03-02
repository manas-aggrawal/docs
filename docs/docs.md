# 📘 Crypto Dashboard Documentation  

This document provides a complete guide on setting up, integrating APIs, managing state, and handling challenges while building the **Crypto Dashboard** using **Next.js**.  

---

## 🛠️ Project Setup Guide  

### 📌 **How to Run the Crypto Dashboard Web App?**  

To set up and run the **Crypto Dashboard**, follow these steps:

### 🔹 **1. Clone the Repository**  
Cloning the project ensures you have the latest codebase:  
```sh
git clone https://github.com/your-username/crypto-dashboard.git
cd crypto-dashboard
```
### 🔹 **2. Install Dependencies** 
```
npm install
```

### 🔹 **3. Start Development Server** 
```
npm run dev
```

### 🔹 **4. Building for prod** 
```
npm run build
npm start
```

## 🛠️ API Integration Details

### 📌 How Data is fetched & updated?
I have used CoinGecko API to fetch real-time cryptocurrency prices.

### 🔹 Fetching data from API
I send an API request to CoinGecko to get prices of 5 cryptocurrencies.

#### Why CoinGecko?
Reliable: It provides accurate market prices.
Free tier: Suitable for our project needs.

### 📌 Updating data
I used react query to keep data updated. It automatically refetches and refreshed data at interval of 1 minute.

#### Manual refresh
You can also manually refresh data by clicking the button "Refresh Prices" which makes the same API call to the CoinGecko API which I have described earlier.

## State Management
#### Why I chose react query?
I chose this instead of Redux or Zustand because it has the following:
1. Automatic Caching - Doesn't make API calls for the given 1 minute interval.
2. Background Fetching - fetches the latest prices automatically after every 1 minute in the background.
3. Error Handling - handles error/loading states automatically

#### How I did it?
```
const { data, isLoading, error } = useQuery({
  queryKey: ["cryptoPrices"],
  queryFn: fetchCryptoPrices,
});
```

#### Why is this better than Redux?
Redux stores the state but does not handle API caching.
React Query provides built-in caching, retries failed requests, and fetches data only when needed.

#### Why not Zustand?
Zustand is great for global state but not for server-state management like API calls.

## Challenges and Solutions
1. Problem: I encountered a `useQuery` error because React Query needs `QueryClientProvider` to work properly but I didn't add that initially.
   1. So for the solution I wrapped the app with `QueryClientProvider`. It ensures all components can access the query state.
2. Problem: Using the `useState`, `useQuery` inside server caused error.
   1. So, as a solution I marked `use Client` at the top of the `dashboard.tsx` file.