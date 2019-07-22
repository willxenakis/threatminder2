<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div id="container" style="text-align: left">
      <h3 style="margin:0">Social Media</h3>
      <div id="addedPlatforms">
        <div class="selectedUser" v-if="facebookSelected">
          <img id="icon" class="socialMediaButton" src="../assets/facebook.png">
          <p id="url">{{facebookLink}}</p>
          <button id="deleteUser" v-on:click="deleteFacebook"><img src="../assets/trachcan.png"></button>
        </div>
        <div class="selectedUser" v-if="instagramSelected">
          <img id="icon" class="socialMediaButton" src="../assets/instagram.jpeg">
          <p id="url">{{instagramLink}}</p>
          <button id="deleteUser" v-on:click="deleteInstagram"><img src="../assets/trachcan.png"></button>
        </div>
        <div class="selectedUser" v-if="twitterSelected">
          <img id="icon" class="socialMediaButton" src="../assets/twitter.png">
          <p id="url">{{twitterLink}}</p>
          <button id="deleteUser" v-on:click="deleteTwitter"><img src="../assets/trachcan.png"></button>
        </div>
        <div class="selectedUser" v-if="youtubeSelected">
          <img id="icon" class="socialMediaButton" src="../assets/youtube.png">
          <p id="url">{{youtubeLink}}</p>
          <button id="deleteUser" v-on:click="deleteYoutube"><img src="../assets/trachcan.png"></button>
        </div>
      </div>
      <button id="addAccountButton" v-if="showAddAccount" v-on:click="add_account">Add Account</button>
      <div id="siteLogos" v-if="buttonClicked">
        <p>Please choose a social media platform</p>
        <button id="facebook" v-if="!facebookSelected" v-on:click="chosenPlatform($event)"  class="socialMediaButton"><img src="../assets/facebook.png"></button>
        <button id="instagram" v-if="!instagramSelected" v-on:click="chosenPlatform($event)"  class="socialMediaButton"><img src="../assets/instagram.jpeg"></button>
        <button id="twitter" v-if="!twitterSelected" v-on:click="chosenPlatform($event)"  class="socialMediaButton"><img src="../assets/twitter.png"></button>
        <button id="youtube" v-if="!youtubeSelected" v-on:click="chosenPlatform($event)"  class="socialMediaButton"><img src="../assets/youtube.png"></button>
        <button v-on:click="cancelPlatformSelection" id="youtube" class="socialMediaButton"><img src="../assets/cancel.jpeg"></button>

      </div>
    </div>
    <div v-if="modalAppear" id="myModal" class="modal">
      <!-- Modal content -->
      <div class="modal-content">
        <p>{{chosenSite}} Account</p>
        <button v-on:click="closeModal" class="close"><img src="../assets/cancel.jpeg"></button>
        <!-- <input v-model="urlInput" type="text" v-bind:placeholder="'https://' + chosenSite + '.com/username'"> -->
        <input id="usernameInput" v-model="urlInput" type="text" placeholder="username">
        <button id="validateButton" v-on:click="search">Validate</button>
        <div v-if="validLink" id="validLink">
          <h3 style="color:green">
          Valid Account
          </h3>
          <div id="user">
            <p style="display: flex; grid-area: name">Link: {{urlInput}}</p>
            <button style="display: flex; grid-area: button; margin: 5px; align-content: center" v-on:click="select">Select User</button>
          </div>
          
        </div>
        <div v-if="invalidLink" id="invalidLink">
          <h3>Invalid Account</h3>
          <p>Sorry, the username you have input is not valid on the chosen social media platform.
            Please try another</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'HelloWorld',
  data () {
    return{
      buttonClicked: false,
      modalAppear: false,
      showAddAccount: true,
      chosenSite: "",
      urlInput: "",
      validLink: false,
      invalidLink: false,
      instagramSelected: false,
      facebookSelected: false,
      youtubeSelected: false,
      twitterSelected: false,
      instagramLink: "",
      facebookLink: "",
      youtubeLink: "",
      twitterLink: "",
    }
  },
  props: {
    msg: String
  },
  methods: {
    add_account: function () {
      this.buttonClicked = true
      this.showAddAccount = false
    },
    chosenPlatform: function (event) {
      let bob = event.currentTarget.id
      this.modalAppear = true
      this.chosenSite = bob
    },
    closeModal: function() {
      this.modalAppear = false
      this.validLink = false
      this.invalidLink = false
    },
    cancelPlatformSelection: function(){
      this.buttonClicked = false
      this.showAddAccount = true
    },
    search: function() {
      let link = "https://www." + this.chosenSite + ".com/" + this.urlInput
      console.log(link)

      this.$http.get(link).then(function(response){
        if(response.status == "200"){
          this.validLink = true
          this.invalidLink = false
          console.log(response.status);
          console.log(response)
        }
      }).catch(e => {
        this.validLink = false
        this.invalidLink = true
        console.log(e);
      });
      console.log(this.chosenSite)
    },
    select: function() {
      if(this.chosenSite=="instagram"){
        this.instagramLink = "https://www." + this.chosenSite + ".com/" + this.urlInput
        this.instagramSelected = true
      }
      else if (this.chosenSite=="facebook"){
        this.facebookLink = "https://www." + this.chosenSite + ".com/" + this.urlInput
        this.facebookSelected = true
      }
      else if (this.chosenSite=="twitter"){
        this.twitterLink = "https://www." + this.chosenSite + ".com/" + this.urlInput
        this.twitterSelected = true
      }
      else if (this.chosenSite=="youtube"){
        this.youtubeLink = "https://www." + this.chosenSite + ".com/" + this.urlInput
        this.youtubeSelected = true
      }
      this.modalAppear = false
      this.buttonClicked = false
      this.showAddAccount = true
      this.validLink= false
      this.invalidLink=false
    },
    deleteInstagram: function(){
      this.instagramSelected=false
      this.instagramLink=""
    },
    deleteFacebook: function(){
      this.facebookSelected=false
      this.facebookLink=""
    },
    deleteTwitter: function(){
      this.twitterSelected=false
      this.twitterLink=""
    },
    deleteYoutube: function(){
      this.youtubeSelected=false
      this.youtubeLink=""
    }
  }
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.selectedUser{
  background: rgb(238, 233, 233);
  height: 4vw;
  overflow: hidden;
  position: relative;
}
#icon{
  position: absolute;
  height: 3vw;
  width: 3vw;
  left: 1vw;
  top: 0.5vw;
  display: inline;
  margin: 0;
}
#url{
  position: absolute;
  top: 0.5vw;
  left: 5vw;
  display: inline;
}
#deleteUser{
  position: absolute;
  right: 3vw;
  top: 1vw;
  height: 2vw;
  width: 2vw;
  display: inline; 
}
#user{
  display: grid;
  grid-template-areas: 'name space button';
  grid-template-columns: 20% 65% 15%;
  position: relative; 
  background: rgb(210, 216, 218);
}
img{
  max-width:100%;
  max-height:100%;
}
#siteLogos{
  background: rgb(210, 216, 218)
}
#usernameInput{
  width: 40%;
  margin: 10px;
  font-size: 12pt
}
.socialMediaButton{
  display: inline;
  height: 50px;
  width: 50px;
  margin: 20px;
}

#addAccountButton {
border: none;
background: #404040;
color: #ffffff !important;
font-weight: 500;
margin: 5px;
margin-left: 0;
padding: 10px;
text-transform: uppercase;
border-radius: 6px;
display: inline-block;
}
#addAccountButton:hover {
cursor: pointer;
}
#validateButton{
  border: none;
  background: #404040;
  color: #ffffff !important;
  width: 20%;
  height: 25px;
  text-transform: uppercase;
  display: inline;
}
#validateButton:hover{
  cursor: pointer;
}

/* The Modal (background) */
.modal {
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  position: relative;
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 40%; /* Could be more or less, depending on screen size */
}
#container{
  width:60%;
  margin: 0 auto
}

.close {
  position: absolute;
  top: 5px;
  right: 5px;
  width: 5%;
  height: 10%
}
</style>