<template>
    <h1>recap</h1>
    <br>
    <button v-on:click="fillArray()">
      test recap
    </button>
    <br>
     <button v-on:click="testeandolo()">
      sumatoria
    </button>
    <br>
    <div>
      {{sumatoria}}
    </div>
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
    <tbody >
      
    </tbody>
  </table>
</div>
</template>

<script>
import axios from 'axios';

export default {
    data: () => ({
    hvi: [], //Creamos variables y un array vacio con al finalidad de popular el array con los datos traidos de la DB   
    UH: [0.80 , 0.85, 0.86 , 0.89,	0.90 , 0.92,	0.93 , 0.95,	0.96 , 0.98,	0.99 , 1.01,	1.02 , 1.04,	1.05 , 1.07,	1.08 , 1.10,	1.11 , 1.13,	1.14 , 1.17,	1.18 , 1.20,	1.21 , 1.23, 1.24 , 1.26,	1.27 , 1.29,	1.30 , 1.32,	1.33 , 1.35, 1.36, 99999],
    UI: [70.0 , 70.9,	71.0 , 71.9,	72.0 , 72.9,	73.0 , 73.9,	74.0 , 74.9,	75.0 , 75.9,	76.0 , 76.9,	77.0 , 77.9,	78.0 , 78.9,	79.0 , 79.9, 80.0 , 80.9,	81.0 , 81.9, 82.0 , 82.9, 83.0, 99999],
    STR: [17.0 , 17.9,	18.0 , 18.9,	19.0 , 19.9,	20.0 , 20.9,	21.0 , 21.9,	22.0 , 22.9,	23.0 , 23.9,	24.0 , 24.9,	25.0 , 25.9,	26.0 , 26.9,	27.0 , 27.9,	28.0 , 28.9,	29.0 , 29.9,	30.0, 99999],
    SFI: [3.0 ,10.9,	11.0 ,15.9,	16.0 , 20.9,	21.0 , 30],
    MIC: [2.0,	2.1,	2.2,	2.3,	2.4,	2.5,	2.6,	2.7,	2.8,	2.9,	3.0,	3.1,	3.2,	3.3,	3.4,	3.5,	3.6,	3.7,	3.8,	3.9,	4.0,	4.1,	4.2,	4.3,	4.4,	4.5,	4.6,	4.7,	4.8,	4.9,	5.0,	5.1,	5.2,	5.3,	5.4,	5.5,	5.6,	5.7],
    CLR: [11-1,	11-2,	11-3,	11-4,	12-1,	12-2,	12-3,	12-4,	13-1,	13-2,	13-3,	13-4,	14-1,	14-2,	14-3,	14-4,	21-1,	21-2,	21-3,	21-4,	22-1,	22-2,	22-3,	22-4,	23-1,	23-2,	23-3,	23-4,	24-1,	24-2,	24-3,	24-4,	31-1,	31-2,	31-3,	31-4,	32-1,	32-2,	32-3,	32-4,	33-1,	33-2,	33-3,	33-4,	34-1,	34-2,	34-3,	34-4,	41-1,	41-2,	41-3,	41-4,	42-1,	42-2,	42-3,	42-4,	43-1,	43-2,	43-3,	43-4,	44-1,	44-2,	44-3,	44-4,	51-1,	51-2,	51-3,	51-4,	52-1,	52-2,	52-3,	52-4,	53-1,	53-2,	53-3,	53-4,	54-1,	54-2,	54-3,	54-4,	61-1,	61-2,	61-3,	61-4,	62-1,	62-2,	62-3,	62-4,	63-1,	63-2,	63-3,	63-4],
    TRSH: [1,	2,	3,	4,	5,	6,	7,	8, 9],
    baseArr: [],
    uhml1: [],
    ui1: [],
    str1:[],
    sumatoria: 0
  }),

  methods: {
    fillArray(){      

      axios.get('http://localhost:8000/api/Recap').then(res => { 
        this.hvi = res.data;  
       
        var i;

        var pos1 = 0;
        var pos2 = 1;
         for(i = 0; i < this.UH.length; i++){             
            let arr = this.hvi.filter((x) => {
             return x.uhml >= this.UH[pos1] && x.uhml <= this.UH[pos2]
            })
            this.uhml1.push(arr.length)
            pos1+=2
            pos2+=2
        }
       
       var pos3 = 0;
       var pos4 = 1;
        for(i = 0; i < this.UI.length; i++){             
            let arr = this.hvi.filter((x) => {
             return x.ui >= this.UI[pos3] && x.ui <= this.UI[pos4]
            })
            this.ui1.push(arr.length)            
            pos3+=2
            pos4+=2
        }

       var pos5 = 0;
       var pos6 = 1;
        for(i = 0; i < this.STR.length; i++){             
            let arr = this.hvi.filter((x) => {
             return x.strength >= this.STR[pos5] && x.strength <= this.STR[pos6]
            })
            this.str1.push(arr.length)
            console.log(arr);
            pos5+=2
            pos6+=2
        }

        // let arr = this.hvi.filter((x) =>{
        //  return x.uhml >= this.UH[10] && x.uhml <= this.UH[11]
        // })
        // this.uhml1 = arr.length        

        // let arr2 = this.hvi.filter((x) => {
        //   return x.ui >= 83.0 && x.ui <= 84.9
        // })
        // this.ui1 = arr2.length
       });    
    },

    testeandolo(){
      this.baseArr = [this.UH, this.UI, this.STR]
      console.log(this.baseArr[0]);
    }
  }
}


  // <tbody>
  //     <tr>
  //       <td> {{ this.UH[0] + ' - ' + this.UH[1] }} </td>
  //       <td>
  //         {{ uhml1[0] }}
  //       </td>
  //       <td>3.0 - 10.9</td>                     
  //       <td>
  //         {{ ui1[0] }}
  //       </td>
  //       <td>70.0 - 70.9</td>                     
  //       <td>{{ str1[0] }}</td> 
  //       <td>17.0 - 17.9</td>                    
  //       <td></td> 
  //       <td>2.0</td>                    
  //       <td></td> 
  //       <td>11-1</td>                    
  //       <td></td>                     
  //     </tr>  
  //     <tr>
  //       <td>{{ this.UH[2] + ' - ' + this.UH[3] }}</td>
  //       <td> {{uhml1[1]}} </td>
  //       <td></td>                     
  //       <td>{{ ui1[1] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[1] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                     
  //     </tr>   
  //      <tr>
  //       <td>{{ this.UH[4] + ' - ' + this.UH[5] }}</td>
  //       <td>{{uhml1[2]}}</td>
  //       <td></td>                     
  //       <td>{{ ui1[2] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[2] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                  
  //     </tr>   
  //      <tr>
  //       <td>{{ this.UH[6] + ' - ' + this.UH[7] }}</td>
  //       <td>{{uhml1[3]}}</td>
  //       <td></td>                     
  //       <td>{{ ui1[3] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[3] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //      <tr>
  //       <td>{{ this.UH[8] + ' - ' + this.UH[9] }}</td>
  //       <td>{{uhml1[4]}}</td>
  //       <td></td>                     
  //       <td>{{ ui1[4] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[4] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                   
  //     </tr> 
  //     <tr>
  //       <td> {{ this.UH[10] + ' - ' + this.UH[11] }} </td>
  //       <td>{{uhml1[5]}}</td>
  //       <td></td>                     
  //       <td>{{ ui1[5] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[5] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                        
  //     </tr>  
  //     <tr>
  //       <td>{{ this.UH[12] + ' - ' + this.UH[13] }}</td>
  //       <td>{{uhml1[6]}}</td>
  //       <td></td>                     
  //       <td>{{ ui1[6] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[6] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                  
  //     </tr>   
  //      <tr>
  //       <td>{{ this.UH[14] + ' - ' + this.UH[15] }}</td>
  //       <td>{{uhml1[7]}}</td>
  //       <td></td>                     
  //       <td>{{ ui1[7] }}</td>
  //       <td></td>                     
  //       <td>{{ str1[7] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                  
  //     </tr>   
  //      <tr>
  //       <td>{{ this.UH[16] + ' - ' + this.UH[17] }}</td>
  //       <td>{{uhml1[8]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[8]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[8] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                
  //     </tr> 
  //      <tr>
  //       <td>{{ this.UH[18] + ' - ' + this.UH[19] }}</td>
  //       <td>{{uhml1[9]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[9]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[9] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                   
  //     </tr> 
  //     <tr>
  //       <td> {{ this.UH[20] + ' - ' + this.UH[21] }} </td>
  //       <td>{{uhml1[10]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[10]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[10] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                         
  //     </tr>  
  //     <tr>
  //       <td>{{ this.UH[22] + ' - ' + this.UH[23] }}</td>
  //       <td>{{uhml1[11]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[11]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[11] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr>   
  //      <tr>
  //       <td>{{ this.UH[24] + ' - ' + this.UH[25] }}</td>
  //       <td>{{uhml1[12]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[12]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[12] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                   
  //     </tr>   
  //      <tr>
  //       <td>{{ this.UH[26] + ' - ' + this.UH[27] }}</td>
  //       <td>{{uhml1[13]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[13]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[13] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                         
  //     </tr> 
  //      <tr>
  //       <td>{{ this.UH[28] + ' - ' + this.UH[29] }}</td>
  //       <td>{{uhml1[14]}}</td>
  //       <td></td>                     
  //       <td>{{ui1[14]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[14] }}</td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                       
  //     </tr> 
  //     <tr>
  //       <td> {{ this.UH[30] + ' - ' + this.UH[31] }} </td>
  //       <td>{{uhml1[15]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[15] }}</td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                           
  //     </tr>  
  //     <tr>
  //       <td>{{ this.UH[32] + ' - ' + this.UH[33] }}</td>
  //       <td>{{uhml1[16]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[16] }}</td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                 
  //     </tr>    
  //     <tr>
  //       <td>{{ this.UH[34] + ' - LONGER'  }}</td>
  //       <td>{{uhml1[17]}}</td>
  //       <td></td>                     
  //       <td>{{ str1[17] }}</td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr>   
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr>
  //      <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr>
  //      <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr> 
  //     <tr>
  //       <td></td>
  //       <td></td>
  //       <td></td>                     
  //       <td></td>
  //       <td></td>                     
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td> 
  //       <td></td>                    
  //       <td></td>                      
  //     </tr>
  //   </tbody>
</script>


