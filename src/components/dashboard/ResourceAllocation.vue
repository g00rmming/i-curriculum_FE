<template>
  <div>
  <div class="row row-cards mt-0">
    <div class="col-lg-4 col-md-12">
      <div class="border card">
        <div class="card-header">
          <h3 class="card-title">전공</h3>
          <div class="card-actions btn-actions">
            <span class="recommend-text">추천과목</span>
            <a class="btn-action" @click="openMajorModal">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="icon icon-tabler icons-tabler-outline icon-tabler-book">
                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                <path d="M3 19a9 9 0 0 1 9 0a9 9 0 0 1 9 0" />
                <path d="M3 6a9 9 0 0 1 9 0a9 9 0 0 1 9 0" />
                <path d="M3 6l0 13" />
                <path d="M12 6l0 13" />
                <path d="M21 6l0 13" />
              </svg>
            </a>
          </div>
        </div>

        <apexchart type="donut" height="350" :options="majorChartOptions" :series="majorSeries"></apexchart>
      </div>
    </div>
    <div class="col-lg-4 col-md-12">
      <div class="border card">
        <div class="card-header">
          <h3 class="card-title">교양</h3>
          <div class="card-actions btn-actions">
            <span class="recommend-text">추천과목</span>
            <a class="btn-action" @click="openGeneralModal">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="icon icon-tabler icons-tabler-outline icon-tabler-book">
                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                <path d="M3 19a9 9 0 0 1 9 0a9 9 0 0 1 9 0" />
                <path d="M3 6a9 9 0 0 1 9 0a9 9 0 0 1 9 0" />
                <path d="M3 6l0 13" />
                <path d="M12 6l0 13" />
                <path d="M21 6l0 13" />
              </svg>
            </a>
          </div>
        </div>
        <apexchart type="donut" height="350" :options="generalChartOptions" :series="generalSeries"></apexchart>
      </div>
    </div>
    <div class="col-lg-4 col-md-12">
      <div class="border card">
        <div class="card-header">
          <h3 class="card-title">핵심 교양</h3>
          <div class="card-actions btn-actions">
            <span class="recommend-text">추천과목</span>
            <a class="btn-action" @click="openGeneralCoreModal"><!-- Download SVG icon from http://tabler-icons.io/i/refresh -->
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="icon icon-tabler icons-tabler-outline icon-tabler-book">
                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                <path d="M3 19a9 9 0 0 1 9 0a9 9 0 0 1 9 0" />
                <path d="M3 6a9 9 0 0 1 9 0a9 9 0 0 1 9 0" />
                <path d="M3 6l0 13" />
                <path d="M12 6l0 13" />
                <path d="M21 6l0 13" />
              </svg>
            </a>
          </div>
        </div>
        <apexchart type="donut" height="350" :options="generalCoreChartOptions" :series="generalCoreSeries"></apexchart>
      </div>
    </div>
  </div>
  
  <RecommendComp :isVisible="modalMajorVisible" @close="modalMajorVisible = false" :RecommendDataList="majorList"> </RecommendComp>
  <RecommendComp :isVisible="modalGeneralVisible" @close="modalGeneralVisible = false" :RecommendDataList="generalList"> </RecommendComp>
  <RecommendComp :isVisible="modalGeneralCoreVisible" @close="modalGeneralCoreVisible = false" :RecommendDataList="generalCoreList"> </RecommendComp>
</div>
</template>

<script>
import RecommendComp from "@/components/dashboard/RecommendComp.vue";
import VueApexCharts from "vue3-apexcharts";

