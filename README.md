<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DROPDASH - Store Your Bags, Explore Freely</title>
  <style>
    /* Basic Reset */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    /* Body Styling */
    body {
      font-family: Arial, sans-serif;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #f2f5f7;
      color: #333;
    }

    /* Center Container */
    .container {
      width: 100%;
      max-width: 400px;
      padding: 20px;
      background-color: #ffffff;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      border-radius: 8px;
    }

    /* Common Styling */
    .section {
      display: none;
    }
    .active {
      display: block;
    }
    .brand-name {
      font-size: 2.5em;
      font-weight: bold;
      color: #007BFF;
      margin-bottom: 10px;
      text-align: center;
    }
    .tagline {
      font-size: 1.1em;
      text-align: center;
      color: #555;
      margin-bottom: 20px;
    }
    .quote {
      font-size: 1.2em;
      font-style: italic;
      text-align: center;
      color: #777;
      margin-bottom: 30px;
    }
    .button {
      width: 100%;
      padding: 12px;
      font-size: 1em;
      color: #fff;
      background-color: #007BFF;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      margin-top: 20px;
      transition: background-color 0.3s;
    }
    .button:hover {
      background-color: #0056b3;
    }
    .dropdown, .input-field, .input-signup {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 4px;
    }

    .section-spacing {
      margin-bottom: 20px;
    }

    /* Responsive Design */
    @media (max-width: 600px) {
      .brand-name {
        font-size: 2em;
      }
      .button {
        font-size: 0.9em;
      }
    }
  </style>
