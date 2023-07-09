
# Forex Market Sentiment Website

This project aims to create a website that displays forex market sentiment using Vue.js and Bootstrap. The website will fetch data from a public API and provide features such as refreshing data every 60 minutes, displaying pair currency views, selecting currencies for showing market sentiment, and storing the last 5 selected currencies as favorites.

## Technologies Used

- Vue.js: A JavaScript framework for building user interfaces.
- Bootstrap: A CSS framework for responsive and mobile-first web development.
- Axios: A JavaScript library for making HTTP requests.

## Step-by-Step Implementation

1. **Set up the Development Environment**
   - Install Node.js on your computer.
   - Use Node Package Manager (npm) to install Vue CLI.

2. **Create a New Vue Project**
   - Use Vue CLI to create a new Vue project, specifying the desired configuration options.

3. **Install Additional Dependencies**
   - Install Bootstrap and Axios using npm.

4. **Configure Bootstrap and SCSS**
   - Import the Bootstrap CSS in the main.js file of the Vue project.

5. **Create Vue Components**
   - Create the necessary Vue components to display forex market sentiment and handle user interactions.
   - Implement logic to fetch data from the public API using Axios.
   - Set up a timer to refresh the data every 60 minutes.
   - Use computed properties to filter and display the pair currency view.
   - Implement a dropdown or other input element to select the currency for showing market sentiment.
   - Use local storage to store the user's last 5 selected currencies as favorites.

6. **Update the App Component and Router**
   - Update the main App.vue component to include routing and the necessary child components.

7. **Run the Development Server**
   - Start the development server to see the website in action.

## Project Structure

```
├── src
│   ├── components
│   │   ├── ForexMarketSentiment.vue      # Component for displaying forex market sentiment
│   │   └── ...                           # Other Vue components
│   ├── main.js                           # Entry point of the application
│   └── ...
├── package.json                          # Project dependencies and scripts
└── ...
```

## Conclusion

By following the step-by-step implementation guide, you can create a website using Vue.js and Bootstrap that displays forex market sentiment. The website will periodically fetch data from a public API, allow users to select currencies, and provide a list of favorite currencies. This project serves as an educational example for integrating Vue.js, Bootstrap, and Axios to build a data-driven web application.