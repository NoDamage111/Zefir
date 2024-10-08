<template> 
  <div class="weather">

    <div>
      <p>Zefir</p>
    </div>
    <div class="container">

      <div class="card weather-form .first" >
        <label for="flow">Производительность, м3/ч?</label>
        <input type="number" name="flow" class="weather-form_input" @keyup.enter="sendRequest" v-model="flow" >
        <label for="pressure">Давление, Па</label>
        <input type="number" name="pressure" class="weather-form_input" @keyup.enter="sendRequest" v-model="pressure">
        <label for="temperature">Температура, °С</label>
        <input type="number" name="temperature" class="weather-form_input" @keyup.enter="sendRequest" v-model="temperature" >
        <button class="weather-form_btn" @click="sendRequest" >Подбор</button>
      </div>

      <div class="card weather-form .second">
        <button class="weather-form_btn" v-for="fan in ArrayOfFans" @click="showGraph">{{ fan.name }}</button>
      </div>

      <div class="card weather-form .third">
        <p v-if="this.workPoint.flow">
          Здесь будет график, но пока только рабочая точка:
        </p>
        <p v-if="this.workPoint.flow">
          Производительность: {{this.workPoint.flow}} м3/ч
        </p>
        <p v-if="this.workPoint.flow">
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

export default{
  data(){
    return{
      flow: 30000,
      pressure:2000,
      temperature:0,
      fansArray:[
        {
          name:"ВР 280-46 №5",
          speed: 1450,
          points:[
            {
              flow:7571,
              pressure: 1967,
              kpd:60
            },
            {
              flow:10582,
              pressure: 2329,
              kpd:70
            },
            {
              flow:14445,
              pressure: 2487,
              kpd:73
            },
            {
              flow:19169,
              pressure: 2594,
              kpd:70
            },
            {
              flow:24265,
              pressure: 2475,
              kpd:60
            },
          ],
          minFlow:7571,
          maxFlow:24265
        },
        {
          name:"ВР 280-46 №8",
          speed: 960,
          points:[
            {
              flow:20327,
              pressure: 2203,
              kpd:60
            },
            {
              flow:28807,
              pressure: 2588,
              kpd:70
            },
            {
              flow:39311,
              pressure: 2821,
              kpd:73
            },
            {
              flow:51791,
              pressure: 2914,
              kpd:70
            },
            {
              flow:66056,
              pressure: 2783,
              kpd:60
            },
          ],
          minFlow:20327,
          maxFlow:66056
        },
        {
          name:"ВР 280-46 №2",
          speed: 2900,
          points:[
            {
              flow:1124,
              pressure: 1064,
              kpd:53
            },
            {
              flow:1474,
              pressure: 1181,
              kpd:57
            },
            {
              flow:1894,
              pressure: 1284,
              kpd:59
            },
            {
              flow:2262,
              pressure: 1284,
              kpd:57
            },
            {
              flow:2578,
              pressure: 1206,
              kpd:53
            },
          ],
          minFlow:1124,
          maxFlow:2578
        }        
      ],
      ArrayOfFans:[],
      systemCurve:[],
      fanCurve:[],
      requriedPoint:{
        flow: this.flow,
        pressure: this.pressure
      },
      workPoint:{
        flow:0,
        pressure:0
      }
    };
  },
  methods:{
    sendRequest(){
      var flow = this.flow;
      var pressure = this.pressure;
      var system = pressure/(flow*flow)

      var systemCurve =[];
     
      makeSystemCurve(flow)
      


      var ArrayOfFans=[]

      for (let index = 0; index < this.fansArray.length; index++) {
        if (checkFan(this.fansArray[index],flow, makeFanCurve(this.fansArray[index],1000), this.workPoint)) {
          ArrayOfFans.push(this.fansArray[index])
          this.workPoint = checkFan(this.fansArray[index],flow, makeFanCurve(this.fansArray[index],1000), this.workPoint)
        }  
      }
      
      var fan1 = makeFanCurve(ArrayOfFans[0],1000)

      this.ArrayOfFans = ArrayOfFans;


      console.log(fan1); 
      console.log(this.workPoint); 
      function makeSystemCurve(flow){
        var numberOfPoints = 10
        var step = flow/numberOfPoints
        
        for (let index = 0; index <= numberOfPoints; index++) {
          var currentFlow = index*step
          var currentPressure = currentFlow*currentFlow*system
          systemCurve.push({currentFlow,currentPressure})
        }
      }

      function makeFanCurve(fan,dots){
        var result=[];
        let step = (fan.maxFlow - fan.minFlow) / dots //шаг по Х

        for (let i = 0; i <= dots; i++) {
          let factor = 0
          for (let k = 0; k < fan.points.length; k++) {
            let li = 1
            for (let j = 0; j < fan.points.length; j++) {
              if (k === j) continue
              else {
              li = li * (((fan.minFlow + i * step) - fan.points[j].flow) / (fan.points[k].flow - fan.points[j].flow))
              }
            }
      factor = factor + fan.points[k].pressure * li
      
    }
    var currentPoint={
      flow: fan.minFlow + i * step,
      pressure: factor
    }
    
    result.push(currentPoint) //Массив значений Y
  }

  return result
}

      function checkFan(fan, flow, fan1, workPoint){
        if (fan.minFlow>flow || fan.maxFlow<flow) {
          return false
        } else {
          for (let index = 0; index < fan1.length; index++) {
            if (fan1[index].pressure>(fan1[index].flow*fan1[index].flow*system)) {
              continue
            } else {
              workPoint.flow = fan1[index].flow
              workPoint.pressure = fan1[index].flow*fan1[index].flow*system
                           
              return workPoint
            }          
          }
          return false
        }
      }
    },
    showGraph(){

    }
  }
}


</script>