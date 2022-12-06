<template>
    <h1>recap</h1>
    <br>
    <button v-on:click="fillArray()">
      test recap
    </button>
    <br>
    <!-- <div>
      {{hvi}}
    </div> -->
     <div class="flex items-start justify-center h-screen">
    <table class="table w-3/4 text-center">   
    <thead>
      <tr>        
        <th class="w-32">UHML</th>
        <th class= "w-32"> TOTAL </th>
        <th class="w-32">UI</th>
        <th class= "w-32"> TOTAL </th>
        <th class="w-32">Strength</th>
        <th class= "w-32"> TOTAL </th>
        <th class="w-32">SFI</th>
        <th class= "w-32"> TOTAL </th>
        <th class="w-32">Mic</th>
        <th class= "w-32"> TOTAL </th>
        <th class="w-32">ColorGrade</th>
        <th class= "w-32"> TOTAL </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>0.80 - 0.85</td>
        <td>
          {{ uhml1 }}
        </td>
        <td>3.0 - 10.9</td>                     
        <td>
          {{ ui1 }}
        </td>
        <td>70.0 - 70.9</td>                     
        <td></td> 
        <td>17.0 - 17.9</td>                    
        <td></td> 
        <td>2.0</td>                    
        <td></td> 
        <td>11-1</td>                    
        <td></td>                     
      </tr>  
      <tr>
        <td>0.86 - 0.89</td>
        <td></td>
        <td>11.0 - 15.9</td>                     
        <td></td>
        <td>71.0 - 71.9</td>                     
        <td></td> 
        <td>18.0 - 18.9</td>                    
        <td></td> 
        <td>2.1</td>                    
        <td></td> 
        <td>11-2</td>                    
        <td></td>                     
      </tr>   
       <tr>
        <td>0.90 - 0.92</td>
        <td></td>
        <td>16.0 - 20.9</td>                     
        <td></td>
        <td>72.0 - 72.9</td>                     
        <td></td> 
        <td>19.0 - 19.9</td>                    
        <td></td> 
        <td>2.2</td>                    
        <td></td> 
        <td>11-3</td>                    
        <td></td>                     
      </tr>   
       <tr>
        <td>0.93 - 0.95</td>
        <td></td>
        <td>21.0 - 30</td>                     
        <td></td>
        <td>73.0 - 73.9</td>                     
        <td></td> 
        <td>20.0 - 20.9</td>                    
        <td></td> 
        <td>2.3</td>                    
        <td></td> 
        <td>11-3</td>                    
        <td></td>                     
      </tr> 
       <tr>
        <td>0.96 - 0.98</td>
        <td></td>
        <td></td>                     
        <td></td>
        <td>74.0 - 74.9</td>                     
        <td></td> 
        <td>21.0 - 21.9</td>                    
        <td></td> 
        <td>2.4</td>                    
        <td></td> 
        <td>11-4</td>                    
        <td></td>                     
      </tr> 
      
    </tbody>
  </table>
   </div>
</template>

<script>
import axios from 'axios';

//Creamos variables y un array vacio con al finalidad de popular el array con los datos traidos de la DB
export default {
    data: () => ({
    hvi: [],
    UHML: '',
    UI: '',
    STRENGTH: '',
    SFI: '',
    MIC: '',
    COLORGRADE: '',
    TRASHID: '',
    uhml1: 0,
    ui1: 0
  }),

  methods: {
    fillArray(){
      let json = { //almacenamos todos los datos en un variable "json"
        Uhml : this.UHML,
        Ui : this.UI,
        Strength : this.STRENGTH,
        Sfi : this.SFI,
        Mic : this.MIC,
        ColorGrade : this.COLORGRADE,
        TrashId : this.TRASHID
      }

      // let filtradoValores = (a,b,c,d,y,z) =>{
      //   a = b.filter((x) =>{
      //     return x.c >= y && x.c <= z
      //   })
      //   var e = a.length
      //   this.d = e
      //   return this.d
      // }

      //this.hvi = json //usamos el array vacio y lo llenamos con los datos que almacenamos en la variable "json"

      axios.get('http://localhost:8000/api/Recap', json).then(res => { //usando axios, llamamos a la API correspondiente colocamos como paramentro, el array
        this.hvi = res.data;
        const uhml_d = this.hvi; 
        // let arr;
        // let uhml;
        // filtradoValores(arr, uhml_d, uhml, uhml1, 1.27, 1.29)

        let arr = uhml_d.filter((x) =>{
         return x.uhml >= 1.27 && x.uhml <= 1.29
        })
        this.uhml1 = arr.length        

        let arr2 = uhml_d.filter((x) => {
          return x.ui >= 83.0 && x.ui <= 84.9
        })
        this.ui1 = arr2.length
      });
    }
  }
}
</script>