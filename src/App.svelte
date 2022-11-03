<script>
  let log = console.log
  let weather = null
  let temp = null
  let humidity = null
  let pressure = null
  let error = false
  let errorMessage = ""
  let name = ""
  let status = null
  async function getWeather() {
    const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${name}&appid=c3c867ebb06bfa2fa0ff4d4ccb43fc16&units=metric`)
    const data = await response.json()
    status = response.status
    console.log(response.status)
    if(status == 200) {
      error = false
      temp = data["main"]["temp"]
      weather = data["weather"][0]["main"]
      humidity = data["main"]["humidity"]
      pressure = data["main"]["pressure"]
    } else {
      error = true
      temp = null
      errorMessage = data['message']
      console.log(data['message'])
    }
  }
  
</script>
<input bind:value={name} type="text">
<button on:click={getWeather} >Search</button>



{#if status==200} 
<br>
<hr>
<strong>Weather: </strong>{weather}
<br>
<strong>Temperature: </strong>{temp}°C
<br>
<strong>Pressure: </strong>{pressure}
<br>
<strong>Humidity: </strong>{humidity}%
{/if}

{#if error}
<p id="errorMessage">{errorMessage}</p>
{/if}