export default {
  components: {
    apexchart: VueApexCharts,
    RecommendComp: RecommendComp,
  },
  props: {
    majorList: [],  // 전공
    generalList: [], // 교양 
    generalCoreList: [], // 핵교
    totalTakenCredit: Number, // 전체이수학점
    generalCoreTakenCredit:Number, //핵심교양 이수학점
    generalEssentialTakenCredit: Number, // 교양 필수 이수학점
    majorTakenCredit: Number, // 전공 이수 학점 
    majorEssentialTakenCredit: Number, // 전공 필수 이수 학점
    one: Number,
    two: Number,
    three: Number,
    four: Number,
    five: Number,
    six: Number,
    creative: Number,
    standardCredit: Number,
  },
  data() {
    return {
      modalMajorVisible: false, // 전공 모달
      modalGeneralVisible: false, // 교양 모달
      modalGeneralCoreVisible: false,//핵교 모달
 

      // 차트 데이터
      majorSeries: [], // 전공
      generalSeries: [], //교양
      generalCoreSeries: [], //핵교 


      //차트 옵션
      majorChartOptions: {
        chart: {
          type: 'donut',
        },
        dataLabels: {
          enabled: true,
          formatter: function (val) {
            return val.toFixed(2) + "%"
          },
          style: {
            fontSize: '10px',
            fontFamily: 'Helvetica, Arial, sans-serif',
            fontWeight: 'bold',
            colors: ["#FFFFFF"]
          },

        },
        responsive: [{
          breakpoint: 480,
          options: {
            chart: {
              width: 200
            },
            legend: {
              position: 'bottom'
            }
          }
        }],
        labels: ['전공필수', '전공선택', '미이수'],
        colors: ["#008FFB","#00E396", "#F05650"],
        plotOptions: {
          pie: {
            donut: {
              size: '60%',
              labels: {
                show: true,
                name: {
                  show: true,
                },
                value: {
                  show: true,
                },
                total: {
                  show: true,
                  showAlways: false,
                  label: '전체학점',
                  fontWeight: 600,
                  color: '#373d3f',
                  formatter: function (w) {
                    return w.globals.seriesTotals.reduce((a, b) => {
                      return a + b
                    }, 0)
                  }
                }
              }
            }
          }
        }
      },
      generalChartOptions: {
        chart: {
          type: 'donut',
        },
        dataLabels: {
          enabled: true,
          formatter: function (val) {
            return val.toFixed(2) + "%"
          },
          style: {
            fontSize: '10px',
            fontFamily: 'Helvetica, Arial, sans-serif',
            fontWeight: 'bold',
            colors: ["#FFFFFF"]
          },

        },
        responsive: [{
          breakpoint: 480,
          options: {
            chart: {
              width: 200
            },
            legend: {
              position: 'bottom'
            }
          }
        }],
        labels: ['교양필수', '교양선택','핵심교양','미이수'],
        colors: ["#008FFB","#00E396",'#FEB019',"#F05650"],
        plotOptions: {
          pie: {
            donut: {
              size: '60%',
              labels: {
                show: true,
                name: {
                  show: true,
                },
                value: {
                  show: true,
                },
                total: {
                  show: true,
                  showAlways: false,
                  label: '전체학점',
                  fontWeight: 600,
                  color: '#373d3f',
                  formatter: function (w) {
                    return w.globals.seriesTotals.reduce((a, b) => {
                      return a + b
                    }, 0)
                  }
                }
              }
            }
          }
        }
      },
      generalCoreChartOptions: {
        chart: {
          type: 'donut',
        },
        dataLabels: {
          enabled: true,
          formatter: function (val) {
            return val.toFixed(2) + "%"
          },
          style: {
            fontSize: '10px',
            fontFamily: 'Helvetica, Arial, sans-serif',
            fontWeight: 'bold',
            colors: ["#FFFFFF"]
          },

        },
        responsive: [{
          breakpoint: 480,
          options: {
            chart: {
              width: 200
            },
            legend: {
              position: 'bottom'
            }
          }
        }],
        labels: ['1영역', '2영역', '3영역','4영역','5영역','6영역','미이수'],
        colors: ["#008FFB","#00E396","#FF5733","#FFC300","#900C3F","#DAF7A6","#6F42C1"],
        plotOptions: {
          pie: {
            donut: {
              size: '60%',
              labels: {
                show: true,
                name: {
                  show: true,
                },
                value: {
                  show: true,
                },
                total: {
                  show: true,
                  showAlways: false,
                  label: '핵심교양',
                  fontWeight: 600,
                  color: '#373d3f',
                  formatter: function (w) {
                    return w.globals.seriesTotals.reduce((a, b) => {
                      return a + b
                    }, 0)
                  }
                }
              }
            }
          }
        }
      },
    };
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    fetchData(){
      console.log("전공 필수 이수 학점: ",this.majorEssentialTakenCredit);
      this.majorSeries = [this.majorEssentialTakenCredit, this.majorTakenCredit - this.majorEssentialTakenCredit, 65 - this.majorTakenCredit];
      this.generalSeries = [this.generalEssentialTakenCredit,this.totalTakenCredit - this.majorTakenCredit - this.generalCoreTakenCredit,this.generalCoreTakenCredit,65-this.generalEssentialTakenCredit-this.totalTakenCredit+this.majorTakenCredit];
     
      if (this.standardCredit === 9) {
        const totalCoreBefore = this.one + this.two + this.three + this.four + this.five + this.six;
        this.generalCoreSeries = [this.one, this.two, this.three, this.four, this.five, this.six, 9 - totalCoreBefore];
        this.generalCoreChartOptions.labels = ['1영역', '2영역', '3영역', '4영역', '5영역', '6영역', '미이수'];
        this.generalCoreChartOptions.colors = ["#008FFB", "#00E396", "#FF5733", "#FFC300", "#900C3F", "#DAF7A6", "#F05650"];
      } else if (this.standardCredit === 12) {
        const totalCoreAfter = this.one + this.two + this.three + this.four + this.five + this.six + this.creative;
        this.generalCoreSeries = [this.one, this.two, this.three, this.four, this.five, this.six, this.creative, 12 - totalCoreAfter];
        this.generalCoreChartOptions.labels = ['1영역', '2영역', '3영역', '4영역', '5영역', '6영역', '창의', '미이수'];
        this.generalCoreChartOptions.colors = ["#008FFB", "#00E396", "#FF5733", "#FFC300", "#900C3F", "#DAF7A6", "#6F42C1", "#F05650"];
      }
       // '전공 이수 학점'이 65 이상인지 확인하여 시리즈를 설정
      if (this.majorTakenCredit >= 65) {
      this.majorSeries = [65];
      this.majorChartOptions.labels = ['완료'];
      this.majorChartOptions.colors = ['#00E396']; // 완료 색상
      this.majorChartOptions.plotOptions.pie.donut.labels.total.label = '전공 요구 사항 충족';
      
    } 
      // '교양 이수 학점'이 65 이상인지 확인하여 시리즈를 설정
      if (this.totalTakenCredit - this.majorTakenCredit + this.generalCoreTakenCredit>= 65) {
      this.generalSeries = [65];
      this.generalChartOptions.labels = ['완료'];
      this.generalChartOptions.colors = ['#00E396']; // 완료 색상
      this.generalChartOptions.plotOptions.pie.donut.labels.total.label = '교양 요구 사항 충족';
      
    } 
     // '핵심교양 이수 학점'이 12 혹은 9 이상인지 확인하여 시리즈를 설정
     if (this.standardCredit === 12 && this.generalCoreTakenCredit === 12 || this.standardCredit === 9 && this.generalCoreTakenCredit === 9) {
      this.generalCoreSeries = [this.standardCredit];
      this.generalCoreChartOptions.labels = ['완료'];
      this.generalCoreChartOptions.colors = ['#00E396']; // 완료 색상
      this.generalCoreChartOptions.plotOptions.pie.donut.labels.total.label = '핵심교양 요구 사항 충족';
      
    } 

    if (this.generalCoreTakenCredit === 0) {
      this.generalCoreSeries = [this.standardCredit];
      this.generalCoreChartOptions.labels = ['미이수'];
      this.generalCoreChartOptions.colors = ['#F05650']; // 미이수 색상
      
    } 
      
    },
    openMajorModal() {
      if(this.majorTakenCredit >= 65){
        this.$swal("이미 전공 졸업 요건을 충족 했습니다.", '', "success");
      
      }else{
        this.modalMajorVisible = true;
      }
    },
    openGeneralModal() {
      if(this.totalTakenCredit - this.majorTakenCredit + this.generalCoreTakenCredit>=65){
     
        this.$swal("이미 교양 졸업 요건을 충족 했습니다.", '', "success");
      
      }else{
        this.modalGeneralVisible = true;
      }

    },
    openGeneralCoreModal(){
      if(this.standardCredit ===9){
        if(this.generalCoreTakenCredit === 9){
          this.$swal("이미 핵심교양 졸업 요건을 충족 했습니다.", '', "success");
        }else{
          this.modalGeneralCoreVisible = true;
        }
      }else if(this.standardCredit === 12){
        if(this.generalCoreTakenCredit === 12){
          this.$swal("이미 핵심교양 졸업 요건을 충족 했습니다.", '', "success");
        }else{
          this.modalGeneralCoreVisible = true;
        }
      }
    },
  }
};
</script>

<style>
.card {
  margin: 1rem;
}

.card-body {
  padding: 1rem;
}

.recommend-text {

  font-size: 16px;

}
</style>