</head>
<body>
  <div class="container" role="main">
    <!-- Home Section -->
    <section id="home" class="section active">
      <div class="brand-name">DROPDASH</div>
      <div class="tagline">Store Your Bags, Explore Freely.</div>
      
      <!-- Motivational Quote -->
      <div id="quote" class="quote"></div>
      
      <!-- How It Works Section -->
      <div class="section-spacing">
        <h3>How It Works</h3>
        <ul>
          <li>Choose your city and select a storage location.</li>
          <li>Pick the date and time for storage.</li>
          <li>Confirm your booking and store your luggage securely.</li>
          <li>Explore your city without carrying heavy bags!</li>
        </ul>
      </div>

      <!-- Pricing Section -->
      <div class="section-spacing">
        <h3>Pricing</h3>
        <p>₹50 per hour per bag</p>
        <p>₹300 per day per bag</p>
        <p>Special discounts for weekly storage.</p>
      </div>

      <!-- Buttons to Sign Up or Login -->
      <button class="button" onclick="navigateTo('login')">Sign In</button>
      <button class="button" onclick="navigateTo('signup')" style="background-color: #555; margin-top: 10px;">Sign Up</button>
    </section>

    <!-- Login Section -->
    <section id="login" class="section">
      <h2>Login</h2>
      <input type="email" id="login-email" class="input-signup" placeholder="Email" aria-label="Enter your email" required>
      <input type="password" id="login-password" class="input-signup" placeholder="Password" aria-label="Enter your password" required>
      <button class="button" onclick="login()">Login</button>
      <button class="button" onclick="navigateTo('home')" style="background-color: #555; margin-top: 10px;">Back to Home</button>
    </section>

    <!-- Sign Up Section -->
    <section id="signup" class="section">
      <h2>Sign Up</h2>
      <input type="email" id="signup-email" class="input-signup" placeholder="Email" aria-label="Enter your email" required>
      <input type="password" id="signup-password" class="input-signup" placeholder="Password" aria-label="Enter your password" required>
      <button class="button" onclick="signup()">Sign Up</button>
      <button class="button" onclick="navigateTo('home')" style="background-color: #555; margin-top: 10px;">Back to Home</button>
    </section>

    <!-- Booking Section -->
    <section id="booking" class="section">
      <h2>Select Your City</h2>
      <select id="city-select" class="dropdown" aria-label="City Selection" onchange="loadLocations()">
        <option value="">Select a City</option>
        <option value="delhi">Delhi</option>
        <option value="mumbai">Mumbai</option>
        <option value="bangalore">Bangalore</option>
        <option value="chennai">Chennai</option>
        <option value="kolkata">Kolkata</option>
        <option value="pune">Pune</option>
        <option value="hyderabad">Hyderabad</option>
        <option value="jaipur">Jaipur</option>
        <option value="kanpur">Kanpur</option>
      </select>
      
      <div id="places-to-explore"></div> <!-- Display places to explore in the selected city -->

      <h2>Select Location</h2>
      <select id="location-select" class="dropdown" aria-label="Location Selection">
        <option value="">Select a Location</option>
      </select>
      
      <h2>Select Date and Time</h2>
      <input type="date" id="date-select" class="input-field" aria-label="Date Selection">
      <input type="time" id="time-select" class="input-field" aria-label="Time Selection">

      <button class="button" onclick="bookStorage()">Book Storage</button>
      <p id="confirmation-message" style="margin-top: 15px; color: #007BFF;"></p>
      <button class="button" onclick="navigateTo('home')" style="background-color: #555; margin-top: 10px;">Back to Home</button>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="section">
      <h2>Contact Us</h2>
      <p>Email: support@dropdash.com</p>
      <p>Phone: +91-12345-67890</p>
      <button class="button" onclick="navigateTo('home')" style="background-color: #555; margin-top: 10px;">Back to Home</button>
    </section>
  </div>

  <script>
    // Dummy locations data for each city
    const cityLocations = {
      delhi: ["Connaught Place", "Karol Bagh", "Saket"],
      mumbai: ["Colaba", "Andheri", "Bandra"],
      bangalore: ["MG Road", "Indiranagar", "Koramangala"],
      chennai: ["T Nagar", "Guindy", "Velachery"],
      kolkata: ["Park Street", "Salt Lake", "Esplanade"],
      pune: ["Camp", "Koregaon Park", "Viman Nagar"],
      hyderabad: ["Banjara Hills", "HITEC City", "Jubilee Hills"],
      jaipur: ["Bapu Bazaar", "C-Scheme", "Jaipur Railway Station"],
      kanpur: ["The Mall Road", "Chunniganj", "Swaroop Nagar"]
    };

    // Main places to explore in each city
    const cityAttractions = {
      delhi: ["Red Fort", "Qutub Minar", "India Gate", "Humayun's Tomb", "Lotus Temple"],
      mumbai: ["Gateway of India", "Marine Drive", "Chhatrapati Shivaji Terminus", "Elephanta Caves", "Juhu Beach"],
      bangalore: ["Bangalore Palace", "Cubbon Park", "Lalbagh Botanical Garden", "Vidhana Soudha", "Bannerghatta National Park"],
      chennai: ["Marina Beach", "Fort St. George", "Kapaleeshwarar Temple", "Government Museum", "Elliot's Beach"],
      kolkata: ["Victoria Memorial", "Howrah Bridge", "Indian Museum", "Dakshineswar Kali Temple", "Kalighat Temple"],
      pune: ["Shaniwar Wada", "Aga Khan Palace", "Sinhagad Fort", "Pune University", "Osho Ashram"],
      hyderabad: ["Charminar", "Golconda Fort", "Hussain Sagar", "Ramoji Film City", "Qutub Shahi Tombs"],
      jaipur: ["Amer Fort", "Hawa Mahal", "City Palace", "Jantar Mantar", "Jaipur Zoo"],
      kanpur: ["Jajmau", "Kanpur Memorial Church", "Green Park", "Phool Bagh", "Moti Jheel"]
    };

    // Function to display a random motivational quote
    function displayQuote() {
      const quotes = [
        "Start where you are. Use what you have. Do what you can.",
        "Success is the sum of small efforts, repeated day in and day out.",
        "Your limitation—it’s only your imagination.",
        "Push yourself, because no one else is going to do it for you.",
        "Great things never come from comfort zones."
      ];
      const randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
      document.getElementById("quote").textContent = `"${randomQuote}"`;
    }

    // Function to navigate between sections
    function navigateTo(sectionId) {
      const sections = document.querySelectorAll('.section');
      sections.forEach(section => {
        section.classList.remove('active');
      });

      const activeSection = document.getElementById(sectionId);
      if (activeSection) {
        activeSection.classList.add('active');
      }
    }

    // Function to load locations based on city selection
    function loadLocations() {
      const city = document.getElementById("city-select").value;
      const locationSelect = document.getElementById("location-select");
      const placesToExplore = document.getElementById("places-to-explore");
      
      locationSelect.innerHTML = '<option value="">Select a Location</option>';
      placesToExplore.innerHTML = ''; // Reset the places list

      if (cityLocations[city]) {
        // Load city locations
        cityLocations[city].forEach(location => {
          const option = document.createElement("option");
          option.value = location;
          option.textContent = location;
          locationSelect.appendChild(option);
        });

        // Load tourist places for the selected city
        const attractionsList = cityAttractions[city];
        const attractionsHeading = document.createElement('h3');
        attractionsHeading.textContent = "Main Places to Explore";
        placesToExplore.appendChild(attractionsHeading);

        const list = document.createElement('ul');
        attractionsList.forEach(attraction => {
          const listItem = document.createElement('li');
          listItem.textContent = attraction;
          list.appendChild(listItem);
        });
        placesToExplore.appendChild(list);
      }
    }

    // Handle booking storage
    function bookStorage() {
      const city = document.getElementById("city-select").value;
      const location = document.getElementById("location-select").value;
      const date = document.getElementById("date-select").value;
      const time = document.getElementById("time-select").value;
      const message = document.getElementById("confirmation-message");

      if (!city || !location || !date || !time) {
        message.textContent = "Please complete all booking details.";
      } else {
        const mapsLink = `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(location + ', ' + city)}`;
        message.innerHTML = `Storage booked successfully at ${location}, ${city} on ${date} at ${time}.<br><a href="${mapsLink}" target="_blank">View Location on Google Maps</a>`;
      }
    }

    // Simple login function
    function login() {
      const email = document.getElementById("login-email").value;
      const password = document.getElementById("login-password").value;
      if (email && password) {
        alert("Logged in successfully!");
        navigateTo('booking');
      } else {
        alert("Please enter both email and password.");
      }
    }

    // Simple signup function
    function signup() {
      const email = document.getElementById("signup-email").value;
      const password = document.getElementById("signup-password").value;
      if (email && password) {
        alert("Sign Up successful!");
        navigateTo('login');
      } else {
        alert("Please enter both email and password.");
      }
    }

    // Initial function call to display a quote on page load
    displayQuote();
  </script>
</body>
</html>

