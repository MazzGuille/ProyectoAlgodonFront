<template>
  <div>
    <h1>
      Importar HVI
    </h1>
    <!-- <input id="archivoExcel" type="file" multiple @change="subirExcel()" /> -->

    <div>
      <input type="file" class="form-control" id="import" style="height:0%;" aria-describedby="imporExcel"
        v-on:change="imporExcel($event)">
    </div>
    <div>
      <button @click="HVIRecap()">Obtener Recap</button>
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
      <tbody>
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
    </table>
  </div>
</template>

<script>
import axios from 'axios';
import router from "@/router/index.js";

export default {
  data: () => ({
    items: [],
    datos: false,
    recap: false
  }),

  methods: {
    imporExcel(evt) {
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
              let MIC = x['Mic'];
              let COLORGRADE = x['ColorGrade'];
              let TRASHID = x['TrashID'];
              return { UHML, UI, STRENGTH, SFI, MIC, COLORGRADE, TRASHID };
            });
            self.items = restX2;
         
          });
        }
      }
    },

    HVIRecap() {
      let self = this;
      let jsons = {
        HVIList : self.items
      };
      axios.post('http://localhost:8000/api/HVI',jsons).then(res => {
        console.log(res);
        if (res.status === 200) alert("Se enviaron los datos correctamente.");
      });
    },    

    generarRecap() {
      router.push("/recap")
    }
  }

}
</script>
