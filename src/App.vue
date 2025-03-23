<template>
  <div class="weather">

    <div>
      <p>Zefir</p>
    </div>

    <div>
    <!-- <pre>{{ jsonData }}</pre> -->
  </div>

    <div class="container">

      <div class="column column-1">
        <label for="systemName">Система</label>
        <input type="text" name="systemName" id="systemName" class="weather-form_input" v-model="systemName">
        <label for="typeFan">Тип</label>
        <select name="typeFan" id="typeFan" class="weather-form_input" v-model="typeFan">
          <option value="Радиальный">Радиальный</option>
          <option value="Крышный вверх">Крышный вверх</option>
          <option value="Крышный в стороны">Крышный в стороны</option>
          <option value="Канальный круглый">Канальный круглый</option>
          <option value="Канальный прямоугольный">Канальный прямоугольный</option>
          <option value="Канальный с колесом свободного вращения">Канальный с колесом свободного вращения</option>
          <option value="Пылевой">Пылевой</option>
          <option value="Свободное колесо">Свободное колесо</option>
          <option value="Осевой">Осевой</option>
          <option value="Крышный осевой">Крышный осевой</option>
          <option value="Тягодутьевая машина">Тягодутьевая машина</option>
        </select>
        <label for="serieFan">Серия</label>
        <select name="serieFan" id="serieFan" class="weather-form_input" v-model="serieFan">
        <option value="all">Все</option>
      <option v-for="item in filteredItems" :key="item.id" :value="item.serieFan">
        {{ item.serieFan }}
      </option>
    </select>
        <label for="materialDesign">Исполнение</label>
        <select name="materialDesign" id="materialDesign" class="weather-form_input" v-model="materialDesign">
          <option value="Общепромышленный" selected="selected">Общепромышленный</option>
          <option value="Коррозионностойкий">Коррозионностойкий</option>
          <option value="Кислотостойкий">Кислотостойкий</option>
          <option value="Взрывозащищенный категории IIВ">Взрывозащищенный категории IIВ</option>
          <option value="Взрывозащищенный категории IIВ и коррозионностойкий">Взрывозащищенный категории IIВ и коррозионностойкий</option>
          <option value="Взрывозащищенный категории IIВ и кислотостойкий">Взрывозащищенный категории IIВ и кислотостойкий</option>
          <option value="Взрывозащищенный категории IIС">Взрывозащищенный категории IIС</option>
          <option value="Взрывозащищенный категории IIС и коррозионностойкий">Взрывозащищенный категории IIС и коррозионностойкий</option>
          <option value="Взрывозащищенный категории IIС и кислотостойкий">Взрывозащищенный категории IIС и кислотостойкий</option>
          <option value="Теплостойкий">Теплостойкий</option>
          <option value="Коррозионностойкий и теплостойкий">Коррозионностойкий и теплостойкий</option>
          <option value="Дымоудаление 400">Дымоудаление 400</option>
          <option value="Дымоудаление 600">Дымоудаление 600</option>
          <option value="Дымоудаление 600 взрывозащищенный категории IIВ">Дымоудаление 600 взрывозащищенный категории IIВ</option>
          <option value="Взрывозащищенный из алюминиевых сплавов">Взрывозащищенный из алюминиевых сплавов</option>
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
          <label for="fluctuationPercentDown">Возможное отклонение при подборе без регулирования оборотов вниз, %</label>
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

      <div class="column column-2">
        <button class="weather-form_btn" v-for="fan in ArrayOfFans" @click="showGraph(fan)" :style="{ backgroundColor: fan.color}">{{ fan.name }}</button>
      </div>

      <div class="column column-3" id="source-html">
        <div v-show="ArrayOfFans.length && this.workPoint.flow">
          <apexchart id="chart"  style="width: 100%; height: 400px;" type="line" :options="chartOptions" :series="series"></apexchart>
          <button class="weather-form_btn" @click="getPDF">сохранить PDF</button>
        </div>

        <p v-if="ArrayOfFans.length && this.workPoint.flow">
          Производительность: {{ this.workPoint.flow }} м3/ч
        </p>
        <p v-if="ArrayOfFans.length && this.workPoint.flow">
          Давление: {{ this.workPoint.pressure }} Па
        </p>
        <p v-if="ArrayOfFans.length && this.workPoint.flow">
          Скорость: {{ this.workPoint.speed }} об/мин
        </p>
        <p v-if="ArrayOfFans.length && this.workPoint.flow">
          Мощность: {{ this.workPoint.reqPower }} кВт
        </p>
        <p v-if="ArrayOfFans.length && this.workPoint.flow&&this.note">
          Примечание: {{ this.note }}
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


