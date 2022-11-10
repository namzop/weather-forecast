<script>
  import { FontAwesomeIcon, FontAwesomeLayers, FontAwesomeLayersText } from 'fontawesome-svelte';
  import {useState} from './hooks';
  // import { faSmileWink as fasSmileWink } from '@fortawesome/free-solid-svg-icons/index.es';
  let log = console.log
  let weather = null
  let temp = null
  let humidity = null
  let pressure = null
  let error = false
  let errorMessage = ""
  let name = ""
  let lat = null
  let lon = null
  let status = null
  let clearWeather = "https://cdn.pixabay.com/photo/2014/03/27/23/57/blue-sky-299765_960_720.jpg"
  async function getWeather() {
    const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${name}&appid=c3c867ebb06bfa2fa0ff4d4ccb43fc16&units=metric`)
    const data = await response.json()
    status = response.status
    console.log(response.status)
    // const response2 = await fetch(`http://api.openweathermap.org/geo/1.0/direct?q=${name}&limit=1&appid=c3c867ebb06bfa2fa0ff4d4ccb43fc16`)
    // const data2 = await response2.json()
    // lat = data2[0]['lat']
    // lon = data2[0]['lon']

    // const response3 = await fetch(`http://api.openweathermap.org/data/2.5/air_pollution?lat=${lat}&lon=${lon}&appid=c3c867ebb06bfa2fa0ff4d4ccb43fc16`)
    // const data3 = await response3.json()
    // console.log(data3)

    if(status == 200) {
      error = false
      temp = data["main"]["temp"]
      weather = data["weather"][0]["main"]
      humidity = data["main"]["humidity"]
      pressure = data["main"]["pressure"]
      changeIcon()
    } else {
      error = true
      temp = null
      errorMessage = data['message']
      console.log(data['message'])
    }
  }
  
  let iconWeather = "fa-solid fa-cloud"
  log(iconWeather)

  function changeIcon() {
    log(iconWeather)
    if (weather == "Rain"){
      iconWeather = "fa-solid fa-cloud-rain"
    }
    else if (weather == "Clouds") {
      return;
    }
    else if (weather == "Clear") {
      iconWeather = "fa-regular fa-sun"
    }
    else if (weather == "Smoke") {
      iconWeather = "fa-regular fa-smoke"
    }
    else if(weather == "Haze") {
      iconWeather = "fa-solid fa-sun-haze"
    }
  }
  
</script>

<svelte:head>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<script src="https://kit.fontawesome.com/6b4ef3000e.js" crossorigin="anonymous"></script>
</svelte:head>

<!-- .background{
  width: 200%
} -->

<div class="background"></div>
<!-- <body style="height:100%; width:200%;  background: url({clearWeather}) no-repeat center fixed; background-size:cover;"> -->

<span class="{iconWeather} fa-2xl rain"></span>
<!-- <span class="fa-solid {iconWeather} fa-2xl rain"></span> -->

<br>
<br>

<form on:submit|preventDefault={() => console.log("Form Submitted")}>
  <input bind:value={name} placeholder="Enter your city" type="text">
  <button on:click={getWeather} >Search</button>
</form>




{#if status==200} 
<h3>
  <br>
  <hr>
  <strong>Weather: </strong>{weather}
  <br>
  <strong>Temperature: </strong>{temp}°C
  <br>
  <strong>Pressure: </strong>{pressure}
  <br>
  <strong>Humidity: </strong>{humidity}%
</h3>
{/if}

{#if error}
<p id="errorMessage">{errorMessage}</p>
{/if}


<!-- </body>  -->