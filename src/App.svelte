<script>
  import "./App.css";
  let api_key = 'YOUR_API_KEY';
  let url_base = 'https://api.openweathermap.org/data/2.5/';
  let query = '';
  let weather = {};

  async function fetchWeather(event) {
    if (event.key === 'Enter') {
      try {
        const res = await fetch(`${url_base}weather?q=${query}&units=metric&APPID=${api_key}`);
        const data = await res.json();
        weather = data;
      } catch (error) {
        console.error(error);
      }
    }
  }

  function dateBuilder() {
    const d = new Date();
    const months = [
      'January',
      'February',
      'March',
      'April',
      'May',
      'June',
      'July',
      'August',
      'September',
      'October',
      'November',
      'December'
    ];
    const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

    const day = days[d.getDay()];
    const date = d.getDate();
    const month = months[d.getMonth()];
    const year = d.getFullYear();

    return `${day} ${date} ${month} ${year}`;
  }
</script>

<style>
  #app {
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
  }
</style>

<main id="app" class={typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''}>
  <div class="min-h-screen py-25 bg-gradient-to-b">
    <br />
    <div class="w-full flex justify-center items-center h-full">
      <input
        type="text"
        class="block w-1/2 py-3 px-3 text-lg text-gray-800 rounded-full shadow-md bg-opacity-50 hover:bg-opacity-75 focus:outline-none focus:bg-opacity-75"
        placeholder="Search..."
        bind:value={query}
        on:keypress={fetchWeather}
      />
    </div>
    <br />

    {#if typeof weather.main !== 'undefined'}
      <div class="text-center">
        <div class="text-white text-2xl font-medium text-center text-shadow-lg">
          {weather.name}, {weather.sys.country}
        </div>
        <div class="text-white text-lg font-italic text-center">{dateBuilder()}</div>
      </div>

      <div class="text-center">
        <div class="inline-block py-10 px-25 text-7xl font-bold text-white text-shadow-lg bg-opacity-25 rounded-full shadow-md">
          {Math.round(weather.main.temp)}°c
        </div>
        <div class="text-white text-4xl font-bold italic text-shadow-lg">
          {weather.weather[0].main}
        </div>
      </div>
    {/if}
  </div>
</main>