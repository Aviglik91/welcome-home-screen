<template>
    <v-row>
        <v-col col="6">
            <v-sheet height="400">
                <v-calendar
                ref="calendar"
                :now="today"
                :value="today"
                :events="processedEvents"
                color="primary"
                type="week"
                ></v-calendar>
            </v-sheet>
        </v-col>
        <v-col col="6">
            <v-sheet height="400">
                <v-calendar
                    ref="calendar2"
                    :now="today"
                    :value="today"
                    :events="processedEvents"
                    color="primary"
                    type="week"
                ></v-calendar>
            </v-sheet>
      </v-col>

        <div class='authentification'>
    <h2>VueJS + Google Calendar Example</h2>
    Authentification
    <button v-if='!authorized' @click="handleAuthClick">Sign In</button>
    <button v-if='authorized' @click="handleAuthClick">Sign Out</button>
  </div>
  <hr>
  <button v-if='authorized' @click="getData">Get Data</button>
  <div class="item-container" v-if="authorized && items">
    <pre v-html="items"></pre></div>
    </v-row>

</template>

<script>

const CLIENT_ID = '186196360540-s1el226tds79q5qd8j333v25rvp6jtg1.apps.googleusercontent.com';
const API_KEY = 'AIzaSyB3lh3hNrxE1HygnqJu1OvnIVwdy1jpvxg';
// Array of API discovery doc URLs for APIs used by the quickstart
const DISCOVERY_DOCS = ['https://www.googleapis.com/discovery/v1/apis/calendar/v3/rest'];
// Authorization scopes required by the API; multiple scopes can be
// included, separated by spaces.
const SCOPES = 'https://www.googleapis.com/auth/calendar.readonly';

export default {
    data: () => ({
        today: '2021-05-02',
        processedEvents: [],
        items: undefined,
        api: undefined,
        authorized: false
    }),
    mounted () {
        this.$refs.calendar.scrollToTime('08:00')
        this.api = gapi;
        this.handleClientLoad();
    },
    methods: {
        handleClientLoad() {
            this.api.load('client:auth2', this.initClient);
            
        },
        initClient() {
            let vm = this;

            vm.api.client.init({
                apiKey: API_KEY,
                clientId: CLIENT_ID,
                discoveryDocs: DISCOVERY_DOCS,
                scope: SCOPES
            }).then(_ => {
                // Listen for sign-in state changes.
                vm.api.auth2.getAuthInstance().isSignedIn.listen(vm.authorized);
            });
        },
        convertDatetimeObj(date){
            let year, month, dt, hours, minutes;
            date = new Date(date);
            year = date.getFullYear();
            month = date.getMonth()+1;
            dt = date.getDate();
            hours = date.getHours();
            minutes = date.getMinutes();

            if (dt < 10) {
                dt = '0' + dt;
            }
            if (month < 10) {
                month = '0' + month;
            }
            if(hours < 10){
                hours = '0' + hours;
            }
            if (minutes < 10) {
                minutes = '0' + minutes;
            }
            let result = `${year}-${month}-${dt} ${hours}:${minutes}`
            console.log(result);
            return result;
 
        },
        handleAuthClick(event) {
            Promise.resolve(this.api.auth2.getAuthInstance().signIn())
                .then(_ => {
                this.authorized = true;
                this.getData();
                });
        },
        getData() {
            let vm = this;

            vm.api.client.calendar.events.list({
                'calendarId': 'primary',
                'timeMin': (new Date()).toISOString(),
                'showDeleted': false,
                'singleEvents': true,
                'maxResults': 100,
                'orderBy': 'startTime'
            }).then(response => {
                
                vm.items = this.processEvents(response.result.items);
                console.log(vm.items);
            });
        },
        handleSignoutClick(){},
        processEvents(json) {
            // name: 'Mash Potatoes',
            //     start: '2019-01-09 12:30',
            //     end: '2019-01-09 15:30',
            if (typeof json == 'string') {
                json = JSON.parse(json);
            }

            json.forEach(rawEvent => {
                    let newEvent = {
                        name: rawEvent.summary,
                        start: this.convertDatetimeObj(rawEvent.start.dateTime),
                        end:  this.convertDatetimeObj(rawEvent.end.dateTime)
                    }
                    this.processedEvents.push(newEvent);
            });
        }
    }
    
}
</script>

<style scoped>
    .weather-block{
        display: flex;
        justify-content: space-evenly;
    }
</style>
