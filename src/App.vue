<template>
  <div class="weather">

    <div>
      <p>Zefir</p>
    </div>

    <div>
    <!-- <pre>{{ jsonData }}</pre> -->
  </div>

    <div class="container">

      <div class="card weather-form .first">
        <label for="systemName">Система</label>
        <input type="text" name="systemName" id="systemName" class="weather-form_input" v-model="systemName">
        <label for="typeFan">Тип</label>
        <select name="typeFan" id="typeFan" class="weather-form_input" v-model="typeFan">
          <option value="radial">Радиальный</option>
          <option value="roofUp">Крышный вверх</option>
          <option value="roofSide">Крышный в стороны</option>
          <option value="vkk">Канальный круглый</option>
          <option value="vkp">Канальный прямоугольный</option>
          <option value="vkFree">Канальный с колесом свободного вращения</option>
          <option value="dust">Пылевой</option>
          <option value="free">Свободное колесо</option>
          <option value="axial">Осевой</option>
          <option value="roofAxial">Крышный осевой</option>
          <option value="tdm">Тягодутьевая машина</option>
        </select>
        <label for="serieFan">Тип</label>
        <select name="serieFan" id="serieFan" class="weather-form_input" v-model="serieFan">
        <option value="all">Все</option>
      <option v-for="item in filteredItems" :key="item.id" :value="item.serieFan">
        {{ item.serieFan }}
      </option>
    </select>
        <label for="materialDesign">Исполнение</label>
        <select name="materialDesign" id="materialDesign" class="weather-form_input" v-model="materialDesign">
          <option value="80" selected="selected">Общепромышленный</option>
          <option value="80_corrosionResistant">Коррозионностойкий</option>
          <option value="80_acidProof">Кислотостойкий</option>
          <option value="80_explosionProof">Взрывозащищенный категории IIВ</option>
          <option value="80_corrosionResistant_explosionProof">Взрывозащищенный категории IIВ и коррозионностойкий</option>
          <option value="80_explosionProof_acidProof">Взрывозащищенный категории IIВ и кислотостойкий</option>
          <option value="80_explosionProof2">Взрывозащищенный категории IIС</option>
          <option value="80_explosionProof2_corrosionResistant">Взрывозащищенный категории IIС и коррозионностойкий</option>
          <option value="80_explosionProof2_acidProof">Взрывозащищенный категории IIС и кислотостойкий</option>
          <option value="200">Теплостойкий</option>
          <option value="200_corrosionResistant">Коррозионностойкий и теплостойкий</option>
          <option value="400_smokeExhaust">Дымоудаление 400</option>
          <option value="600_smokeExhaust">Дымоудаление 600</option>
          <option value="600_smokeExhaust_explosionProof">Дымоудаление 600 взрывозащищенный категории IIВ</option>
          <option value="explosionProof_aluminum">Взрывозащищенный из алюминиевых сплавов</option>
        </select>
        <label for="climat">Климатическое исполнение двигателя</label>
        <select name="climat" id="climat" class="weather-form_input" v-model="climat">
          <option value="u1" selected="selected">У1</option>
          <option value="u2">У2</option>
          <option value="u3">У3</option>
          <option value="uhl1">УХЛ1</option>
          <option value="uhl2">УХЛ2</option>
          <option value="uhl3">УХЛ3</option>
          <option value="t1">Т1</option>
          <option value="t2">Т2</option>
          <option value="om2">ОМ2</option>
        </select>
        <label for="flow">Производительность, м3/ч?</label>
        <input type="number" name="flow" id="flow" class="weather-form_input" @keyup.enter="sendRequest" v-model="flow">
        <label for="pressure">Давление, Па</label>
        <input type="number" name="pressure" id="pressure" class="weather-form_input" @keyup.enter="sendRequest" v-model="pressure">
        <label for="temperature">Температура, °С</label>
        <input type="number" name="temperature" id="temperature" class="weather-form_input" @keyup.enter="sendRequest"
          v-model="temperature">
          <label for="fluctuationPercentUp">Возможное отклонение при подборе без регулирования оборотов вверх, %</label>
          <input type="number" name="fluctuationPercentUp" id="fluctuationPercentUp" class="weather-form_input"  v-model="fluctuationPercentUp">
          <label for="fluctuationPercentDown">Возможное отклонение при подборе без регулирования оборотов вверх, %</label>
          <input type="number" name="fluctuationPercentDown" id="fluctuationPercentDown" class="weather-form_input"  v-model="fluctuationPercentDown">
          <label for="height">Высота над уровнем моря, м</label>
          <input type="number" name="height" id="height" class="weather-form_input"  v-model="height">
          <label for="calcType">Тип расчета</label>
        <select name="calcType" id="calcType" class="weather-form_input" v-model="calcType">
          <option value="default" selected="selected">По умолчанию</option>
          <option value="stat">Статический</option>
          <option value="full">Полный</option>
        </select>
        <label for="startType">Тип запуска</label>
        <select name="startType" id="startType" class="weather-form_input" v-model="startType">
          <option value="directStart" selected="selected">Прямой пуск</option>
          <option value="smoothStart">Плавный пуск</option>
          <option value="frequencyConverter">Частотный преобразователь</option>
        </select>
        <label for="fifth_type">Исполнение 5</label>
        <input type="checkbox" name="fifth_type" id="fifth_type" class="weather-form_input" v-model="fifth_type">
        <label for="note">Примечание</label>
         <input type="text" name="note" id="note" class="weather-form_input" v-model="note">     
        <button class="weather-form_btn" @click="sendRequest">Подбор</button>
      </div>

      <div class="card weather-form .second">
        <button class="weather-form_btn" v-for="fan in ArrayOfFans" @click="showGraph(fan)">{{ fan.name }}</button>
      </div>

      <div class="card weather-form .third" id="source-html">
        <div v-show="ArrayOfFans.length && this.workPoint.flow">
          <apexchart id="chart" width="1500" type="line" :options="chartOptions" :series="series"></apexchart>
          <button class="weather-form_btn" @click="getPDF">сохранить PDF</button>
        </div>

        <p v-if="ArrayOfFans.length && this.workPoint.flow">
          Производительность: {{ this.workPoint.flow }} м3/ч
        </p>
        <p v-if="ArrayOfFans.length && this.workPoint.flow">
          Давление: {{ this.workPoint.pressure }} Па
        </p>
      </div>

    </div>
 

    <div class="weather-bg">
      <div>
        <img src="" alt="">
      </div>
    </div>
  </div>
