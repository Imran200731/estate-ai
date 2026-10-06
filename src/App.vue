<script setup>
import { ref, computed } from "vue";

const searchLocation = ref("");
const propertyType = ref("All Types");
const budget = ref("Any Budget");

const showAI = ref(false);
const showContact = ref(false);
const selectedProperty = ref(null);
const savedProperties = ref([]);

const properties = [
  {
    id: 1,
    title: "Modern Hillside Villa",
    location: "Nashik, Maharashtra",
    type: "Villa",
    price: 18500000,
    priceText: "₹1.85 Cr",
    beds: 4,
    baths: 4,
    area: "3,200 sq.ft",
    image:
      "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?w=1200"
  },
  {
    id: 2,
    title: "Luxury Garden Residence",
    location: "Mumbai, Maharashtra",
    type: "House",
    price: 32500000,
    priceText: "₹3.25 Cr",
    beds: 5,
    baths: 5,
    area: "4,500 sq.ft",
    image:
      "https://images.unsplash.com/photo-1600607687920-4e2a09cf159d?w=1200"
  },
  {
    id: 3,
    title: "Skyline Premium Apartment",
    location: "Pune, Maharashtra",
    type: "Apartment",
    price: 9500000,
    priceText: "₹95 L",
    beds: 3,
    baths: 3,
    area: "1,850 sq.ft",
    image:
      "https://images.unsplash.com/photo-1600607688969-a5bfcd646154?w=1200"
  },
  {
    id: 4,
    title: "Contemporary Family Home",
    location: "Nashik, Maharashtra",
    type: "House",
    price: 12500000,
    priceText: "₹1.25 Cr",
    beds: 4,
    baths: 3,
    area: "2,600 sq.ft",
    image:
      "https://images.unsplash.com/photo-1600566753086-00f18fb6b3ea?w=1200"
  },
  {
    id: 5,
    title: "Urban Luxury Apartment",
    location: "Mumbai, Maharashtra",
    type: "Apartment",
    price: 14500000,
    priceText: "₹1.45 Cr",
    beds: 3,
    baths: 2,
    area: "1,650 sq.ft",
    image:
      "https://images.unsplash.com/photo-1600607687920-4e2a09cf159d?w=1200"
  },
  {
    id: 6,
    title: "Serene Countryside Villa",
    location: "Nashik, Maharashtra",
    type: "Villa",
    price: 21000000,
    priceText: "₹2.10 Cr",
    beds: 5,
    baths: 5,
    area: "4,100 sq.ft",
    image:
      "https://images.unsplash.com/photo-1600047509807-ba8f99d2cdde?w=1200"
  }
];

const filteredProperties = computed(() => {
  return properties.filter((property) => {
    const locationMatch =
      searchLocation.value.trim() === "" ||
      property.location
        .toLowerCase()
        .includes(searchLocation.value.toLowerCase());

    const typeMatch =
      propertyType.value === "All Types" ||
      property.type === propertyType.value;

    let budgetMatch = true;

    if (budget.value === "Under ₹1 Cr") {
      budgetMatch = property.price < 10000000;
    }

    if (budget.value === "₹1–2 Cr") {
      budgetMatch =
        property.price >= 10000000 &&
        property.price <= 20000000;
    }

    if (budget.value === "₹2–5 Cr") {
      budgetMatch =
        property.price > 20000000 &&
        property.price <= 50000000;
    }

    if (budget.value === "Above ₹5 Cr") {
      budgetMatch = property.price > 50000000;
    }

    return locationMatch && typeMatch && budgetMatch;
  });
});

function searchProperties() {
  document
    .getElementById("properties")
    ?.scrollIntoView({
      behavior: "smooth"
    });
}

function resetSearch() {
  searchLocation.value = "";
  propertyType.value = "All Types";
  budget.value = "Any Budget";
}

function toggleSave(propertyId) {
  if (savedProperties.value.includes(propertyId)) {
    savedProperties.value =
      savedProperties.value.filter(
        (id) => id !== propertyId
      );
  } else {
    savedProperties.value.push(propertyId);
  }
}

