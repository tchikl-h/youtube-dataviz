<script>
  import { link } from "svelte-routing";
  import UploadFile from "/home/herve/repo/youtube-dataviz/src/components/Tools/UploadFile.svelte";

  // core components
  const github = "../assets/img/github.svg";
  const google = "../assets/img/google.svg";
  export let location;
</script>

<div class="container mx-auto px-4 h-full">
  <div class="flex content-center items-center justify-center h-full">
    <div class="w-full lg:w-4/12 px-4">
      <div
        class="relative flex flex-col min-w-0 break-words w-full mb-6 shadow-lg rounded-lg bg-gray-300 border-0"
      >
        <div class="rounded-t mb-0 px-6 py-6">
          <UploadFile/>
        </div>
      </div>
    </div>
  </div>
</div>
