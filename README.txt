
KrishEMitra Chatbot and Website
================================

KrishEMitra is a comprehensive platform designed to assist farmers with crop information, weather updates, and disease management strategies. The platform includes a chatbot integrated with multilingual capabilities and a dynamic website to provide actionable insights for farmers.

Features
--------

### Chatbot:
- Multilingual support in **English**, **Hindi**, and **Kannada**.
- Offers guidance on:
  - Real-time weather forecasts.
  - Best practices for cultivating crops like rice, wheat, and others.
  - Pest and disease management strategies.
- Easy-to-use interface for farmers with regional language support.

### Website:
- Developed using **HTML**, **CSS**, and **JavaScript**.
- Includes pages like `about-us.html` and an interactive homepage.
- Mobile-friendly and optimized for various devices.

### Weather Updates:
- Integrates a **weather API** for real-time weather forecasts.
- Displays temperature, precipitation chances, and other key weather metrics.

Getting Started
---------------

### Prerequisites
- A **Weather API key** from a provider like OpenWeather or WeatherStack.
- **Node.js** installed on your system.
- A text editor or IDE (e.g., Visual Studio Code) for editing the source code.

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/tanmays2k6/KrishEMitra.git
   cd KrishEMitra
   ```

2. Install the dependencies for the backend:
   ```bash
   npm install express axios body-parser dotenv
   ```

3. Set up the directory structure:
   ```
   KrishEMitra/
   ├── backend/
   │   └── chatbot.js
   ├── public/
   │   ├── about-us.html
   │   ├── index.html
   │   └── style.css
   ├── README.md
   └── package.json
   ```

Configuring the Weather API
---------------------------

1. Sign up for a Weather API service 
2. Obtain your API key.
3. Create a `.env` file in the root directory and add your API key:
   ```plaintext
   WEATHER_API_KEY=your_weather_api_key
   ```

4. Update the chatbot backend file (`backend/chatbot.js`) to include weather data:
   ```javascript
   const express = require('express');
   const bodyParser = require('body-parser');
   const axios = require('axios');
   require('dotenv').config();

   const app = express();
   app.use(bodyParser.json());

   const WEATHER_API_URL = "https://api.openweathermap.org/data/2.5/weather";

   app.post('/chat', async (req, res) => {
       const userMessage = req.body.message.toLowerCase();
       let botResponse = '';

       if (userMessage.includes('weather') || userMessage.includes('rain')) {
           try {
               const response = await axios.get(WEATHER_API_URL, {
                   params: {
                       q: 'Bangalore', // Replace with user location if available
                       appid: process.env.WEATHER_API_KEY,
                       units: 'metric',
                   },
               });
               const weather = response.data;
               botResponse = `The current weather in ${weather.name} is ${weather.main.temp}°C with ${weather.weather[0].description}.`;
           } catch (error) {
               botResponse = 'Sorry, I was unable to fetch the weather information at the moment. Please try again later.';
           }
       } else if (userMessage.includes('rice') || userMessage.includes('paddy')) {
           botResponse = 'Rice cultivation requires well-drained soil and consistent water supply. Plant during monsoon (June-July). Watch out for common diseases like Rice Blast and Brown Spot.';
       } else {
           botResponse = 'Thank you for your question about farming. Could you please provide more details about your query?';
       }

       res.json({ reply: botResponse });
   });

   app.listen(3000, () => {
       console.log('Chatbot server is running on http://localhost:3000');
   });
   ```

Running the Project
-------------------

1. Start the backend server:
   ```bash
   node backend/chatbot.js
   ```

2. Serve static files:
   - Ensure the website is hosted via the backend server using `express.static()`:
     ```javascript
     app.use(express.static('public'));
     ```
   - Open `http://localhost:3000` in your browser.

3. Test the chatbot:
   - Ask questions about weather, crops, or pests in English, Hindi, or Kannada.

Troubleshooting
---------------

### Error: `Cannot GET /about-us.html`
- Ensure the `about-us.html` file is correctly placed in the `/public` folder.
- Verify the static file serving configuration in the backend:
  ```javascript
  app.use(express.static('public'));
  ```

### Error: Weather Data Not Displayed
- Check if the Weather API key is correctly added in the `.env` file.
- Ensure the API endpoint URL and query parameters are correct.

Contributions
-------------

You can contribute by:
- Adding more crops or pest-related information.
- Improving the user interface of the website.
- Extending support for additional regional languages.

License
-------

This project is open-source and licensed under the MIT License.
