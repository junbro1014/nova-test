<template>
  <Card class="flex flex-col items-center justify-center">
    <div class="px-8 py-3 w-full">
        <div class="py-8">
            <canvas ref="chartRef"></canvas>
        </div>
    </div>
  </Card>
</template>

<script setup>
import {onMounted, ref} from "vue";
import Chart from 'chart.js/auto';
let chartData = {
    labels: ['a','b'],
    datasets: [
        {
            barPercentage: 1,
            categoryPercentage: 0.4,
            label: 'Andon(Min)',
            yAxisID: 'data_1',
            data: [2,8],
            backgroundColor: ['#25c161'],
            datalabels: {
                display: false,
            }
        },
        {
            barPercentage: 1,
            categoryPercentage: 0.4,
            label: 'Andon(Count)',
            yAxisID: 'data_2',
            data: [6,5],
            backgroundColor: ['#90a3af'],
            datalabels: {
                display: false,
            }
        },
    ]
};
const chartOptions = {
    layout: {
        padding: {
            top: 25,
            bottom: 12,
        },
    },
    interaction: {
        intersect: false,
        mode: 'index',
    },
    plugins: {
        legend: {
            display: true,
            position: 'top',
            align: 'start',
            labels: {
                usePointStyle: true,
                boxWidth: 8,
                boxHeight: 8,
                font: {
                    size: 13,
                    weight: 500,
                },
                color: '#4a5d6e', // 글자 색상을 지정합니다.
            },
        },
        tooltip: {
            enabled: true,
            backgroundColor: '#4a5d6e',
            font: {
                size: 13,
            },
            callbacks: {
                // 레이블 색상 변경
                labelColor: function (tooltipItem, chart) {
                    if(tooltipItem.datasetIndex === 0) {
                        return {
                            borderWidth: 0,
                            borderRadius: 5,
                            backgroundColor: '#25c161'
                        }
                    } else if(tooltipItem.datasetIndex === 1) {
                        return {
                            borderWidth: 0,
                            borderRadius: 5,
                            backgroundColor: '#90a3af'
                        }
                    }
                }
            },
        },

    },
    scales: {
        x: {
            grid: {
                borderColor: '#D7DBE2',
                display: false,
            },
            border: {
                display: false,
            },
            ticks: {
                color: '#354655', // x축 눈금의 색
                font: {
                    size: 12,
                },
            },
        },
        data_1: {
            grid: {
                color: '#D7DBE2',
                display: true, // 선이 아예 안 그려지게 됩니다.
                drawTicks: true, // 눈금 표시 여부를 지정합니다.
            },
            border: {
                display: false,
            },
            position: 'left',
            // y축 위쪽 여유공간
            afterDataLimits: (scale) => {
                scale.max = scale.max * 1.1;
            },
            ticks: {
                maxTicksLimit: 6,
                weight: 500,
                color: '#2d3e4d',
            },
            title: {
                display: false,
            },
        },
        data_2: {
            grid: {
                color: '#D7DBE2',
                drawOnChartArea: false,
                drawTicks: true,
            },
            position: 'right',
            afterDataLimits: (scale) => {
                scale.max = scale.max * 1.1;
            },
            border: {
                display: false,
            },
            title: {
                display: false,
            },
            ticks: {
                maxTicksLimit: 6,
                weight: 500,
                color: '#2d3e4d',
            },
        },
    },
    maxBarThickness: 26,
    borderRadius: 6,
    responsive: true,
    maintainAspectRatio: false
};

const chartRef = ref(null);
let andonChart;

onMounted(() => {
    const andonCtx = chartRef.value.getContext('2d');
    andonChart = new Chart(andonCtx, {
        type: 'bar',
        data: chartData,
        options: chartOptions,
        // plugins: chart_plugin,
    })
})
</script>
