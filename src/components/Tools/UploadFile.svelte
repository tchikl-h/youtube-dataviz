<script>
    import { history } from "/home/herve/repo/youtube-dataviz/src/store/history.js";
    let files;
    let dataFile = null;
    let isOnlyOneFile = false;
    let isJson = true;
    let isFileUnreadable = false;

    function fileErrors() {
        if (files > 1) {
            isOnlyOneFile = false;
            return true;
        }
        if (files[0].type !== "application/json") {
            isJson = false;
            return true
        }
        return false;
    }

    function upload() {
        if (fileErrors()) {
            return;
        }
        let file = files[0];
        if (file) {
            var reader = new FileReader();
            reader.readAsText(file, "UTF-8");
            reader.onload = function (evt) {
                history.set(JSON.parse(evt.target.result));
            }
            reader.onerror = function (evt) {
                isFileUnreadable = true;
            }
        }
    }
</script>

<style>
    @import url('//maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css');

    .isa_info, .isa_success, .isa_warning, .isa_error {
    margin: 10px 0px;
    padding:12px;
    }
    .isa_info {
        color: #00529B;
        background-color: #BDE5F8;
    }
    .isa_success {
        color: #4F8A10;
        background-color: #DFF2BF;
    }
    .isa_warning {
        color: #9F6000;
        background-color: #FEEFB3;
    }
    .isa_error {
        color: #D8000C;
        background-color: #FFD2D2;
    }
    .isa_info i, .isa_success i, .isa_warning i, .isa_error i {
        margin:10px 22px;
        font-size:2em;
        vertical-align:middle;
    }
    .input-container {
		text-align: center;
        display: flex;
        flex-flow: row;
        justify-content: space-around;
    }
</style>

<div class="input-container">
    <input id="fileUpload" type="file" bind:files>
    
    {#if dataFile && files[0]}
        <p>
            {files[0].name}
        </p>
    {/if}
    
    {#if files}
        <button class="bg-white active:bg-gray-100 text-gray-800 font-normal px-4 py-2 rounded outline-none focus:outline-none mr-2 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150" on:click={upload}>Submit</button>
    {:else}
        <button class="bg-grey active:bg-gray-100 text-gray-800 font-normal px-4 py-2 rounded outline-none focus:outline-none mr-2 mb-1 uppercase shadow hover:shadow-md inline-flex items-center font-bold text-xs ease-linear transition-all duration-150" on:click={upload} disabled>Submit</button>
    {/if}
</div>
{#if isOnlyOneFile}
    <div class="isa_error input-container">
        <i class="fa fa-times-circle"></i>
        You should only choose one file.
    </div>
{/if}
{#if !isJson}
    <div class="isa_error input-container">
        <i class="fa fa-times-circle"></i>
        The file needs to be of type JSON.
    </div>
{/if}
{#if isFileUnreadable}
    <div class="isa_error input-container">
        <i class="fa fa-times-circle"></i>
        The file loaded is unreadable.
    </div>
{/if}