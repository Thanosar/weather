<template>
    <div id="app" :class="weather.main && weather.weather[0].main === 'Rain' ? 'rain' : ''">
        <main>
            <div class="search-box">
                <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather"/>
            </div>

            <div class="error" v-if="err">
                {{err}}
            </div>

            <div class="weather-wrap" v-if="weather.main">
                <div class="location-box">
                    <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
                    <div class="date">{{date}}</div>
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
        name: 'Weather-App',
        data() {
            return {
                apiKey: '13b2b9e599041193155bd29a9521f934',
                urlBase: 'https://api.openweathermap.org/data/2.5/',
                query: '',
                date: new Date().toLocaleDateString("en-US", {
                    weekday: 'long',
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric'
                }),
                weather: {},
                err: ''
            }
        },
        methods: {
            fetchWeather(e) {
                if (e.key !== 'Enter') {
                    return;
                }
                fetch(`${this.urlBase}weather?q=${this.query}&units=metric&APPID=${this.apiKey}`)
                    .then(res => {
                        return res.json()
                    }).then(this.setWeather)
            },
            setWeather(weather) {
                this.err = "";
                if (weather.cod === "404") {
                    this.err = weather.message;
                }
                this.weather = weather;
            }
        }
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'Montserrat', sans-serif;
    }

    #app {
        background-image: url("https://images.pexels.com/photos/1480807/pexels-photo-1480807.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940");
        background-size: cover;
        background-position: bottom;
        transition: 0.4s;
    }

    #app.rain {
      background-image: url("https://images.pexels.com/photos/4275890/pexels-photo-4275890.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940");
    }

    main {
        min-height: 100vh;
        padding: 25px;
        background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
    }

    .search-box {
        width: 100%;
        margin-bottom: 150px;
    }

    .search-box .search-bar {
        display: block;
        width: 100%;
        padding: 15px;
        color: #313131;
        font-size: 20px;
        appearance: none;
        border: none;
        outline: none;
        box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.5);
        border-radius: 0 16px 0 16px;
        transition: 0.4s;
    }

    .search-box .search-bar:focus {
        box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.75);
        border-radius: 16px 0 16px 0;
    }

    .location-box .location {
        color: #FFF;
        font-size: 25px;
        font-weight: 500;
        text-align: center;
        text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
        margin-bottom: 10px;
    }

    .error {
        color: #ff0005;
        font-size: 25px;
        font-weight: 500;
        text-align: center;
        text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }

    .location-box .date {
        color: #FFF;
        font-size: 20px;
        font-weight: 300;
        font-style: italic;
        text-align: center;
    }

    .weather-box {
        text-align: center;
    }

    .weather-box .temp {
        display: inline-block;
        padding: 10px 25px;
        color: #FFF;
        font-size: 102px;
        font-weight: 900;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.25);
        border-radius: 16px;
        margin: 30px 0px;
        box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

    .weather-box .weather {
        color: #FFF;
        font-size: 48px;
        font-weight: 700;
        font-style: italic;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
</style>
