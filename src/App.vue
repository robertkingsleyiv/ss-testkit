<template>
<div id="app">
    <div id="Navbar">
        <span>Bens Soccer Signals Test Kit</span>
     
        <span>Update Count: <span>{{updateCounter}}</span></span>
        <button class="testKit_Button">
          <a target="_blank" href="https://www.sportmonks.com/docs/football/2.0/livescores/a/get-all-for-today/15">Sportmonks Documentation</a>
        </button>
        <div id="matchFlow_Selector">
        <button @click="matchflow =  1" class="testKit_Button">
          5
        </button>
             <button @click="matchflow =  2" class="testKit_Button">
          10
        </button>
     <button @click="matchflow =  3" class="testKit_Button">
          15
        </button>
        </div>

    </div>
    <div id="Scanner">
        <div id="Head">
          <span>Time</span>
                    <span>Teams</span>
                              <span>Scores</span>
                                        <span>Odds</span>
                                                     <span>Possession</span>   
                                                                     <span>Supremacy</span>        
                                                        <span>Total Shots</span>
                                                          <span>On Goal Shots</span>
                                                            <span>Inside Box Shots</span>
                                                                          <span>Corners</span>
                                                                             <span id="mf_Heading">
                                                                               <span>Match Flow</span> 
                                                                               <div id="secondRow_mf_Heading">


                                                                                          <span>Attacks</span>
                                                                                         <span>Supremacy</span>
                                                                                                   <span>Dangerous</span>
                                                                                                             <span>..</span>
                                                                               </div>
                                                                      

                                                                             </span>
        </div>
        <div id="Scanner-Body">
            <div v-for="item in data" :key="item.id" class="Scanner-Row">
                <div class="Scanner-Item">
                    <div id="Time">
                      {{item.time.status}}
                      {{item.time.minute}}

                    </div>
                    <div class="Home">
                        <div class="Team Name">{{item.localTeam.data.name}}</div>
                        <div class="Score">{{item.scores.localteam_score}}</div>
                        <div class="Live Odds" v-if="item.inplayOdds.data[0]">{{item.inplayOdds.data[0].values[0].value}}</div>
                        <span v-else>-</span>
                        <div class="Possession" v-if="item.stats.data[0]">{{item.stats.data[0].possessiontime}}%</div>
                                                <span v-else>-</span>
                        <div class="Supremacy" v-if="item.stats.data[0]">{{Math.round(item.supremacy.localTeam)}}%</div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[0]"><span v-if="item.stats.data[0].shots">{{item.stats.data[0].shots.total}}</span></div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[0]"><span v-if="item.stats.data[0].shots">{{item.stats.data[0].shots.ongoal}}</span></div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[0]"><span v-if="item.stats.data[0].shots">{{item.stats.data[0].shots.insidebox}}</span></div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[0]">{{item.stats.data[0].corners}}</div>
                                                <span v-else>-</span>
                    </div>
                    <hr class="itemHr">
                    <div class="Away">
                                <div class="Team Name">{{item.visitorTeam.data.name}}</div>
                        <div class="Score">{{item.scores.visitorteam_score}}</div>
                        <div class="Live Odds" v-if="item.inplayOdds.data[0]">{{item.inplayOdds.data[0].values[2].value}}</div>
                                                <span v-else>-</span>
                        <div class="Possession" v-if="item.stats.data[1]">{{item.stats.data[1].possessiontime}}%</div>
                                                <span v-else>-</span>
                        <div class="Supremacy" v-if="item.stats.data[1]">{{Math.round(item.supremacy.visitorTeam)}}%</div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[1]"><span v-if="item.stats.data[0].shots">{{item.stats.data[1].shots.total}}</span></div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[1]"><span v-if="item.stats.data[0].shots">{{item.stats.data[1].shots.ongoal}}</span></div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[1]"><span v-if="item.stats.data[0].shots">{{item.stats.data[1].shots.insidebox}}</span></div>
                                                <span v-else>-</span>
                        <div class="Attempts" v-if="item.stats.data[1]">{{item.stats.data[1].corners}}</div>
                                                <span v-else>-</span>

                  
                    </div>
                </div>
            </div>

        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios'
let $SM_KEY = "e26BGpAseLRsnrB0jg01ZnSyZFeLNq8b8ruymhZvR5xe9zYRxKnUVJsd6f5p"
export default {
    name: 'app',
    components: {

    },
    data() {
        return {
            data: [],
            updateCounter: 0,
            matchflow: 1
        }
    },
    mounted() {
        this.pollMatchData()
    },
    methods: {
        pollMatchData() {
            axios.get(`http://167.71.188.139:9299/data`).then(resp => {
                this.data = resp.data
                this.updateCounter++
            })
            console.log("data updated...")
            setTimeout(this.pollMatchData, 10000)
        }
    }

}
</script>

<style>
#secondRow_mf_Heading{
      display: grid;
        align-items: center;
      justify-items: center;

    grid-auto-columns: 55px;
    grid-auto-flow: column;
    font-size: 10px;
    grid-column-gap: 25px;

}
#mf_Heading{
  display:grid;
  margin-left: 250px
}
#matchFlow_Selector{
  margin-top:30px;
}
#Head{
  background: #273646;

  color:white;

   display: grid;
   border-bottom: 2px solid cadetblue;
        align-items: center;
    grid-auto-columns: 75px;
    grid-auto-flow: column;
    justify-items: center;
    grid-column-gap: 25px;
}
#Navbar {
    display: grid;
    grid-template-rows: 1fr 1fr;
    grid-template-columns: 1fr 1fr;
    grid-auto-flow: column;
    /* width: 50%; */
    align-items: center;
    align-self: center;
    justify-self: center;
    justify-items: center;
}
#Time {
    grid-area: time;
    padding-right: 20px;
    margin-left: -10px;
}
.Home {
    grid-area: home;
    display: grid;
        align-items: center;
    grid-auto-columns: 75px;
    grid-auto-flow: column;
    grid-column-gap: 25px;
}

.Away {
    grid-area: away;
     display: grid;
         align-items: center;
    grid-auto-columns: 75px;
    grid-auto-flow: column;
    grid-column-gap: 25px;
}
.testKit_Button{
  padding:10px;
  background:#2c3e50;
  color:white;
  font-weight: bolder;
  outline: none;
  cursor: pointer;
  border: 5px solid #273646;
  text-transform: uppercase;
}
.testKit_Button > a{
 color:white;
}


.Scanner-Row:nth-child(even) {
    background: rgb(214, 214, 214);
}
.Scanner-Row{
  padding: 25px 25px;
}

.itemHr {
    grid-area: seperate;
    border: 1px solid black;
    width: 100%;
}

.Scanner-Item {
    display: grid;
    grid-template-rows: 35px 3px 35px;
    grid-row-gap: 10px;
    align-content: center;
    grid-template-columns: 65px 1fr;
    grid-template-areas: 
     "time home"
     "time seperate"
      "time away";
    justify-content: center;
    align-items: center;
    width: 100%;

}

#Scanner {
    display: grid;
    grid-template-areas: "head""body";
    grid-template-rows: 1fr 5fr;
    border: 1px solid black;
    height: 80vh;
    position: fixed;
    width: 100vw;
    justify-self: center;
    left: 0%;
    top:17%;
    margin-top: 45px;
}

#Head {
    grid-area: head
}

#Scanner-Body {
    grid-area: body;
    overflow: scroll;
}

#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 10px;
}
</style>
