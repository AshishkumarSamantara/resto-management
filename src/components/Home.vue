
<template>
   <Header />
    <h1>Hello {{name}}, Welcome on Home Page</h1>
    <table border=" 1">
        <tr>
            <td>Id</td>
            <td>Name</td>
            <td>Contact</td>
            <td>Address</td>
            <td>Actions</td>
        </tr>
        <tr v-for="item in restaurant"  :key="item.id">
            <td>
                {{ item.id }}
            </td>
            <td>
                {{ item.name }}
            </td>
            <td>
                {{ item.contact }}
            </td>
            <td>
                {{ item.address }}
            </td>
            <td><router-link :to="'/update/'+item.id">Update</router-link></td>
            <button v-on:click="deleteRestaurant(item.id)">Delete</button>
            
        </tr>
    </table>
</template>
<script>
import Header from './Header.vue'
import axios from 'axios'
export default {
    name:"Home" ,
    data(){
        return{
            name:'',
            restaurant:[],
        }
    },
    components:{
        Header
    },
    methods:{
       async deleteRestaurant(id)
        {
            let result = await axios.delete("http://localhost:3000/restaurants/"+id);
            if(result.status==200)
            {
                this.loadData()
            }
        },
        async loadData()
    {
        let user = localStorage.getItem('user-info');
        this.name = JSON.parse(user).name;
        if(!user)
        {
             this.$router.push({name: 'SignUp'})
        }
        let result = await axios.get("http://localhost:3000/restaurants");
        console.warn(result)
        this.restaurant=result.data;
    }
    },
    
  async mounted()
   {
    this.loadData()
   }
   
}
</script>

<style>

table {
  border-collapse: collapse; 
  width: 100%; 
  font-family: sans-serif; 
}
th, td {
  border: 1px solid #ddd; 
  padding: 8px 15px; 
  text-align: left; 
}

th {
  background-color: #f1f1f1; 
  font-weight: bold; 
}

tr:nth-child(even) { 
  background-color: #f9f9f9;
}

</style>