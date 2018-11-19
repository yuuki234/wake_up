<template>
  <div class="hello">
    <h1>youtube目覚し</h1>
    <h2>タイマー</h2>
    <input v-model.number=timer_hour type="number">
    時
    <input v-model.number=timer_min type="number">
    分
    youtube再生リスト
    <input type="text" v-model=timer_youtube>
    <button v-on:click="timer">スタート</button>
    <ol v-for="item in date_file" v-bind:key="item.id">
      <select v-model=item.hour>
        <option v-for="n in 24" v-bind:key="n">
          {{ n-1 }}
        </option>
      </select>
      :
      <select v-model=item.min>
        <option v-for="n in 60" v-bind:key="n">
          {{ n-1 }}
        </option>
      </select>
      youtube再生リスト<input type="text" v-model=item.youtube>
      <input type="checkbox" v-model=item.check>
      <button v-on:click="alerm_delete(item.id)">アラーム削除</button>
    </ol>
    <button v-on:click="alerm_add">アラーム追加</button>
    <p>{{ now_hour }}時 {{ now_min }}分 {{ now_sec }}</p>
    <h2>timer</h2>
    <p>{{ timer_hour }}:{{ timer_min }}:{{ timer_sec }}</p>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      now_hour: '',
      now_min: '',
      now_sec: '',
      timer_hour: 0,
      timer_min: 0,
      timer_sec: 0,
      timer_youtube: 'hogehoge',
      date_file: [
        { id: 1, hour: '10', min: '42', check: true, youtube: 'https://youtu.be/JxAlyikNOqs?list=PLUX1pZMKwUe4yoP2cvTHpD21_1MDa_5rL' }
      ]
    }
  },
  created: function () {
    window.localStorage.getItem('alerm')
    window.setInterval(() => {
      var date = new Date()
      this.now_hour = date.getHours()
      this.now_min = date.getMinutes()
      this.now_sec = date.getSeconds()
      this.date_alerm()
    }, 1000)
  },
  methods: {
    date_alerm: function () {
      for (let date of this.date_file) {
        if (parseInt(date.hour) === this.now_hour && parseInt(date.min) === this.now_min && this.now_sec === 0 && date.check) {
          window.open(date.youtube)
        }
      }
    },
    alerm_add: function () {
      var max = this.date_file[this.date_file.length - 1]
      this.date_file.push({
        id: max.id + 1,
        hour: '0',
        min: '0',
        check: true,
        youtube: ''
      })
    },
    alerm_delete: function (index) {
      if (this.date_file.length > 1) {
        for (let step = 0; step < this.date_file.length; step++) {
          if (this.date_file[step].id === index) {
            this.date_file.splice(step, 1)
          }
        }
      }
    },
    timer: function () {
      this.timer_sec = 59
      if (this.timer_hour !== 0 && this.timer_min === 0) {
        this.timer_hour = this.timer_hour - 1
        this.timer_min = 59
      } else {
        this.timer_min = this.timer_min - 1
      }
      var olse =
        window.setInterval(() => {
          if (this.timer_hour === 0 && this.timer_min === 0 && this.timer_sec === 0) {
            window.open(this.timer_youtube)
            window.clearInterval(olse)
          } else {
            if (this.timer_hour !== 0 && this.timer_min === 0) {
              this.timer_hour = this.timer_hour - 1
              this.timer_min = 59
            } else if (this.timer_min !== 0 && this.timer_sec === 0) {
              this.timer_min = this.timer_min - 1
              this.timer_sec = 59
            } else {
              this.timer_sec--
            }
          }
        }, 1000)
    }
  },
  destroyed: function () {
    window.localStorage.setItem('alerm', 'https://www.youtube.com/watch?v=XZM1qLn9C54')
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
