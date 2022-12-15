<template>   
    <div class="pb-10">
      <div class="mt-3 w-full flex items-center justify-center">
        <button class="bg-blue-600 hover:bg-blue-700 text-white font-semibold p-1.5 rounded" v-on:click="fillArray()">
          Generar RECAP
        </button>
      </div>
      <div v-show="excel" class="mt-3 w-full flex items-center justify-center">
        <button class="bg-blue-600 hover:bg-blue-700 text-white font-semibold p-1.5 rounded" v-on:click="exportarExcel()">
          Exportar a excel
        </button>
      </div>
      <br>
       <div class="flex items-start justify-center h-screen">
      <table class="w-3/4 text-center">
      <thead class=" bg-blue-600 font-semibold text-white pt-2 pb-2 sticky top-0">
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
      <tbody v-if="!isTableEmpty">
        <tr v-for="(items, index) in baseArr" :key="index">
          <td class="bg-gray-300 text-black text-bold">{{items[0].T1}}</td>
          <td class="bg-gray-200 text-black text-bold">{{items[0].D1}}</td>
          <td class="bg-gray-300 text-black text-bold">{{items[0].T2}}</td>
          <td class="bg-gray-200 text-black text-bold">{{items[0].D2}}</td>
          <td class="bg-gray-300 text-black text-bold">{{items[0].T3}}</td>
          <td class="bg-gray-200 text-black text-bold">{{items[0].D3}}</td>
          <td class="bg-gray-300 text-black text-bold">{{items[0].T4}}</td>
          <td class="bg-gray-200 text-black text-bold">{{items[0].D4}}</td>
          <td class="bg-gray-300 text-black text-bold">{{items[0].T5}}</td>
          <td class="bg-gray-200 text-black text-bold">{{items[0].D5}}</td>
          <td class="bg-gray-300 text-black text-bold">{{items[0].T6}}</td>
          <td class="bg-gray-200 text-black text-bold">{{items[0].D6}}</td>
        </tr>
      </tbody>
      <div v-show="rendering" v-else class=" ml-auto mr-auto w-full h-full flex justify-center items-center">
       <svg aria-hidden="true" class="w-20 h-20 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
          <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
       </svg>
      </div>
      </table>
      
      </div>
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
    CLR: ['11-1',	'11-2',	'11-3',	'11-4',	'12-1',	'12-2',	'12-3',	'12-4',	'13-1',	'13-2',	'13-3',	'13-4',	'14-1',	'14-2',	'14-3',	'14-4',	'21-1',	'21-2',	'21-3',	'21-4',	'22-1',	'22-2',	'22-3',	'22-4',	'23-1',	'23-2',	'23-3',	'23-4',	'24-1',	'24-2',	'24-3',	'24-4',	'31-1',	'31-2',	'31-3',	'31-4',	'32-1',	'32-2',	'32-3',	'32-4',	'33-1',	'33-2',	'33-3',	'33-4',	'34-1',	'34-2',	'34-3',	'34-4',	'41-1',	'41-2',	'41-3',	'41-4',	'42-1',	'42-2',	'42-3',	'42-4',	'43-1',	'43-2',	'43-3',	'43-4',	'44-1',	'44-2',	'44-3',	'44-4',	'51-1',	'51-2',	'51-3',	'51-4',	'52-1',	'52-2',	'52-3',	'52-4',	'53-1',	'53-2',	'53-3',	'53-4',	'54-1',	'54-2',	'54-3',	'54-4',	'61-1',	'61-2',	'61-3',	'61-4',	'62-1',	'62-2',	'62-3',	'62-4',	'63-1',	'63-2',	'63-3',	'63-4'],
    TRSH: [1,	2,	3,	4,	5,	6,	7,	8, 9],
    baseArr: [],
    uhmlTotal: [],
    uiTotal: [],
    strTotal:[],
    sfiTotal:[],
    micTotal:[],
    clrTotal:[],
    rendering: false,
    excel: false
  }),

  methods: {
    fillArray(){      
      this.rendering = true
      axios.get('http://localhost:8000/api/Recap').then(res => { 
        this.hvi = res.data;  
       
        var i;      

        var pos1 = 0;
        var pos2 = 1;
         for(i = 0; i < this.UH.length/2; i++){             
            let arr = this.hvi.filter((x) => {
             return x.uhml >= this.UH[pos1] && x.uhml <= this.UH[pos2]
            })
            this.uhmlTotal.push(arr.length)
            pos1+=2
            pos2+=2
        }
       
       var pos1 = 0;
       var pos2 = 1;
        for(i = 0; i < this.UI.length/2; i++){             
            let arr = this.hvi.filter((x) => {
             return x.ui >= this.UI[pos1] && x.ui <= this.UI[pos2]
            })
            this.uiTotal.push(arr.length)            
            pos1+=2
            pos2+=2
        }

       var pos1 = 0;
       var pos2 = 1;
        for(i = 0; i < this.STR.length/2; i++){             
            let arr = this.hvi.filter((x) => {
             return x.strength >= this.STR[pos1] && x.strength <= this.STR[pos2]
            })
            this.strTotal.push(arr.length)
            pos1+=2
            pos2+=2
        }

       var pos1 = 0;
       var pos2 = 1;
        for(i = 0; i < this.SFI.length/2; i++){             
            let arr = this.hvi.filter((x) => {
             return x.sfi >= this.SFI[pos1] && x.sfi <= this.SFI[pos2]
            })
            this.sfiTotal.push(arr.length)
            pos1+=2
            pos2+=2
        }

       var pos1 = 0;
        for(i = 0; i < this.MIC.length; i++){             
            let arr = this.hvi.filter((x) => {
             return x.mic === this.MIC[pos1] 
            })
            this.micTotal.push(arr.length)
            pos1++
        }

          var pos1 = 0;
        for(i = 0; i < this.CLR.length; i++){             
            let arr = this.hvi.filter((x) => {
             return x.colorgrade === this.CLR[pos1] 
            })
            this.clrTotal.push(arr.length)
            pos1++
        }

        var tt1 = 0
        var tt2 = 1
        var tt3 = 0
        var dd = 0
        for(i= 0; i < 92; i++){
          let baseArrFill  = [ 
          {
           T1: `${this.UH[tt1]} - ${this.UH[tt2]}`, D1: this.uhmlTotal[dd],
           T2: `${this.UI[tt1]} - ${this.UI[tt2]}`, D2: this.uiTotal[dd], 
           T3: `${this.STR[tt1]} - ${this.STR[tt2]}`, D3: this.strTotal[dd],
           T4: `${this.SFI[tt1]} - ${this.SFI[tt2]}`, D4: this.sfiTotal[dd], 
           T5: this.MIC[tt3], D5: this.micTotal[dd], 
           T6: this.CLR[tt3], D6: this.clrTotal[dd] 
          }]              
                
          this.baseArr.push(baseArrFill )
          //   if(this.uhmlTotal[dd] === 99999){
          //   this.baseArr[tt3][0].T1 = `${this.SFI[tt1]} +`            
          // }
           if(typeof this.uhmlTotal[dd] === "undefined"){
           this.baseArr[tt3][0].T1 = ''            
          }

            if(typeof this.uiTotal[dd] === "undefined"){                       
            this.baseArr[tt3][0].T2 = ''
          }

            if(typeof this.strTotal[dd] === "undefined"){                       
            this.baseArr[tt3][0].T3 = ''
          }

          if(typeof this.sfiTotal[dd] === "undefined"){                       
            this.baseArr[tt3][0].T4 = ''
          }

          if(this.baseArr[tt3][0].T1.includes('99999')){
            this.baseArr[tt3][0].T1 = '1.36 - +'
          }

          if(this.baseArr[tt3][0].T2.includes('99999')){
            this.baseArr[tt3][0].T2 = '83 - +'
          }

          if(this.baseArr[tt3][0].T3.includes('99999')){
            this.baseArr[tt3][0].T3 = '30 - +'
          }

          tt1+=2
          tt2+=2
          tt3++
          dd++
        }
        
         this.excel= true 
       });    
    },
    
  },

  computed: {
    isTableEmpty(){
      return this.baseArr.length === 0;
    }
  }
}
</script>



