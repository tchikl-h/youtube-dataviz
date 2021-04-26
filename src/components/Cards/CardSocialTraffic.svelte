<script>
  import { history } from "/home/herve/repo/youtube-dataviz/src/store/history.js";
  import Traffic from "/home/herve/repo/youtube-dataviz/src/model/Traffic.js";

  let youtubers = getBestYoutubers(5);
  let barColors = ["red", "green", "purple", "blue", "orange"];

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

function getYoutubersMap() {
  const allYoutubers = $history.map(h => {
    if (h && h.subtitles && h.subtitles.length > 0) {
      return h.subtitles[0].name
    }
  });

  let map = count(allYoutubers);

  map[Symbol.iterator] = function* () {
    yield* [...this.entries()].sort((a, b) => a[1] - b[1]);
  }
  return map;
}

function getBestYoutubers(nb) {
  let youtubersList = [];
  const map = getYoutubersMap();

  if (map.size >= nb) {
    for(let i = 1; i <= nb; i++) {
      youtubersList.push(
        new Traffic(
          Array.from(map)[map.size - i][0],
          Array.from(map)[map.size - i][1],
          Math.trunc(100 * parseInt(Array.from(map)[map.size - i][1]) / $history.length)
        )
      )
    }
  }
  return youtubersList;
}
</script>

<div
  class="relative flex flex-col min-w-0 break-words bg-white w-full mb-6 shadow-lg rounded"
>
  <div class="rounded-t mb-0 px-4 py-3 border-0">
    <div class="flex flex-wrap items-center">
      <div class="relative w-full px-4 max-w-full flex-grow flex-1">
        <h3 class="font-semibold text-base text-gray-800">
          Traffic
        </h3>
      </div>
      <div class="relative w-full px-4 max-w-full flex-grow flex-1 text-right">
        <button
          class="bg-indigo-500 text-white active:bg-indigo-600 text-xs font-bold uppercase px-3 py-1 rounded outline-none focus:outline-none mr-1 mb-1 ease-linear transition-all duration-150"
          type="button"
        >
          See all
        </button>
      </div>
    </div>
  </div>
  <div class="block w-full overflow-x-auto">
    <!-- Projects table -->
    <table class="items-center w-full bg-transparent border-collapse">
      <thead class="thead-light">
        <tr>
          <th
            class="px-6 bg-gray-100 text-gray-600 align-middle border border-solid border-gray-200 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-no-wrap font-semibold text-left"
          >
            Youtuber
          </th>
          <th
            class="px-6 bg-gray-100 text-gray-600 align-middle border border-solid border-gray-200 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-no-wrap font-semibold text-left"
          >
            Views
          </th>
          <th
            class="px-6 bg-gray-100 text-gray-600 align-middle border border-solid border-gray-200 py-3 text-xs uppercase border-l-0 border-r-0 whitespace-no-wrap font-semibold text-left min-w-140-px"
          ></th>
        </tr>
      </thead>
      {#if youtubers.length > 0}
      <tbody>
        {#each youtubers as youtuber, i}
        <tr>
          <th
            class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-no-wrap p-4 text-left"
          >
            {youtuber.name}
          </th>
          <td
            class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-no-wrap p-4"
          >
            {youtuber.views}
          </td>
          <td
            class="border-t-0 px-6 align-middle border-l-0 border-r-0 text-xs whitespace-no-wrap p-4"
          >
            <div class="flex items-center">
            <span class="mr-2">{youtuber.pourcentage}%</span>
              <div class="relative w-full">
                <div
                  class="overflow-hidden h-2 text-xs flex rounded bg-{barColors[i]}-200"
                >
                  <div
                    style="width: {youtuber.pourcentage}%;"
                    class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-{barColors[i]}-500"
                  ></div>
                </div>
              </div>
            </div>
          </td>
        </tr>
        {/each}
      </tbody>
      {/if}
    </table>
  </div>
</div>
