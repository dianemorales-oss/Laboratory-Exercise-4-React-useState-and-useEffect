# CvSU Mini Store - Live Search and Filter

A mini product catalog built with React and Vite for **DCIT26: Application Development and Emerging Technologies**.

This project extends **Laboratory Exercise 3: React Props and Component Communication** by adding **Laboratory Exercise 4: React `useState` and `useEffect` (Live Search and Filter)** features.

## Features

- Reusable `ProductCard` component with props, callback communication, and `children` badges
- Reusable controlled `SearchBar` component (keyword input, category dropdown, sort dropdown, and Clear Filters button)
- Live product filtering by keyword and category
- Price sorting options: Default, Price: Low to High, and Price: High to Low
- Debounced search input (300ms) using `useEffect`, `setTimeout`, and cleanup with `clearTimeout`
- Document title updates with the current number of matching products
- Result count display (`Showing X of Y products`) and friendly empty-state message
- Cart functionality from Lab 3 preserved (Add to Cart, total count, and last added item)
- Responsive layout for the search bar and product grid

## Project Structure

```text
src/
  components/
    ProductCard.jsx
    ProductCard.css
    SearchBar.jsx
    SearchBar.css
  data/
    products.js
  App.jsx
  App.css
  index.css
  main.jsx
```

## Run the Project

```bash
npm install
npm run dev
```

## Guide Questions (Laboratory Exercise 4)

### 1. Why do we use two separate states, `searchText` and `debouncedSearch`, instead of only one?

`searchText` stores what the user is typing immediately so the input remains responsive and controlled on every keystroke. `debouncedSearch` stores the delayed value that is actually used for filtering. Separating them lets the UI update instantly while filtering runs only after the user pauses typing.

### 2. What would happen if the debounce effect did not return a cleanup function?

Without a cleanup function, every keystroke would schedule a new timer but previous timers would not be canceled. Older timers could still run later and update `debouncedSearch` with stale values, causing extra renders and out-of-order or flickering filter results.

### 3. What is the purpose of the dependency array? What happens if it is empty (`[]`)? What happens if it is left out completely?

The dependency array tells React which values the effect depends on, so React knows when to re-run (and clean up) the effect. If it is empty (`[]`), the effect runs once after the initial render and its cleanup runs on unmount. If it is left out completely, the effect runs after every render, which can cause repeated work or infinite update loops if the effect updates state.

### 4. Why should we avoid sorting the original `products` array directly?

Sorting mutates the array in place. Sorting the original `products` array would permanently change the source data and can cause bugs in parts of the app that expect the original order. Creating a copy before sorting (for example, `[...filteredProducts].sort(...)`) keeps the original data unchanged and rendering predictable.

### 5. Is the filtered product list better stored in its own state, or calculated from existing state during rendering? Explain your choice.

It is better to calculate the filtered product list from existing state during rendering (`debouncedSearch`, `category`, `sortOrder`, and the original `products` data). Because it can be derived, storing it separately would duplicate data and risk getting out of sync. Computing it during render keeps a single source of truth and makes the logic simpler and more reliable.
