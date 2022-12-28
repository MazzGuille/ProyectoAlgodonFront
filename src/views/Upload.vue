<template>
  <div v-if="respuesta">
    <div class="pt-3 w-full flex justify-center items-center">
      <h1 class="text-blue-600 font-semibold">Importar HVI</h1>
      <!-- <button @click="logout()">DATOS USUARIO</button> -->
    </div>
    <div v-if="isLoading" class=" mx-auto h-18 w-96">
      <p class="text-blue-600 font-bold m-auto text-center">CARGANDO A LA BASE DE DATOS, ESTE PROCESO PODRIA TOMAR UNOS MINUTOS</p>
      <div class="mx-auto w-32">
        <svg aria-hidden="true" class="w-20 h-20 mx-auto mt-4 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
            <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
         </svg>
      </div>
    </div>
    <div
      class="
        p-0
        flex flex-col
        justify-around
        items-center
        mt-4
        h-16
        w-full
        ml-auto
        mr-auto
      "
    >
      <div class="">
        <label v-show="archivo" class="cursor-pointer text-white font-semibold" for="import"
          ><span
            class="bi bi-upload bg-blue-600 hover:bg-blue-700 p-2 rounded"
            data-mdb-ripple="true"
            data-mdb-ripple-color="light"
          >
            Seleccionar archivo
          </span></label
        >
        <input
          type="file"
          class="hidden"
          id="import"
          v-on:change="imporExcel($event)"
        />
      </div>
      <div
        v-show="recap"
        class="
          flex flex-col
          justify-around
          items-center
          mt-4
          h-16
          w-full
          ml-auto
          mr-auto
        "
      >
        <input v-model="FileName" type="text" class="hidden" id="xlsInput" />
        
          <button
            class="
              cursor-pointer
              text-white
              font-semibold
              bg-blue-600
              hover:bg-blue-700
              p-2
              rounded
            "
            @click="HVIRecap()"
          >
            Enviar datos
          </button >        
        
      </div>
    </div>
     <div v-if="recapLink" class=" mx-auto w-1/3 flex justify-center items-center">
            <button  class="
                cursor-pointer
                text-white
                font-semibold
                bg-blue-600
                hover:bg-blue-700
                p-2
                rounded
              ">
               <router-link :to="{ name: 'Recap' }">Ir a RECAP</router-link>
            </button>
          </div>
    <div class="text-center flex items-start justify-center h-screen">
      <table class="mt-5 w-3/4">
        <thead class="bg-blue-600 text-white pt-2 pb-2 sticky top-0">
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
            <td class="bg-gray-300 text-black">{{ im + 1 }}</td>
            <td class="bg-gray-200 text-black">{{ item.UHML }}</td>
            <td class="bg-gray-300 text-black">{{ item.UI }}</td>
            <td class="bg-gray-200 text-black">{{ item.STRENGTH }}</td>
            <td class="bg-gray-300 text-black">{{ item.SFI }}</td>
            <td class="bg-gray-200 text-black">{{ item.MIC }}</td>
            <td class="bg-gray-300 text-black">{{ item.COLORGRADE }}</td>
            <td class="bg-gray-200 text-black">{{ item.TRASHID }}</td>
          </tr>
        </tbody>
        <div
          v-show="rendering"
          v-else
          class="ml-auto mr-auto w-full h-full flex justify-center items-center"
        >
          <svg
            aria-hidden="true"
            class="
              w-20
              h-20
              text-gray-200
              animate-spin
              dark:text-gray-600
              fill-blue-600
            "
            viewBox="0 0 100 101"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
              fill="currentColor"
            />
            <path
              d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
              fill="currentFill"
            />
          </svg>
        </div>
      </table>
    </div>
  </div>
  <div
    class="
      min-h-screen
      w-full
      bg-blue-600
      text-white
      font-extrabold
      flex
      justify-center
      items-center
    "
    v-else
  >
    <p class="text-4xl">
      PARA TENER ACCESO A LA APLICACION DEBES INICIAR SESION
    </p>
  </div>
</template>

<script>
import axios from "axios";
import router from "@/router/index.js";
import { onMounted, ref } from "vue";

export default {
  data: () => ({
    items: [],
    FileName: "",
    datos: false,
    recap: false,
    rendering: false,
    isLoading: false,
    recapLink: false,
    archivo: true
  }),

  methods: {
    imporExcel(evt) {
      this.rendering = true;
      let self = this,
        excel = evt.target.files[0],
        data = [
          {
            name: "jayanth",
            data: "scd",
            abc: "sdef",
          },
        ];

      if (!window.XLSX) {
        console.error("Navegador no soporta XLSX");
        return;
      }

      XLSX.utils.json_to_sheet(data, "out.xlsx");
      if (excel) {
        let fileReader = new FileReader();
        fileReader.readAsBinaryString(excel);

        fileReader.onload = (event) => {
          let data = event.target.result,
            workbook = XLSX.read(data, { type: "binary" });

          workbook.SheetNames.forEach((sheet) => {
            let rowObject = XLSX.utils.sheet_to_row_object_array(
              workbook.Sheets[sheet]
            );

            let restX2 = rowObject.map((x) => {
              let UHML = x["UHML"];
              let UI = x["UI"];
              let STRENGTH = x["Strength"];
              let SFI = x["SFI"];
              let MIC = x["MIC"];
              let COLORGRADE = x["ColorGrade"];
              let TRASHID = x["TrashID"];
              return { UHML, UI, STRENGTH, SFI, MIC, COLORGRADE, TRASHID };
            });
            self.items = restX2;
            console.log(self.items);
          });
        };
        this.recap = true;
      }
    },

   async HVIRecap() {
      let self = this;
      let jsons = {
        HVIList: self.items,
        Title: self.FileName,
      };
      this.isLoading = true;
    try {
     await axios.post("http://localhost:8000/api/HVI", jsons).then((res) => {
        console.log(res);
        if (res.status === 200) alert("Se enviaron los datos correctamente.");
        this.HVIList = [];
        // router.push("/recap");
      });      
    } catch (error) {
      console.error(error)
    }      
    this.isLoading = false;
    this.recapLink = true;
    this.recap = false;
    this.archivo = false
    },

    // datosUsuario(){
    // let url = 'http://localhost:8000/api/User/user'
    // let config = {
    // headers:{'Content-Type': 'application/json'},
    // withCredentials: true
    // }
    //  this.probando = axios.get(url, config)
    //  console.log(this.probando);
    //  this.respuesta = true
    // }
  },

  computed: {
    isTableEmpty() {
      return this.items.length === 0;
    },
  },

  setup() {
    let respuesta = ref(false);
    onMounted(() => {
      let url = "http://localhost:8000/api/User/user";
      let config = {
        headers: { "Content-Type": "application/json" },
        withCredentials: true,
      };
      axios.get(url, config).then((res) => {
        console.log(res);
        if (res.status === 200) {
          respuesta.value = true;
        }
      });
    });

    const logout = async () => {
      let url = "http://localhost:8000/api/User/logout";
      let config = {
        headers: { "Content-Type": "application/json" },
        withCredentials: true,
      };
      axios.post(url, config).then((res) => {
        console.log(res);
        // if(res.status === 200){
        //   router.push("/")
        // }
      });
    };

    return {
      respuesta,
      logout,
    };
  },
};
</script>
