<template>
  <!-- 온도가 16이상인경우 클래스명에 warm 아니면 없음 -->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp>16 ?'warm' : ''">
    <main>
      <div class="search-box">
        <!-- v-model="query"  query에 입력된 값을 데이터에 넣을수 있다-->
        <!-- v-on:keypress="fetchWeather"  	키보드의 키를 눌렀다가 놓았을 때 실행함-->
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
      </div>
      <!-- weather.main 값이 있으면 출력 -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
         <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}℃</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <p>{{ weather.weather[0].description }}</p>
        </div>
      </div>
    </main>
  </div>

</template>

<script>
export default {
  name: 'App',
  data(){
    return{
      api_key:'c86fd0853397293be3c5dd0c262e2951',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
    
  },
  //fetch(url, options) 첫번째 인자로 URL, 두번째 인자로 옵션 객체
  //API 호출이 성공했을 경우에는 응답(response) 객체를 resolve하고, 실패했을 경우에는 예외(error) 객체를 reject
  //fetch(url, options)
  //.then((response) => console.log("response:", response))
  //.catch((error) => console.log("error:", error));
  //fetch("https://api.openweathermap.org/data/2.5/")
  //.then((response) => response.json())
  //.then((data) => console.log(data));
  //이 메서드를 호출하면, 응답(response) 객체로 부터 JSON 포멧의 응답 전문을 자바스크립트 객체로 변환하여 얻을 수 있습니다.
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            console.log(res)
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    
    //오늘날짜를 가져오는 함수
    dateBuilder(){
      // new Date() 현재의 날짜와 시간을 나타내는 문자열을 반환
      // 함수 호출시 new를 추가하면 Date() 새로운 객체를 생성함 
      let d = new Date();
      
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      console.log(d);

      return `${day} ${date} ${month} ${year}`

    }

  }
}
</script>

<style>
*{margin: 0;padding: 0;box-sizing: border-box;}
body{font-family: 'montserrat',sans-serif;}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}
#app.warm{background-image: url('./assets/warm-bg.jpg');}
main{
  min-height: 100vh;padding: 25px;background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}
.search-box{width: 100%;margin-bottom: 30px;}
.search-box .search-bar{width: 100%;display: block;padding: 15px;color: #313131;font-size: 20px;appearance: none;border: none;outline: none;background: none;box-shadow: 0 0 8px rgba(0,0,0,0.25);background-color: rgba(255,255,255,0.5);border-radius: 0 16px 0 16px;transition: .4s;}
.search-box .search-bar:focus{box-shadow:0 0 16px rgba(0,0,0,0.25);background-color: rgba(255,255,255,0.75);border-radius: 16px 0 16px 0;}
.location-box .location{color: #fff;font-size: 32px;font-weight: 500;text-align: center;text-shadow: 1px 3px rgba(0,0,0,0.25);}
.location-box .date{color: #fff;font-size: 20px;font-weight: 300;font-style: italic;text-align: center;}
.weather-box{text-align: center;}
.weather-box .temp{display: inline-block;padding: 10px 25px;color: #fff;font-size: 102px;font-weight: 900;text-shadow: 3px 6px rgba(0,0,0,0.25);background-color: rgba(255,255,255,0.25);border-radius: 16px;margin: 30px 0;box-shadow: 3px 6px rgba(0,0,0,0.25);}
.weather-box .weather{color: #fff;font-size: 48px;font-weight: 700;font-style: italic;text-shadow: 3px 6px rgba(0,0,0,0.25);margin-bottom: 20px;}
.weather-box p{color: #fff;}
</style>