<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' : ''
    "
  >
    <main>
     <div class="search-box">
        <input class="checkbox" type="checkbox" id="checkbox" />
        <div class="formContainer">
            <input
              class="form__input"
              type="email"
              v-model="query"
              @keypress="fetchweather"
            />
            <label
              class="form__toggle"
              for="checkbox"
              data-title="Search.."
            ></label>
          
        </div>
      </div>

      <div class="weather-warp" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api: "c30bd77797199cb9e1b66a5c33108f67",
      url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchweather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url}weather?q=${this.query}&units=metric&APPID=${this.api}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      let days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-image: url("./assets/cold-bgi.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bgi.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.checkbox {
  display: none;
}
.checkbox:checked + .formContainer {
  width: 100%;
}
.checkbox:checked + .formContainer .form__toggle {
  visibility: hidden;
  opacity: 0;
  transform: scale(0.7);
}
.checkbox:checked + .formContainer .form__input,
.checkbox:checked + .formContainer .form__buttonLabel {
  transition: 0.2s 0.1s;
  visibility: visible;
  opacity: 1;
  transform: scale(1);
}
.checkbox:not(:checked)
  + .formContainer
  .form__input:required:valid
  ~ .form__toggle::before,
.checkbox:checked
  + .formContainer
  .form__input:required:valid
  ~ .form__toggle::before {
  content: "Thank You! \1F60A";
}
.checkbox:not(:checked)
  + .formContainer
  .form__input:required:valid
  ~ .form__toggle {
  pointer-events: none;
  cursor: default;
}

.formContainer,
.form__toggle {
  width: 10em;
  height: 3.25em;
}

.formContainer {
  position: relative;
  font-weight: 700;
}

.form__toggle {
  position: absolute;
  border-radius: 3.25em;
  background-color: #ffffff;
  transition: 0.2s;
}

.form__toggle {
  color: darkgray;
  top: 0;
  cursor: pointer;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
.form__toggle::before {
  font-size: 20px;
  content: attr(data-title);
}

.form__input {
  font: inherit;
  border: 0;
  outline: 0;
  border-radius: 3.25em;
  box-sizing: border-box;
}

.form__input {
  font-size: 1.75em;
  opacity: 0;
  visibility: hidden;
  transform: scale(0.7);
  transition: 0s;
}

.form__input {
  color: darkgray;
  height: 100%;
  width: 100%;
  padding: 0 0.714em;
}
.form__input::placeholder {
  color: currentColor;
}
.form__input:required:valid {
  color: darkgray;
}
.form__input:required:valid + .form__buttonLabel {
  color: darkgray;
}
.form__input:required:valid + .form__buttonLabel::before {
  pointer-events: initial;
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  text-align: center;
}
.weather-warp .location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 100;
  text-align: center;
  font-style: italic;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
