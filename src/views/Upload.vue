<template>
  <div>
    <h1>
      Importar HVI
    </h1>
    <div>
      <input type="file" class="form-control" id="import" style="height:0%;" aria-describedby="imporExcel"
        v-on:change="imporExcel($event)">
    </div>
    <div>
      <input v-model="FileName" type="text"/>
      <button @click="HVIRecap()">Obtener Recap</button>
      <br>
      <button @click="test()">TEST</button>
    </div>

    <button v-if="datos" v-on:click="probarExcel()" class=" h-16 w-16 bg-blue-600">Exportar datos</button>
    <button v-if="recap" v-on:click="generarRecap()" class=" h-16 w-16 bg-red-600">Generar RECAP</button>
  </div>
  <div class="text-center flex items-start justify-center h-screen">
    <table class="table w-3/4">
      <thead>
        <tr>
          <th>#</th>
          <th class="w-32">UHML</th>
          <th class="w-32">UI</th>
          <th class="w-32">Strength</th>
          <th class="w-32">SFI</th>
          <th class="w-32">Mic</th>
          <th class="w-32">ColorGrade</th>
          <th class="w-32">TrashID</th>
        </tr>
      </thead>
      <tbody v-if="!isTableEmpty">
        <tr v-for="(item, im) in items" v-bind:key="item">          
          <td>{{ im + 1 }}</td>
          <td>{{ item.UHML }}</td>
          <td>{{ item.UI }}</td>
          <td>{{ item.STRENGTH }}</td>
          <td>{{ item.SFI }}</td>
          <td>{{ item.MIC }}</td>
          <td>{{ item.COLORGRADE }}</td>
          <td>{{ item.TRASHID }}</td>
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
</template>

<script>
import axios from 'axios';
import router from "@/router/index.js";

export default {
  data: () => ({
    items: [],
    FileName: '',
    datos: false,
    recap: false,
    rendering: false,
  }),

  methods: {
    imporExcel(evt) {
      this.rendering = true
      let self = this,
        excel = evt.target.files[0],
        data = [{
          "name": "jayanth",
          "data": "scd",
          "abc": "sdef"
        }];

      if (!window.XLSX) { console.error('Navegador no soporta XLSX'); return; };

      XLSX.utils.json_to_sheet(data, 'out.xlsx');
      if (excel) {      
        let fileReader = new FileReader();
        fileReader.readAsBinaryString(excel);

        fileReader.onload = (event) => {

          let data = event.target.result,
            workbook = XLSX.read(data, { type: "binary" });

          workbook.SheetNames.forEach(sheet => {
            let rowObject = XLSX.utils.sheet_to_row_object_array(workbook.Sheets[sheet]);            

            let restX2 = rowObject.map((x) => {
              let UHML = x['UHML'];
              let UI = x['UI'];
              let STRENGTH = x['Strength'];
              let SFI = x['SFI'];
              let MIC = x['MIC'];
              let COLORGRADE = x['ColorGrade'];
              let TRASHID = x['TrashID'];
              return { UHML, UI, STRENGTH, SFI, MIC, COLORGRADE, TRASHID };
            });
            self.items = restX2;
            console.log(self.items)      
          });
        }
      }
    },

    HVIRecap() {
      let self = this;
      let jsons = {
        HVIList : self.items,
        Title: self.FileName
      };

      axios.post('http://localhost:8000/api/HVI', jsons).then(res => {
        console.log(res);
        if (res.status === 200) alert("Se enviaron los datos correctamente.");
        location.reload()
      });
    }
  },

  computed: {
    isTableEmpty(){
      return this.items.length === 0;
    }
  }
}
</script>
<style scoped>
  th, h1 {
    position: sticky;
    top: 0;
  }
</style>