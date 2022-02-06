<template>
  <div>
    <header></header>
    <h1>API Tracking Routes</h1>
    <p>Choisissez une période pour commencer</p>

    <div class="period">
      <Calendar @send-dates="getDates" :datas="startLimits" type="debut" />
      
      <Calendar @send-dates="getDates" :datas="endLimits" type="fin" />
    </div>

   <TotalCalls v-if="this.start != '' && this.end != ''" :count="counts" :start="start" :end="end" />

   <!-- <Chart></Chart> -->
  </div>
</template>

<script>
  import axios from 'axios';
  import Calendar from './components/Calendar.vue'
  import TotalCalls from './components/TotalCalls.vue'
  // import Chart from './components/Chart.vue'

  export default {
    name: 'App',
    components: {
      Calendar,
      TotalCalls,
      // Chart
    },
    data() {
      return {
        start: '',
        end: '',
        dates: [],
        startLimits: {},
        endLimits: {},
        counts: 0
      }
    },
    created(){
      this.getData()
    },
    methods: {
      getCounts(){
        // Réinitialiser le nombre d'appels
        this.counts = 0;

        let startRef = new Date(this.start);
        let endRef = new Date(this.end);
        
        // Compter le nombre d'appels entre les dates choisies
        this.dates.forEach(element => {
          let dateCompare = new Date(element.date);

          if (startRef <= dateCompare && dateCompare <= endRef) {
            this.counts += element.count;
          }

        })
      },
      getDates(data) {
        // Au changement de la date du début
        if (data.name == 'debut') {
          this.start = data.date;

          // Empêcher l'utilisateur de choisir une date antérieure à la date de début
          this.endLimits["min"] = data.date;

          // Réinitialiser la limite de fin si l'utilisateur choisit une date de début postérieure à la date de fin choisie
          if (data.date > this.endLimits["max"]){
            this.endLimits["max"] = this.dates[747].date;
          }

        // Au changement de la date de fin
        } else {
          this.end = data.date;
        }

        this.getCounts()
      },
      getData() {
        let options = {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        };
        let date;
        let dateString;

        axios
        .get('https://api-buildings.homeys.io/api/tracking', {
          "contentType": "application/json; charset=utf-8;",
          "headers": {
            "Authorization": "t8dwXjjIjrRq54LSq1Vt3SmWSYi9K7jbVzP5UdgD7ptPTJE5N0"
          }
        }).then(response => {
          response.data.forEach((element, index) => {
            date = new Date(element.date);
            dateString = date.toLocaleDateString('en-US', options)

            this.dates.push({
              id: index,
              dateString: dateString,
              date: element.date,
              count: element.count,
              path: element.path
            })
          })
          // Initialiser les limites du calendrier aux dates des données
          this.startLimits["min"] = this.dates[0].date;
          this.startLimits["max"] = this.dates[747].date;
          this.endLimits["min"] = this.dates[0].date;
          this.endLimits["max"] = this.dates[747].date;
        })
      }
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  body {
    margin: 0;
  }

  header {
    background-color: #304282;
    height: 60px;
  }
  
  .period {
    display: flex;
    flex-direction: column;
    justify-content: space-around;

    width: 50%;
    margin: auto;
  }

  @media screen and (min-width: 360px){
    .period {
      flex-direction: row;
    }
  }
</style>