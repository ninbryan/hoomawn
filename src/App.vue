<template>
  <div>
    <div>Birth</div>
    
    <section class="birth columns section">
      <div class="column">
        <div><label for="birth-year">Year</label></div>
        <input type="number" name="birth-year" v-model="birthYear" :max="nowDate.getFullYear()" />
      </div>
      
      <div class="column">
        <div><label for="birth-month">Month</label></div>
        <input type="number" name="birth-month" v-model="birthMonth" min="1" max="12" />
      </div>
      
      <div class="column">
        <div><label for="birth-day">Day</label></div>
        <input type="number" name="birth-day" v-model="birthDay" min="1" max="31"/>
      </div>
    </section>
    
    <div>
      <div>Life Span</div>
      <div>
        <label for="life">Years</label>
        <input type="number" name="life" v-model="life" :min="0"/>
      </div>
    </div>
    
    <div>AGE (in years): {{age.years}}</div>
    <div>AGE (in months): {{age.months}}</div>
    <div>AGE (in days): {{age.days}}</div>
    
    <button @click="toggle">{{buttonText}}</button>
    
    <div class="container fluid">
      <div v-for="(months, year) in rows" class="year" key="year-{{year}}">
        <div v-for="(lived, month) in months" class="month" :class="{lived: lived}" key="year-{{year}}-month-{{month}}"></div>
      </div>
    </div>
  </div>
</template>

<style type="text/css">
  .month {
    float: left;
    background-color: lightgreen;
    margin: 1px;
    width: 10px;
    height: 10px;
    border-radius: 50%;
  }
  .month.lived {
    background-color: green;
  }
</style>

<script>
export default {
  el: '#app',
  data: function() {
    const nowDate = new Date(Date.now());
    return {
      life: 125,
      birthYear: nowDate.getFullYear() - 24,
      birthMonth: nowDate.getMonth() + 1,
      birthDay: nowDate.getDate(),
      isDays: false
    };
  },
  methods: {
    toggle: function() {
      this.isDays = !this.isDays
    }
  },
  computed: {
    birthDate: function(data) {
      return new Date(data.birthYear, data.birthMonth - 1, data.birthDay);
    },
    nowDate: function() {
      return new Date(Date.now());
    },
    age: function(data) {
      let ageInMilliseconds = (data.nowDate - data.birthDate);
      return {
        years: ageInMilliseconds / (365.25 * 24 * 60 * 60 * 1000),
        months: (ageInMilliseconds / (365.25 * 24 * 60 * 60 * 1000)) * 12,
        days: ageInMilliseconds / (24 * 60 * 60 * 1000)
      };
    },
    buttonText: function() {
      return this.isDays ? "in Days" : "in Months";
    },
    rows: function() {
      if (this.isDays) {
        let days = [];
        
        let all = this.life * 365.25;
        let a = parseInt(this.age.days);
        
        for (let x = 0; x < all; x++) {
          days.push(x < a ? 1 : 0);
        }
        
        return [days];
      }
      let a = parseInt(this.age.months) ;
      let years = [];
      for (let year = 0; year < this.life; year++) {
        years.push([]);
        for (let month = 0; month < 12; month++) {
          if (a) {
            years[year].push(1);
            a -= 1;
          } else {
            years[year].push(0);
          }
        }
      }
      return years;
    }
  }
};
</script>