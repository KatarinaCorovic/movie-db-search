<template>
  <div class="hello">
    <div class="inputContainer">
    <i class="fa fa-search"></i><input type="text" v-model = "searchTerm" v-on:keyup="updateList" v-bind:placeholder="placeholderValues.search">
    <i class="fa fa-plus"></i>

    
       
    <select name="list" id="list" v-model="searchType" v-on:click="updateList">
    <option value="1">Movies</option>
    <option value="2">Series</option>
    <option value="3">Episodes</option>
</select>
 
    </div>


  <ul v-if="list">

    <li v-for="(item, index) in list" v-bind:key="index">
      <img v-bind:src="item.Poster" alt="">

       <h2> {{item.Title}}  ({{item.Year}}) </h2>
       <p>{{item.Type}}</p>
       <p>Movie</p>     
    </li>
    
  </ul>
  <p  v-if="error">{{ this.error}}</p>
  </div>
</template>

<script>
import axios from 'axios';


export default {
  name: 'HelloWorld',
 data(){
   return{
     placeholderValues:{
       search:'Search...'
     },

     apiKey: "c81ffb3d",
     searchTerm: '',
     error:"",
   list: null,
   searchType: ""
    
   } 

},

methods: {
    updateList: function(){
      axios
      .get('http://www.omdbapi.com/?s=' + this.searchTerm + '&apikey=c81ffb3d')
      .then(response => {
        if(response.data.Response == "True" ){
        this.list = response.data.Search;
        this.error = null;
        console.log(response.data.Search);
        } else {
          if(response.data.Error == "Something went wrong.")
          {
            this.error = "Search for something."
            
          }else{
            this.error = response.data.Error; 
          }
          this.list = null;

        }
        
      })
      .catch(this.error ="Something went wrong, please try again later.")
      

    }
  } 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.hello{
  

.inputContainer{
  position: relative;
     

  input{
    background-color: #1BBC9B;
    width:800px;
    height:70px;
    border: none;
    font-size: 20px;
    border: 1px solid #1BBC9B; 
    border-radius: 7px;
    color:#34495E;
    padding-left: 50px;


  }

.fa-search{
  position:absolute;
  top:27px;
  left:250px;
  color:#34495E;
  font-size: 20px;

}

.fa-plus{
  position: absolute;
    color:#34495E;
      font-size: 23px;
      top:25px;
      left:1030px;
}

select{
  width:200px;
  height:73px;
  background-color:#34495E ;
  border-radius: 7px; 
  font-size:20px; 
  cursor: pointer;


  option{
    font-size:20px;
    background-color:#fff;
    

  }

}

  }

  ul{
     display:inline - block;
     margin:0 auto;
      margin-left: -40px;



    li{
      position: relative;
      list-style-type: none;
      display:flex;
      justify-content: space-between;
      align-items: center;
      width:1025px;
      text-align: center;
      background-color:#34495E;
      height: 70px;
      padding:15px;
      font-size:15px;
      color:#fff;
      // font-family: 'Arial', sans-serif;
      position: relative;
      
      &:last-child{
        border-radius: 0 0 7px 0;
      }

      img{
        position:absolute;
        top:0;
        left:0;
        width:80px;
        height: 80px;
        padding:10px;
      }

      p{
        font-size: 17px;
        text-align: right;
       
        text-transform: capitalize;
      }


    }

       
  }
}

</style>
