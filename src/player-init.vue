<template>
  <section class="player-init">
    <div class="box title is-1">
      Add Player:
      <input autofocus v-model="playerName" class="input is-primary" type="text" @keyup.enter="addPlayer" placeholder="Type Name">

      <button type="button" class="button is-primary" @click="addPlayer">Add Player</button>
      <button type="button" class="button is-success" @click="shuffle">done</button>
    </div>
    
    <br />
    <br />
    <div v-for="(player, idx) in names">
      <div class="box list-item">
        <div class="is-pulled-left">Player name: <span class="player-name">{{player}}</span> </div>
        <button :data-idx="idx" class="button is-small is-danger is-pulled-right" @click="deletePlayer(idx)">X</button>
      </div>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        words:
        ["נקניק", "מְדוּזָה", "עִפָּרוֹן", "ענן", "ירח", "מַיִם", "מַחשֵׁב", "בית", "ספר", "רֶשֶׁת", "פטיש", "הליכה", "באלימות", "בֵּינוֹנִי", "סִפְרוּת", "כִּסֵא", "שתיים", "חַלוֹן", "מיתרים", "מוּסִיקָלִי", "זֶבּרָה", "קסִילוֹפוֹן", "פינגווין", "בית", "כֶּלֶב", "סופי", "דְיוֹ", "מוֹרֶה", "כֵּיף", "אתר", "אינטרנט", "בננה", "דוֹד", "ברכות", "מגה", "עשר", "מדהים", "כָּחוֹל", "אינטרנט", "בקבוק", "צָמוּד", "אֵזוֹר", "עגבנייה", "בית-כלא", "הידרו", "ניקוי", "טלויזיה", "לִשְׁלוֹחַ", "צְפַרְדֵעַ", "גָבִיעַ", "סֵפֶר", "התקרבות", "נופל", "גיימר", "מִכסֶה", "מִיץ", "מוניטור", "סֶרֶן", "קשר", "בקול", "רם", "חבטות", "גִיטָרָה", "גילוח", "שיער", "כדורגל", "מַיִם", "מחבט", "שולחן", "מאוחר", "כְּלֵי", "תִקְשׁוֹרֶת", "שולחן", "העבודה", "סְנַפִּיר", "מוֹעֲדוֹן", "עַף", "חלק", "מִפלֶצֶת", "סָגוֹל", "אַפּוֹטרוֹפּוֹס", "נוֹעָז", "קישור", "הַצָגָה", "עוֹלָם", "לאומי", "תגובה", "אֵלֵמֶנט", "קֶסֶם", "אַריֵה"],
        playerName: '',
        names: [],
        players: [],
        msg: 'Add Players'
      }
    },
    methods: {
      addPlayer() {
        this.names.push(this.playerName);
        this.playerName = '';
        // console.log('names', this.names);
      },
      deletePlayer: function (currIdx) {
        this.names = this.names.filter((player, idx) => idx !== currIdx);
        // console.log('this.players', this.names);
      },
      shuffle() {
        var shuffledNames = [];
        var shuffledWords = [];
        while (this.words.length > 0) {
          var idx = Math.floor(Math.random() * this.words.length);
          shuffledWords.push(this.words.splice(idx, 1)[0])
        }
        while (this.names.length > 0) {
          var idx = Math.floor(Math.random() * this.names.length);
          shuffledNames.push(this.names.splice(idx, 1)[0])
        }
        this.buildChain(shuffledNames, shuffledWords);
      },
      buildChain(shuffledNames, shuffledWords) {
        var preShuffledPlayers = [];
        shuffledNames.forEach((name, id) => {
          let player = {
            id,
            name,
            word: shuffledWords[id],
            isAlive: true
          }
          preShuffledPlayers.push(player);
        });
        preShuffledPlayers.forEach((player, index) => {
          if (index === preShuffledPlayers.length - 1) { player.target = preShuffledPlayers[0] }
          else { player.target = preShuffledPlayers[index + 1] }
        console.log('player is: ', player.name, ' target is: ', player.target.name);
        });
        while (preShuffledPlayers.length > 0) {
          var rng = Math.floor(Math.random() * preShuffledPlayers.length);
          this.players.push(preShuffledPlayers.splice(rng, 1)[0])
        }
        // console.log(this.players);
        this.$emit('getPlayers', this.players)
      },

      exportList() {

      }
    },

  }
</script>

<style scoped>



.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px;
}

.box {
  width: 300px;
  margin: 2px auto;
}

.input {
  margin: 10px;
}

.player-name {
  font-weight: bold;
  font-size: 20px;
}

</style>