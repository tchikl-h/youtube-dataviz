<script>
  import { history } from "/home/herve/repo/youtube-dataviz/src/store/history.js";
  import Year from "/home/herve/repo/youtube-dataviz/src/model/Year.js";
  import { onMount } from "svelte";
  // library that creates chart objects in page
  import Chart from "chart.js";

  // init chart
  onMount(async () => {
    var config = {
      type: "line",
      data: {
        labels: [
          "January",
          "February",
          "March",
          "April",
          "May",
          "June",
          "July",
          "August",
          "September",
          "October",
          "November",
          "December"
        ],
        datasets: getDataSets()
      },
      options: {
        maintainAspectRatio: false,
        responsive: true,
        title: {
          display: false,
          text: "Sales Charts",
          fontColor: "white",
        },
        legend: {
          labels: {
            fontColor: "white",
          },
          align: "end",
          position: "bottom",
        },
        tooltips: {
          mode: "index",
          intersect: false,
        },
        hover: {
          mode: "nearest",
          intersect: true,
        },
        scales: {
          xAxes: [
            {
              ticks: {
                fontColor: "rgba(255,255,255,.7)",
              },
              display: true,
              scaleLabel: {
                display: false,
                labelString: "Month",
                fontColor: "white",
              },
              gridLines: {
                display: false,
                borderDash: [2],
                borderDashOffset: [2],
                color: "rgba(33, 37, 41, 0.3)",
                zeroLineColor: "rgba(0, 0, 0, 0)",
                zeroLineBorderDash: [2],
                zeroLineBorderDashOffset: [2],
              },
            },
          ],
          yAxes: [
            {
              ticks: {
                fontColor: "rgba(255,255,255,.7)",
              },
              display: true,
              scaleLabel: {
                display: false,
                labelString: "Value",
                fontColor: "white",
              },
              gridLines: {
                borderDash: [3],
                borderDashOffset: [3],
                drawBorder: false,
                color: "rgba(255, 255, 255, 0.15)",
                zeroLineColor: "rgba(33, 37, 41, 0)",
                zeroLineBorderDash: [2],
                zeroLineBorderDashOffset: [2],
              },
            },
          ],
        },
      },
    };
    var ctx = document.getElementById("line-chart").getContext("2d");
    window.myLine = new Chart(ctx, config);
  });

  function count(array_elements) {
    var map = new Map();
    array_elements.sort();

    var current = null;
    var cnt = 0;
    for (var i = 0; i < array_elements.length; i++) {
        if (array_elements[i] != current) {
            if (cnt > 0) {
                map.set(current, cnt);
            }
            current = array_elements[i];
            cnt = 1;
        } else {
            cnt++;
        }
    }
    if (cnt > 0) {
        // console.log(current + ' comes --> ' + cnt + ' times');
    }
    return map;
  }

  function getUnique(items) {
    let uniqueItems = [];
    items.forEach((el) => {
      if (uniqueItems.indexOf(el) == -1) {
        uniqueItems.push(el);
      }
    })
    return uniqueItems;
  }

  function getRandomColor() {
    var letters = '0123456789ABCDEF';
    var color = '#';
    for (var i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }

  function getMonthsData(year, yearsMonthsMap) {
    let months = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];

    months.forEach((m, i) => {
      for (let [key, value] of yearsMonthsMap) {
        if (key.substring(0, 4) == year) {
          if (i + 1 == parseInt(key.substring(5, 7))) {
            months[i] = months[i] + value;
          }
        }
      }
    })
    return months;
  }

  function getDataSets() {
    let months = $history.map(h => {
      if (h && h.time) {
        return h.time.substring(0, 7);
      }
    });
    
    let yearsMonthsMap = count(months);

    let years = [];
    for (let [key, value] of yearsMonthsMap) {
      years.push(key.substring(0, 4));
    }

    years = getUnique(years);

    let datasets = years.map(y => {
      return new Year(
        y,
        getRandomColor(),
        getMonthsData(y, yearsMonthsMap)
      )
    })
    return datasets;
  }
</script>

<div
  class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded bg-gray-800"
>
  <div class="rounded-t mb-0 px-4 py-3 bg-transparent">
    <div class="flex flex-wrap items-center">
      <div class="relative w-full max-w-full flex-grow flex-1">
        <h6 class="uppercase text-gray-200 mb-1 text-xs font-semibold">
          Overview
        </h6>
        <h2 class="text-white text-xl font-semibold">
          Sales value
        </h2>
      </div>
    </div>
  </div>
  <div class="p-4 flex-auto">
    <!-- Chart -->
    <div class="relative h-350-px">
      <canvas id="line-chart"></canvas>
    </div>
  </div>
</div>