function openProperty(property) {
  selectedProperty.value = property;
}

function closeProperty() {
  selectedProperty.value = null;
}

function scrollToSection(id) {
  document
    .getElementById(id)
    ?.scrollIntoView({
      behavior: "smooth"
    });
}
</script>

<template>
  <div class="app">

    <!-- NAVBAR -->
    <nav class="navbar">

      <div class="nav-container">

        <button
          class="logo"
          @click="scrollToSection('home')"
        >
          <span class="logo-mark">E</span>
          <span>
            ESTATE <b>AI</b>
          </span>
        </button>

        <div class="nav-links">

          <button @click="scrollToSection('home')">
            Home
          </button>

          <button @click="scrollToSection('properties')">
            Properties
          </button>

          <button @click="scrollToSection('about')">
            About
          </button>

          <button @click="scrollToSection('contact')">
            Contact
          </button>

        </div>

        <button
          class="ai-button"
          @click="showAI = true"
        >
          ✦ AI Assistant
        </button>

      </div>

    </nav>


    <!-- HERO -->
    <section
      id="home"
      class="hero"
    >

      <div class="hero-container">

        <div class="hero-content">

          <span class="badge">
            ✦ SMARTER REAL ESTATE SEARCH
          </span>

          <h1>
            Find a place
            <span>you'll love.</span>
          </h1>

          <p>
            Discover extraordinary homes with intelligent
            property search and personalized recommendations.
          </p>


          <!-- SEARCH -->
          <div class="search-box">

            <div class="search-field">

              <label>
                LOCATION
              </label>

              <input
                v-model="searchLocation"
                type="text"
                placeholder="Nashik, Mumbai..."
                @keyup.enter="searchProperties"
              />

            </div>


            <div class="search-field">

              <label>
                PROPERTY TYPE
              </label>

              <select v-model="propertyType">

                <option>
                  All Types
                </option>

                <option>
                  Villa
                </option>

                <option>
                  House
                </option>

                <option>
                  Apartment
                </option>

              </select>

            </div>


            <div class="search-field">

              <label>
                BUDGET
              </label>

              <select v-model="budget">

                <option>
                  Any Budget
                </option>

                <option>
                  Under ₹1 Cr
                </option>

                <option>
                  ₹1–2 Cr
                </option>

                <option>
                  ₹2–5 Cr
                </option>

                <option>
                  Above ₹5 Cr
                </option>

              </select>

            </div>


            <button
              class="primary-button search-button"
              type="button"
              @click="searchProperties"
            >
              🔎 Search
            </button>

          </div>


          <!-- STATS -->
          <div class="hero-stats">

            <div>
              <strong>
                10K+
              </strong>

              <span>
                Properties
              </span>
            </div>

            <div>
              <strong>
                5K+
              </strong>

              <span>
                Happy Clients
              </span>
            </div>

            <div>
              <strong>
                50+
              </strong>

              <span>
                Cities
              </span>
            </div>

          </div>

        </div>


        <!-- HERO IMAGE -->
        <div class="hero-visual">

          <div class="image-card">

            <img
              src="https://images.unsplash.com/photo-1600585154340-be6161a56a0c?w=1400"
              alt="Modern luxury house"
            />

            <div class="property-overlay">

              <div>

                <small>
                  FEATURED PROPERTY
                </small>

                <h3>
                  Modern Hillside Villa
                </h3>

                <p>
                  📍 Nashik, Maharashtra
                </p>

              </div>

              <strong>
                ₹1.85 Cr
              </strong>

            </div>

          </div>


          <div class="floating-card">

            <span>
              ✨
            </span>

            <div>

              <strong>
                AI Match
              </strong>

              <p>
                98% compatible
              </p>

            </div>

          </div>

        </div>

      </div>

    </section>


    <!-- PART 2 CONTINUES BELOW -->
    <!-- FEATURED PROPERTIES -->
    <section
      id="properties"
      class="properties"
    >

      <div class="section-heading">

        <div>
          <span class="section-label">
            EXPLORE
          </span>

          <h2>
            Featured properties
          </h2>
        </div>

        <button
          class="view-all"
          type="button"
          @click="resetSearch"
        >
          View all →
        </button>

      </div>


      <div class="property-grid">

        <div
          v-for="property in filteredProperties"
          :key="property.id"
          class="property-card"
          @click="openProperty(property)"
        >

          <div class="property-image">

            <img
              :src="property.image"
              :alt="property.title"
            />

            <button
              class="save-button"
              type="button"
              @click.stop="toggleSave(property.id)"
            >
              {{
                savedProperties.includes(property.id)
                  ? "♥"
                  : "♡"
              }}
            </button>

            <span class="property-type">
              {{ property.type }}
            </span>

          </div>


          <div class="card-content">

            <h3>
              {{ property.title }}
            </h3>

            <p class="location">
              📍 {{ property.location }}
            </p>


            <div class="card-bottom">

              <strong>
                {{ property.priceText }}
              </strong>

              <span>
                {{ property.beds }} Beds ·
                {{ property.baths }} Baths
              </span>

            </div>

          </div>

        </div>


        <!-- NO RESULTS -->
        <div
          v-if="filteredProperties.length === 0"
          class="no-results"
        >

          <div class="no-results-icon">
            🔎
          </div>

          <h3>
            No properties found
          </h3>

          <p>
            Try changing your location, property type,
            or budget.
          </p>

          <button
            class="primary-button"
            type="button"
            @click="resetSearch"
          >
            Reset Search
          </button>

        </div>

      </div>

    </section>


    <!-- AI SECTION -->
    <section
      id="about"
      class="ai-section"
    >

      <div class="ai-container">

        <div class="ai-content">

          <span class="badge">
            ✦ POWERED BY AI
          </span>

          <h2>
            Your intelligent
            <span>property advisor.</span>
          </h2>

          <p>
            Estate AI understands what you're looking for
            and helps you discover properties that actually
            match your lifestyle, budget, and preferences.
          </p>


          <div class="ai-features">

            <div class="ai-feature">

              <span>
                ✨
              </span>

              <div>
                <h3>
                  Smart Recommendations
                </h3>

                <p>
                  Get properties matched to your requirements.
                </p>
              </div>

            </div>


            <div class="ai-feature">

              <span>
                ⚡
              </span>

              <div>
                <h3>
                  Faster Search
                </h3>

                <p>
                  Find suitable homes without endless searching.
                </p>
              </div>

            </div>


            <div class="ai-feature">

              <span>
                🔒
              </span>

              <div>
                <h3>
                  Trusted Choices
                </h3>

                <p>
                  Compare properties before making decisions.
                </p>
              </div>

            </div>

          </div>


          <button
            class="primary-button"
            type="button"
            @click="showAI = true"
          >
            Try AI Assistant →
          </button>

        </div>


        <div class="ai-visual">

          <div class="ai-card">

            <div class="ai-card-header">

              <span>
                ✦
              </span>

              <div>
                <strong>
                  Estate AI
                </strong>

                <small>
                  Property Assistant
                </small>
              </div>

            </div>


            <div class="chat-message user-message">
              I need a modern villa in Nashik under ₹2 Cr.
            </div>


            <div class="chat-message ai-message">
              Great choice! I found several properties
              that match your requirements.
            </div>


            <div class="match-card">

              <div class="match-icon">
                🏡
              </div>

              <div>

                <strong>
                  98% Match
                </strong>

                <span>
                  Modern Hillside Villa
                </span>

              </div>

            </div>

          </div>

        </div>

      </div>

    </section>


    <!-- CONTACT -->
    <section
      id="contact"
      class="contact-section"
    >

      <div class="contact-container">

        <div>

          <span class="section-label">
            GET IN TOUCH
          </span>

          <h2>
            Ready to find your
            next home?
          </h2>

          <p>
            Talk to our team and let Estate AI help you
            find the right property.
          </p>

        </div>


        <button
          class="primary-button contact-button"
          type="button"
          @click="showContact = true"
        >
          Contact us →
        </button>

      </div>

    </section>


    <!-- FOOTER -->
    <footer class="footer">

      <div class="footer-container">

        <div class="footer-brand">

          <button
            class="logo"
            type="button"
            @click="scrollToSection('home')"
          >
            <span class="logo-mark">
              E
            </span>

            <span>
              ESTATE <b>AI</b>
            </span>
          </button>

          <p>
            Smarter property discovery powered by AI.
          </p>

        </div>


        <div class="footer-links">

          <button
            type="button"
            @click="scrollToSection('home')"
          >
            Home
          </button>

          <button
            type="button"
            @click="scrollToSection('properties')"
          >
            Properties
          </button>

          <button
            type="button"
            @click="scrollToSection('about')"
          >
            About
          </button>

          <button
            type="button"
            @click="scrollToSection('contact')"
          >
            Contact
          </button>

        </div>

      </div>


      <div class="footer-bottom">
        © 2026 Estate AI. All rights reserved.
      </div>

    </footer>


    <!-- PROPERTY MODAL -->
    <div
      v-if="selectedProperty"
      class="modal-overlay"
      @click.self="closeProperty"
    >

      <div class="modal">

        <button
          class="modal-close"
          type="button"
          @click="closeProperty"
        >
          ×
        </button>

        <img
          :src="selectedProperty.image"
          :alt="selectedProperty.title"
        />

        <div class="modal-content">

          <span class="property-type">
            {{ selectedProperty.type }}
          </span>

          <h2>
            {{ selectedProperty.title }}
          </h2>

          <p>
            📍 {{ selectedProperty.location }}
          </p>

          <h3>
            {{ selectedProperty.priceText }}
          </h3>

          <div class="modal-details">

            <span>
              🛏 {{ selectedProperty.beds }} Beds
            </span>

            <span>
              🛁 {{ selectedProperty.baths }} Baths
            </span>

            <span>
              📐 {{ selectedProperty.area }}
            </span>

          </div>

          <button
            class="primary-button"
            type="button"
            @click="showContact = true; closeProperty()"
          >
            Contact about this property
          </button>

        </div>

      </div>

    </div>


    <!-- AI MODAL -->
    <div
      v-if="showAI"
      class="modal-overlay"
      @click.self="showAI = false"
    >

      <div class="ai-modal">

        <button
          class="modal-close"
          type="button"
          @click="showAI = false"
        >
          ×
        </button>

        <div class="ai-modal-icon">
          ✦
        </div>

        <h2>
          Estate AI Assistant
        </h2>

        <p>
          Tell us what kind of property you're looking for
          and we'll help you narrow down your choices.
        </p>

        <input
          type="text"
          placeholder="Example: 3BHK villa in Nashik under ₹2 Cr"
        />

        <button
          class="primary-button"
          type="button"
          @click="showAI = false"
        >
          Find My Property →
        </button>

      </div>

    </div>


    <!-- CONTACT MODAL -->
    <div
      v-if="showContact"
      class="modal-overlay"
      @click.self="showContact = false"
    >

      <div class="contact-modal">

        <button
          class="modal-close"
          type="button"
          @click="showContact = false"
        >
          ×
        </button>

        <span class="section-label">
          CONTACT
        </span>

        <h2>
          Let's find your property.
        </h2>

        <p>
          Leave your details and our team will get
          in touch with you.
        </p>


        <form
          @submit.prevent="showContact = false"
        >

          <input
            type="text"
            placeholder="Your name"
            required
          />

          <input
            type="email"
            placeholder="Your email"
            required
          />

          <input
            type="tel"
            placeholder="Phone number"
            required
          />

          <textarea
            placeholder="Tell us what you're looking for..."
            rows="4"
          ></textarea>

          <button
            class="primary-button"
            type="submit"
          >
            Send Request →
          </button>

        </form>

      </div>

    </div>

  </div>
</template>