<template>
    <h4 align='center' class="header-syle">These are the list of Customer's records</h4>
    <p align='center'>Click on the each items to show the details</p>
    <div class="container">
        <div v-if="state.error.message">
        <h4>{{state.error.message}}</h4>
        </div>
        
        <div v-if="state.records.length == 0">
            <p align='center' class="fetch" > Fetching...</p>
            <div class="progress">
                <div class="indeterminate"></div>
            </div>
        </div>

        <div>
            <ul class="collapsible">
            <li v-for="(profile, index) in paginate" :key="index" >
                <div class="collapsible-header"><i class="material-icons">person</i><b>Username: </b> {{' '+ profile.UserName}}</div>
                <div class="collapsible-body">
                    <span>
                        <div class="row">
                            <div class="col s12 m12">
                            <div class="card-panel indigo">
                                <span class="white-text">
                                    <p>Fullname: {{profile.FirstName + ' ' + profile.LastName}}</p>
                                    <p>Email: {{ ' '+ profile.Email }}</p>
                                    <p>Gender: {{' '+profile.Gender }}</p>
                                    <p>Credit Card Number: {{ ' '+ profile.CreditCardNumber }}</p>
                                    <p>Phone NUmber: {{ ' '+ profile.PhoneNumber }}</p>
                                    <p>Last Login: {{ ' ' +profile.LastLogin}}</p>
                                    <p>Payment Method: {{ ' ' +profile.PaymentMethod}}</p>
                                </span>
                            </div>
                            </div>
                        </div>
                    </span>
                </div>
            </li>
            </ul>
        </div>
       
        <div class="row">
            <button class="btn col s3 m3" @click="previous_page" > <i class="material-icons left">navigate_before</i>Back </button>

            <button class="btn col s3 m3 offset-s1 offset-m1" @click="next_page" > <i class="material-icons right">navigate_next</i> Next </button> 
        </div>
    </div>
    
</template>

<script>

    

import axios from 'axios';
import {reactive, computed} from 'vue';
import M from 'materialize-css';
export default {
    name: 'records',

    setup(){
        const state = reactive({
            records: [],
            error: {},
            page_track: 0,
            end: false
        })

        const paginate = computed( () => {
            const ITEMS = 20
            const START = parseInt(state.page_track) * parseInt(ITEMS)
            const STOP = START + ITEMS
            let paginate_records
            if(state.records.length < 20){
                paginate_records = state.records.slice(START)
                //state.end = true
            }else{
                paginate_records = state.records.slice(START, STOP)
            }
            return paginate_records
        })



        const next_page = () =>  state.page_track++
        
        const previous_page = () => state.page_track--

        return {
            state,
            paginate,
            next_page,
            previous_page
        }
    },

    
    mounted(){
        M.AutoInit()
        
        axios.get('https://api.enye.tech/v1/challenge/records').then( result => {
            console.log(result)
            //update state
            const data = result.data.records.profiles
            this.state.records = data
        }).catch( err => {
            
            this.state.error.message = err.message

            console.log(this.state.error)
            // update error object
        })
    }
}
</script>

<style scoped>

.fetch{
    color: rgb(84, 206, 84);
    font-size: 19px;
}

.header-syle{
    color: indianred;
    padding-top: 10px;
    padding-bottom: 10px;
}

</style>