import { jsPDF } from "jspdf";
import * as XLSX from 'xlsx';

export default {

  data() {
    return {
      selectedItem: '',
      jsonData: null,
      typeData:null,
      motorData:null,
      motorPowerList:[0.12,0.18,0.25,0.37,0.55,0.75,1.1,1.5,2.2,3,4,5.5,7.5,11,15,18.5,22,30,37,45,55,75,90,110,132,160,200,250,315,355,400],
      motorSpeedList:[750,1000,1500,3000],
      series: [
        {
          name: 'Point',
          type: 'line',
          data: [{
            x: 0,
            y: 0
          },],

        },
        {
          name: 'Pv',
          type: 'line',

          data: [],
          markers: {
          size: 0 // Убирает точки
          }
        },
        {
          name: 'ReqPoint',
          type: 'line',
          data: [{
            x: 0,
            y: 0
          },],
          
        },
        {
          name: 'System',
          type: 'line',
          data: [],
          markers: {
    size: 0 // Убирает точки
  }
        },
        {
          name: 'Pv5',
          type: 'line',
          data: []
        }],
        grid:{
          show: true, // Включить сетку
          borderColor: '#90A4AE', // Цвет границы сетки
        },
      chartOptions: {
        chart: {
          id: 'chart',
          // type: 'line',

          zoom: {
            enabled : false
          },
          toolbar:{
            show: false
          }
        },

        stroke: {
          width: [10, 5, 10, 1],
          curve: 'monotoneCubic',
          dashArray: [0, 0, 0, 0]
        },
        fill: {
          type: 'solid',
        },
        markers: {
          size: [0, 0]
        },
        tooltip: {
          shared: false,
          intersect: true,
        },
        legend: {
          show: false
        },
        xaxis: {
          grid: {
    lines: {
      show: true, // Включить вертикальные линии сетки
      // color: '#90A4AE', // Цвет линий
    }
  },
          type: 'numeric',
          forceNiceScale: true,
          tickAmount:20
          // max: 50000,
        },
        yaxis: {
          grid:{
            lines: {
      show: true, // Включить вертикальные линии сетки
      // color: '#90A4AE', // Цвет линий
    }
          },
          labels: {
            show: true,
            offsetX: -10,
            offsetY: 0,
            rotate: 0,
            // forceNiceScale: true,
            formatter: (val) => { return Math.floor(val) },
          },
          type: 'numeric',
          tickAmount: undefined
        },
      },
      typeFan: "Радиальный",
      materialDesign:'Общепромышленный',
      climat:'u1',
      fluctuationPercentUp: 20,
      fluctuationPercentDown:0,
      height:0,
      calcType: 'default',
      startType:'directStart',
      fifth_type: false,
      note:'',
      serieFan:"all",
      systemName: "",
      density: 1.293,
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
    await this.loadExcelFile('data.xlsx', 'typeData',false)
    await this.loadExcelFile('Таблица двигателей.xlsx', 'motorData',false)
    await this.loadExcelFile('vents.xlsx', 'fansArray', true)
    },
  async mounted() {
  },
  computed: {
    // Фильтруем элементы в зависимости от выбранной категории
    filteredItems() {
      return this.typeData.filter((item) => item.typeFan === this.typeFan);
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
      var atmPressure = 101325*Math.exp(-0.029*9.81*this.height/(8.314*(this.temperature+273)))
      var density = this.density*(273/(273+this.temperature))*(atmPressure/101325)
      // console.log(density);
      
      var system = pressure / (flow * flow)

      var ArrayOfFans = []

      for (let index = 0; index < this.fansArray.length; index++) {

        
        
        checkFan(this.fansArray[index], flow, pressure, this.typeFan, 
        this.temperature, this.serieFan, this.materialDesign, this.fluctuationPercentUp, this.fluctuationPercentDown, 
        density, this.fifth_type,this.startType, this.calcType, this.motorPowerList, this.motorSpeedList, this.motorData)

        if (ArrayOfFans.length == 0) {
          this.workPoint.flow = 0
          this.workPoint.pressure = 0
        }
      }

      this.ArrayOfFans = ArrayOfFans;

      
      function makeFanCurve(fan, dots,density, calcType) {
        // console.log(fan);

        var result = [];
        let step = (fan.maxFlow - fan.minFlow) / dots //шаг по Х

        for (let i = 0; i <= dots; i++) {
          let factorPressure = 0
          let factorKpd = 0
          for (let k = 0; k < fan.points.length; k++) {
            let li = 1
            for (let j = 0; j < fan.points.length; j++) {
              if (k == j) continue
              else {
                li = li * (((fan.minFlow + i * step) - fan.points[j].flow) / (fan.points[k].flow - fan.points[j].flow))
              }
            }
            factorPressure = factorPressure + fan.points[k].pressure * li
            factorKpd = factorKpd + fan.points[k].kpd * li
          }

          if (calcType == "stat"&&fan["Тип внесенного графика"]=="Полный") {
              var pdv = 1.204*((fan.minFlow + i * step)/fan["Площадь выходного отверстия (не для вентиляторов с безразмерными характеристиками), м2"]/3600)*((fan.minFlow + i * step)/fan["Площадь выходного отверстия (не для вентиляторов с безразмерными характеристиками), м2"]/3600)/2
              
              factorPressure = factorPressure-pdv
            }


          var currentPoint = {
            x: fan.minFlow + i * step,
            y: factorPressure*density/1.204,
            kpd: factorKpd, 
            name: fan.name
          }

          result.push(currentPoint) //Массив значений Y
        }

        return result
      }
//       function makeFanCurve(fan, dots) {
//         // console.log(fan);
//         const n = fan.points.length;
//         let sumX = 0, sumY = 0, sumX2 = 0, sumX3 = 0, sumX4 = 0, sumXY = 0, sumX2Y = 0;
//         for (let i = 0; i < n; i++) {
//         const xi = fan.points[i].flow-0;
//         const yi = fan.points[i].pressure-0;
//         sumX += xi;
//         sumY += yi;
//         sumX2 += xi * xi;
//         sumX3 += xi * xi * xi;
//         sumX4 += xi * xi * xi * xi;
//         sumXY += xi * yi;
//         sumX2Y += xi * xi * yi;
//     }
// // Система уравнений для квадратичной аппроксимации:
//     // [ sumX4, sumX3, sumX2 ] [ a ]   [ sumX2Y ]
//     // [ sumX3, sumX2, sumX  ] [ b ] = [ sumXY  ]
//     // [ sumX2, sumX,  n     ] [ c ]   [ sumY   ]

//     const A = [
//         [sumX4, sumX3, sumX2],
//         [sumX3, sumX2, sumX],
//         [sumX2, sumX, n]
//     ];
//     const B = [sumX2Y, sumXY, sumY];
//  // Решаем систему уравнений методом Крамера
//     const detA = determinant(A);
//     if (detA === 0) throw new Error("Система уравнений не имеет решения");

//     const detA1 = determinant(replaceColumn(A, B, 0));
//     const detA2 = determinant(replaceColumn(A, B, 1));
//     const detA3 = determinant(replaceColumn(A, B, 2));

//     const aa = detA1 / detA;
//     const bb = detA2 / detA;
//     const cc = detA3 / detA;
// // console.log(aa,bb,cc);


//     var result = [];
//     const lastIndex = fan.points.length-1
//     const step = (fan.points[lastIndex].flow-fan.points[0].flow)/dots
//     for (let i = 0; i < dots; i++) {
//       let currentPoint = {
//         x: fan.points[0].flow-0+step*i,
//         y: aa*(fan.points[0].flow+step*i)*(fan.points[0].flow+step*i)+bb*(fan.points[0].flow+step*i)+cc
//       }    
//       result.push(currentPoint) 
//     }
//     // console.log(result);
    
//     return result


//     function determinant(matrix) {
//     return matrix[0][0] * (matrix[1][1] * matrix[2][2] - matrix[1][2] * matrix[2][1]) -
//            matrix[0][1] * (matrix[1][0] * matrix[2][2] - matrix[1][2] * matrix[2][0]) +
//            matrix[0][2] * (matrix[1][0] * matrix[2][1] - matrix[1][1] * matrix[2][0]);
// }

// function replaceColumn(matrix, column, index) {
//     const newMatrix = matrix.map(row => [...row]);
//     for (let i = 0; i < newMatrix.length; i++) {
//         newMatrix[i][index] = column[i];
//     }
//     return newMatrix;
// }
//       }

      function checkFan(fan, flow, pressure, typeFan, temperature, serieFan,materialDesign, fluctuationPercentUp, fluctuationPercentDown,density, fifth_type,startType, calcType, motorPowerList, motorSpeedList, motorData) {
        
        if (fan.minFlow > flow || fan.maxFlow < flow) {
          return false
        }
        if (typeFan != fan.type) {
          // console.log(1);
          return false
        }
        if(serieFan != fan.serie&&serieFan!="all"){
          return false
        }
        if(fifth_type==false&&fan["Исполнение 5 радиального вентилятора (ременная передача)"]==1)
        {
          return false
        }
        if(fan[materialDesign]==0)
        {
        return false
        }
        else {

          var currentFanCurve = makeFanCurve(fan, 200, density,calcType )
          for (let index = 0; index < currentFanCurve.length; index++) {
            if (currentFanCurve[index].y > (currentFanCurve[index].x * currentFanCurve[index].x * system)) {
              continue
            } else {            
              var possibleFan = {
                curve: currentFanCurve,
                workPoint: {
                  flow: currentFanCurve[index].x,
                  pressure: currentFanCurve[index].x * currentFanCurve[index].x * system,
                  kpd:currentFanCurve[index].kpd,
                  reqPower: Math.round(currentFanCurve[index].x*currentFanCurve[index].x * currentFanCurve[index].x * system/currentFanCurve[index].kpd/3600)/10
                },
                reqPoint:{
                  flow:flow,
                  pressure:pressure
                },
                points: fan.points,
                name: fan.name,
                speed: fan.speed,
                maxSpeed:fan.speed,
                minSpeed: fan["Мин. частота, об/мин"],
                maxPower:fan["Макс. мощность, кВт"]
              }
              console.log(possibleFan.name);
              
               var vfdRatio = 1
                if (startType == "frequencyConverter") {
                  vfdRatio = possibleFan.reqPoint.flow/possibleFan.workPoint.flow
               
                  for (let i = 0; i < possibleFan.curve.length; i++) {
                    possibleFan.curve[i].x=possibleFan.curve[i].x*vfdRatio
                    possibleFan.curve[i].y= possibleFan.curve[i].y*vfdRatio*vfdRatio
                  }     
                  possibleFan.speed = Math.round(possibleFan.speed*vfdRatio)
                  possibleFan.workPoint.flow = possibleFan.reqPoint.flow
                  possibleFan.workPoint.pressure = possibleFan.reqPoint.pressure   
                  
                  
              ArrayOfFans.push(possibleFan)
                } 
                else if(fan["Исполнение 5 радиального вентилятора (ременная передача)"]!="1") 
                {
                  var possibleSpeeds = []
                  var minSpeed = fan["Мин. частота, об/мин"]*0.85
                  var maxSpeed = fan["Макс. частота, об/мин"]*1.15
                  var possiblePowers = []
                  var minPower = possibleFan.workPoint.flow*possibleFan.workPoint.pressure/36000/90
                  var maxPower = fan["Макс. мощность, кВт"]
                  motorPowerList.forEach(element => {
                    if (element>=minPower&&element<=maxPower) {
                      possiblePowers.push(element)
                    }
                  })
                  
                  motorSpeedList.forEach(element => {
                    if (element>=minSpeed&&element<=maxSpeed) {
                      possibleSpeeds.push(element)
                    }
                  })

                  var speedRatio = 1
                  for (let i = 0; i < possibleSpeeds.length; i++) {
                    var tempPossibleFan={}
                    Object.assign(tempPossibleFan,possibleFan)
                    speedRatio = possibleSpeeds[i]/tempPossibleFan.maxSpeed
                    
                    var tempPoint = {
                      flow:tempPossibleFan.workPoint.flow*speedRatio,
                      pressure:tempPossibleFan.workPoint.pressure*speedRatio*speedRatio,
                      reqPower:tempPossibleFan.workPoint.reqPower*speedRatio*speedRatio*speedRatio
                    }
                    
                      if ((tempPoint.pressure>pressure*(fluctuationPercentUp/100+1))||(tempPoint.pressure<pressure*(1-fluctuationPercentDown/100) )) {
                        console.log("skip");                      
                      continue
                       }
                       else 
                       {

                        tempPossibleFan.reqPower = tempPoint.reqPower
                        tempPossibleFan.speed = Math.round(tempPossibleFan.speed*speedRatio)
                        var filterdPossiblePowers = possiblePowers.filter(element => element>=tempPossibleFan.reqPower)
                        var possiblemotors = motorData.filter(element => element["Мощность, кВт"]==filterdPossiblePowers[0]&&
                        element["Номинальная частота вращения, об/мин"]==tempPossibleFan.speed&&
                        element["Производитель"]=="ВР")
                        // console.log(possiblemotors);
                        var speedRatio2 = possiblemotors[0]["Частота вращения, об/мин"]/tempPossibleFan.speed
                        
                        tempPoint.flow = tempPoint.flow*speedRatio2
                        tempPoint.pressure= tempPoint.pressure*speedRatio2*speedRatio2
                        tempPossibleFan.color = 'blue'
                        if ((tempPoint.pressure>pressure*(fluctuationPercentUp/100+1))||(tempPoint.pressure<pressure*(1-fluctuationPercentDown/100) )) {
                        console.log("skip2");                      
                          tempPossibleFan.color = 'red'
                          var isPush = true
                       }
                        // console.log(speedRatio);
                        for (let i = 0; i < tempPossibleFan.curve.length; i++) {
                          tempPossibleFan.curve[i].x=tempPossibleFan.curve[i].x*speedRatio2*speedRatio
                          tempPossibleFan.curve[i].y= tempPossibleFan.curve[i].y*speedRatio2*speedRatio*speedRatio2*speedRatio
                  }   
                      
                  tempPossibleFan.reqPower = tempPossibleFan.workPoint.reqPower*speedRatio2*speedRatio2*speedRatio2*speedRatio*speedRatio*speedRatio
                  tempPossibleFan.speed = Math.round(tempPossibleFan.speed*speedRatio2)
                  tempPossibleFan.workPoint.flow = tempPoint.flow
                  tempPossibleFan.workPoint.pressure = tempPoint.pressure
                      // console.log(tempPossibleFan.speed);
                       }
                       if (isPush) {
                        ArrayOfFans.push(tempPossibleFan)
                       } else {ArrayOfFans.unshift(tempPossibleFan)}
                       
                  }
                  break
                  // console.log(possibleSpeeds, possiblePowers);
                  
                } else if (fan["Исполнение 5 радиального вентилятора (ременная передача)"]=="1") {

                }
              
              return true
            }

            break
          }
          return false
        }
      }

      
    },
    async loadExcelFile(filePath, targetVariable, isConvertData) {
      try {
        // Загружаем файл с сервера
        const response = await fetch(filePath);
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
          const result = rows.map((row) => {
            const obj = {};
            headers.forEach((header, columnIndex) => {
              // Удаляем null и пустые строки
              if (row[columnIndex] !== null && row[columnIndex] !== "") {
                obj[header] = row[columnIndex];
              }
            });
            return obj;
          });
            // console.log(result);
            
            
          // Сохраняем результат в указанную переменную
          if (isConvertData) {
            this[targetVariable] = this.convertData(result);
          }
          else
          {
            this[targetVariable] = result;
         }
          // console.log(this[targetVariable]);
          
        } else {
          console.error('Файл не содержит данных');
        }
      } catch (error) {
        console.error('Ошибка при загрузке файла:', error);
      }
    },
    convertData(vents) {
    
    var fans = []
    for (const vent in vents) {
      if (Object.prototype.hasOwnProperty.call(vents, vent)) {
        const element = vents[vent];
        // console.log(element);
        var tempObj = element
        tempObj.name = element["Модель колеса"]
        tempObj.speed = element["Макс. частота, об/мин"]
        tempObj.minFlow = element["Мин. расход, м3/ч"]
        tempObj.maxFlow = element["Макс. расход, м3/ч"]
        tempObj.type = element["Тип вентилятора"]
        tempObj.serie = element["Серия"]
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

      
      // console.log(fan);
      
      this.workPoint.flow = Math.floor(fan.workPoint.flow) 
      this.workPoint.pressure = Math.floor(fan.workPoint.pressure)
      this.workPoint.speed = Math.floor(fan.speed)
      this.workPoint.reqPower = Math.floor(fan.workPoint.reqPower*10)/10
      // console.log(fan);
      this.series[0].data[0].x = fan.workPoint.flow
      this.series[0].data[0].y = fan.workPoint.pressure
      this.series[3].data = makeSystemCurve(fan)
      this.series[2].data[0] = {
        x: this.flow,
        y: this.pressure
      }
      var checkCurve = []

      for (let i = 0; i < fan.curve.length; i++) {
        // console.log(fan);
        
        var tempPoint = {
          x: fan.curve[i].x,
          y: fan.curve[i].y
        }
        checkCurve.push(tempPoint)
      }
      
      this.series[1].data = checkCurve
      // this.update
      // Theme(fan)
      // chart.updateOptions({
      //   chart: {
      //     // offsetY: 100,
      //     // offsetX: 100,
      //     // height: 700,
      //     // width:700,
      //     id:'chart',
      //     type: 'line',
      //     zoom:{
      //       enabled:false
      //     },
      //     toolbar:{
      //       show:false
      //     }
      //   },
      // })
      
      function makeSystemCurve(fan) {
        // console.log(fan);
        
        var system = fan.workPoint.pressure / (fan.workPoint.flow * fan.workPoint.flow)
        var systemCurve = []

 

        for (let index = 0; index < fan.curve.length; index++) {
          var systemCurvePoint = {
            x: fan.curve[index].x,
            y: fan.curve[index].x * fan.curve[index].x * system
          }

          if (systemCurvePoint.x > fan.workPoint.flow&&systemCurvePoint.x > fan.reqPoint.flow) {
            systemCurve.push(systemCurvePoint)
            break
          }
          systemCurve.push(systemCurvePoint)
        }

        return systemCurve
      }
    },
    // updateTheme(fan) {
    //   this.chartOptions = {
    //     // chart: {
    //     //   // offsetY: 100,
    //     //   // offsetX: 100,
    //     //   height: 350,
    //     //   width:350,
    //     //   id:'chart',
    //     //   type: 'line',
    //     //   zoom:{
    //     //     enabled:false
    //     //   },
    //     //   toolbar:{
    //     //     show:false
    //     //   }
    //     // },
    //     grid:{
    //       xaxis: {
    //     lines: {
    //         show: true
    //     }
    // },   
    // yaxis: {
    //     lines: {
    //         show: true
    //     }
    // },  
    //     },
    //     // fill: {
    //     //   type: 'solid',
    //     // },
    //     // markers: {
    //     //   size: [2, 2]
    //     // },
    //     tooltip: {
    //       shared: false,
    //       intersect: true,
    //     },
    //     legend: {
    //       show: true
    //     },
    //     xaxis: {
    //       type: 'numeric',
    //       // stepSize: 1000,
    //       // min: fan.points[0].flow * 0.9-(fan.points[0].flow * 0.9)%1000,
    //       // max: fan.points[fan.points.length - 1].flow * 1.1-(fan.points[fan.points.length - 1].flow * 1.1)%1000+1000,
    //       tickAmount: 92.
    //     },
    //     yaxis: {
    //       // max: function (max) { return max-max%1000+1000},
    //       // min: function (min) { return min-min%1000 },
    //       labels: {
    //         show: true,
    //         offsetX: -10,
    //         offsetY: 0,
    //         rotate: 0,
    //         formatter: (val) => { return Math.floor(val) },
    //       },
    //       type: 'numeric',
    //       tickAmount: 10
    //     },
    //     stroke: {
    //       width: [10, 5, 1, 10],
    //       curve: 'monotoneCubic',
    //       dashArray: [0, 0, 0, 0]
    //     }
    //   }
    // }
  }
}

</script>