</template>

<script>


import vents from '../vents.json'
import { jsPDF } from "jspdf";
import * as XLSX from 'xlsx';

export default {

  data() {
    return {
      selectedItem: '',
      jsonData: null,
      series: [
        {
          name: 'Point',
          type: 'scatter',
          data: [{
            x: 0,
            y: 0
          },]
        },
        {
          name: 'Pv',
          type: 'line',
          data: []
        },
        {
          name: 'System',
          type: 'line',
          data: []
        },
        {
          name: 'ReqPoint',
          type: 'scatter',
          data: [{
            x: 0,
            y: 0
          },]
        },
        {
          name: 'Pv5',
          type: 'line',
          data: []
        }],
      chartOptions: {
        chart: {
          id: 'chart',
          type: 'line',
          zoom: {
            enabled : false
          },
          toolbar:{
            show: false
          }
        },
        stroke: {
          width: [10, 5, 1, 10],
          curve: 'monotoneCubic',
          dashArray: [0, 0, 0, 0]
        },
        fill: {
          type: 'solid',
        },
        markers: {
          size: [2, 2]
        },
        tooltip: {
          shared: false,
          intersect: true,
        },
        legend: {
          show: true
        },
        xaxis: {
          type: 'numeric',


        },
        yaxis: {
          max: function (max) { return Math.floor(max * 0.012) * 100 },
          min: function (min) { return Math.floor(min / 100) * 100 },
          labels: {
            show: true,
            offsetX: -10,
            offsetY: 0,
            rotate: 0,
            formatter: (val) => { return Math.floor(val) },
          },
          type: 'numeric',
          tickAmount: undefined
        },
      },
      typeFan: "radial",
      serieFan:"all",
      systemName: "",
      tickAmount: 5,
      vents: [],
      flow: 30000,
      pressure: 2000,
      temperature: 20,
      fansArray: [],
      ArrayOfFans: [],
      systemCurve: [],
      fanCurve: [],
      requriedPoint: {
        flow: this.flow,
        pressure: this.pressure
      },
      workPoint: {
        flow: 0,
        pressure: 0
      }
    };
  },

  async created() {
    // Загружаем файл при создании компонента
    await this.loadExcelFile();
  },
  mounted() {
    this.fansArray = convertData(vents)
    function convertData(vents) {
      var fans = []
      for (const vent in vents) {
        if (Object.prototype.hasOwnProperty.call(vents, vent)) {
          const element = vents[vent];
          // console.log(element);
          var tempObj = {}
          tempObj.name = element["Модель колеса"]
          tempObj.speed = element["Макс. частота, об/мин"]
          tempObj.minFlow = element["Мин. расход, м3/ч"]
          tempObj.maxFlow = element["Макс. расход, м3/ч"]
          tempObj.type = element["Тип вентилятора"]
          tempObj.points = []

          var point1 = {
            flow: element["Расход 1, м<sup>3</sup>/ч"],
            pressure: element["Давление 1, Па"],
            kpd: element["КПД значение 1, % / Мощность в точке 1, кВт"]
          }
          var point2 = {
            flow: element["Расход 2, м<sup>3</sup>/ч"],
            pressure: element["Давление 2, Па"],
            kpd: element["КПД значение 2, % / Мощность в точке 2, кВт"]
          }
          var point3 = {
            flow: element["Расход 3, м<sup>3</sup>/ч"],
            pressure: element["Давление 3, Па"],
            kpd: element["КПД значение 3, % / Мощность в точке 3, кВт"]
          }
          var point4 = {
            flow: element["Расход 4, м<sup>3</sup>/ч"],
            pressure: element["Давление 4, Па"],
            kpd: element["КПД значение 4, % / Мощность в точке 4, кВт"]
          }
          var point5 = {
            flow: element["Расход 5, м<sup>3</sup>/ч"],
            pressure: element["Давление 5, Па"],
            kpd: element["КПД значение 5, % / Мощность в точке 5, кВт"]
          }
          var point6 = {
            flow: element["Расход 6, м<sup>3</sup>/ч"],
            pressure: element["Давление 6, Па"],
            kpd: element["КПД значение 6, % / Мощность в точке 6, кВт"]
          }
          var point7 = {
            flow: element["Расход 7, м<sup>3</sup>/ч"],
            pressure: element["Давление 7, Па"],
            kpd: element["КПД значение 7, % / Мощность в точке 7, кВт"]
          }
          var point8 = {
            flow: element["Расход 8, м<sup>3</sup>/ч"],
            pressure: element["Давление 8, Па"],
            kpd: element["КПД значение 8, % / Мощность в точке 8, кВт"]
          }
          var point9 = {
            flow: element["Расход 9, м<sup>3</sup>/ч"],
            pressure: element["Давление 9, Па"],
            kpd: element["КПД значение 9, % / Мощность в точке 9, кВт"]
          }
          var point10 = {
            flow: element["Расход 10, м<sup>3</sup>/ч"],
            pressure: element["Давление 10, Па"],
            kpd: element["КПД значение 10, % / Мощность в точке 10, кВт"]
          }
          if (parseInt(point1.flow)) {
            tempObj.points.push(point1)
          }
          if (parseInt(point2.flow)) {
            tempObj.points.push(point2)
          }
          if (parseInt(point3.flow)) {
            tempObj.points.push(point3)
          }
          if (parseInt(point4.flow)) {
            tempObj.points.push(point4)
          }
          if (parseInt(point5.flow)) {
            tempObj.points.push(point5)
          }
          if (parseInt(point6.flow)) {
            tempObj.points.push(point6)
          }
          if (parseInt(point7.flow)) {
            tempObj.points.push(point7)
          }
          if (parseInt(point8.flow)) {
            tempObj.points.push(point8)
          }
          if (parseInt(point9.flow)) {
            tempObj.points.push(point9)
          }
          if (parseInt(point10.flow)) {
            tempObj.points.push(point10)
          }

          // console.log(tempObj);

          fans.push(tempObj)
        }
      }
      return fans

    }


  },
  computed: {
    // Фильтруем элементы в зависимости от выбранной категории
    filteredItems() {
      console.log(this.typeFan);
      
      return this.jsonData.filter((item) => item.typeFan === this.typeFan);
    },
  },
  watch: {
    // Отслеживаем изменение выбранной категории
    typeFan(newCategory) {
      if (newCategory) {
        // Устанавливаем первый элемент из filteredItems как выбранный по умолчанию
        this.serieFan = "all";
      } else {
        // Если категория не выбрана, сбрасываем выбранный элемент
        // this.serieFan = '';
      }
    },
  },
  methods: {
    sendRequest() {

      var flow = this.flow;
      var pressure = this.pressure;
      var system = pressure / (flow * flow)

      var ArrayOfFans = []

      for (let index = 0; index < this.fansArray.length; index++) {


        checkFan(this.fansArray[index], flow, pressure, this.type, this.temperature)

        if (ArrayOfFans.length == 0) {
          this.workPoint.flow = 0
          this.workPoint.pressure = 0
        }
      }

      this.ArrayOfFans = ArrayOfFans;

      function makeFanCurve(fan, dots) {
        // console.log(fan);

        var result = [];
        let step = (fan.maxFlow - fan.minFlow) / dots //шаг по Х

        for (let i = 0; i <= dots; i++) {
          let factor = 0
          for (let k = 0; k < fan.points.length; k++) {
            let li = 1
            for (let j = 0; j < fan.points.length; j++) {
              if (k == j) continue
              else {
                li = li * (((fan.minFlow + i * step) - fan.points[j].flow) / (fan.points[k].flow - fan.points[j].flow))
              }
            }
            factor = factor + fan.points[k].pressure * li

          }
          var currentPoint = {
            x: fan.minFlow + i * step,
            y: factor
          }

          result.push(currentPoint) //Массив значений Y
        }

        return result
      }

      function checkFan(fan, flow, pressure, type, temperature) {
        for (let i = 0; i < fan.points.length; i++) {
          fan.points[i].pressure = fan.points[i].pressure * (293 / (273 + temperature))

        }
        if (fan.minFlow > flow || fan.maxFlow < flow) {
          return false
        }

        if (type != fan.type) {
          return false
        }
        else {
          // console.log(fan);

          var currentFanCurve = makeFanCurve(fan, 200)
          for (let index = 0; index < currentFanCurve.length; index++) {
            if (currentFanCurve[index].y > (currentFanCurve[index].x * currentFanCurve[index].x * system)) {
              continue
            } else if (index != 0) {
              var possibleFan = {
                curve: currentFanCurve,
                workPoint: {
                  flow: currentFanCurve[index].x,
                  pressure: currentFanCurve[index].x * currentFanCurve[index].x * system
                },
                points: fan.points,
                name: fan.name,
                speed: fan.speed,
                maxValueX: fan.maxFlow * 1.5

              }
               if ((possibleFan.workPoint.pressure>pressure*1.2)||(possibleFan.workPoint.pressure<pressure) ) {
                break
               }

              ArrayOfFans.push(possibleFan)
              return true
            }

            break
          }
          return false
        }
      }
    },
    async loadExcelFile() {
      try {
        // Указываем путь к файлу в папке public
        const url = 'data.xlsx';

        // Загружаем файл с сервера
        const response = await fetch(url);
        const arrayBuffer = await response.arrayBuffer(); // Получаем бинарные данные

        // Читаем данные из Excel
        const workbook = XLSX.read(arrayBuffer, { type: 'array' }); // Чтение из бинарных данных
        const firstSheetName = workbook.SheetNames[0];
        const worksheet = workbook.Sheets[firstSheetName];

        // Преобразуем данные в JSON
        const rawData = XLSX.utils.sheet_to_json(worksheet, { header: 1 });

        // Обрабатываем данные
        if (rawData.length > 0) {
          const headers = rawData[0]; // Первая строка — заголовки (ключи объектов)
          const rows = rawData.slice(1); // Остальные строки — данные

          // Создаем массив объектов
          this.jsonData = rows.map((row) => {
            const obj = {};
            headers.forEach((header, columnIndex) => {
              // Удаляем null и пустые строки
              if (row[columnIndex] !== null && row[columnIndex] !== "") {
                obj[header] = row[columnIndex];
              }
            });
            return obj;
          });
        } else {
          console.error('Файл не содержит данных');
        }
      } catch (error) {
        console.error('Ошибка при загрузке файла:', error);
      }
    },
getPDF(){
  const chart = window.ApexCharts.getChartByID('chart')
  const dataURI = chart.dataURI({scale:1}).then(({ imgURI, blob }) => {
  // const { jsPDF } = window.jspdf
  const pdf = new jsPDF();
  pdf.addImage(imgURI, 'PNG', 0, 0);
  pdf.save("pdf-chart.pdf");
})
},
    showGraph(fan) {

      const chart = window.ApexCharts.getChartByID('chart')
      console.log(chart);
      
      
      this.workPoint.flow = Math.floor(fan.workPoint.flow) 
      this.workPoint.pressure = Math.floor(fan.workPoint.pressure)
      // console.log(fan);
      this.series[0].data[0].x = fan.workPoint.flow
      this.series[0].data[0].y = fan.workPoint.pressure
      this.series[2].data = makeSystemCurve(fan)
      this.series[3].data[0] = {
        x: this.flow,
        y: this.pressure
      }
      var checkCurve = []

      for (let i = 0; i < fan.points.length; i++) {
        var tempPoint = {
          x: fan.points[i].flow,
          y: fan.points[i].pressure
        }
        checkCurve.push(tempPoint)
      }
      
      this.series[1].data = checkCurve
      // this.updateTheme(fan)
      chart.updateOptions({
        chart: {
          // offsetY: 100,
          // offsetX: 100,
          height: 700,
          width:700,
          id:'chart',
          type: 'line',
          zoom:{
            enabled:false
          },
          toolbar:{
            show:false
          }
        },
      })
      
      function makeSystemCurve(fan) {
        var system = fan.workPoint.pressure / (fan.workPoint.flow * fan.workPoint.flow)
        var systemCurve = []

        for (let index = 0; index < fan.curve.length; index++) {
          var systemCurvePoint = {
            x: fan.curve[index].x,
            y: fan.curve[index].x * fan.curve[index].x * system
          }

          if (systemCurvePoint.x > fan.workPoint.flow * 1.05) {
            break
          }
          systemCurve.push(systemCurvePoint)
        }

        return systemCurve
      }
    },
    updateTheme(fan) {
      this.chartOptions = {
        // chart: {
        //   // offsetY: 100,
        //   // offsetX: 100,
        //   height: 350,
        //   width:350,
        //   id:'chart',
        //   type: 'line',
        //   zoom:{
        //     enabled:false
        //   },
        //   toolbar:{
        //     show:false
        //   }
        // },
        grid:{
          xaxis: {
        lines: {
            show: true
        }
    },   
    yaxis: {
        lines: {
            show: true
        }
    },  
        },
        // fill: {
        //   type: 'solid',
        // },
        // markers: {
        //   size: [2, 2]
        // },
        tooltip: {
          shared: false,
          intersect: true,
        },
        legend: {
          show: true
        },
        xaxis: {
          type: 'numeric',
          // stepSize: 1000,
          min: fan.points[0].flow * 0.9-(fan.points[0].flow * 0.9)%1000,
          max: fan.points[fan.points.length - 1].flow * 1.1-(fan.points[fan.points.length - 1].flow * 1.1)%1000+1000,
          tickAmount: 92.
        },
        yaxis: {
          max: function (max) { return max-max%1000+1000},
          min: function (min) { return min-min%1000 },
          labels: {
            show: true,
            offsetX: -10,
            offsetY: 0,
            rotate: 0,
            formatter: (val) => { return Math.floor(val) },
          },
          type: 'numeric',
          tickAmount: 10
        },
        stroke: {
          width: [10, 5, 1, 10],
          curve: 'monotoneCubic',
          dashArray: [0, 0, 0, 0]
        }
      }
    }
  }
}

</script>