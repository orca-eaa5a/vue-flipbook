<template lang="en">
    <div id="flipbook-container">
        
    </div>
</template>
<script>
// import {pdfjsLib, testFunc} from "pdfjs-dist/webpack";
window.$ = window.jQuery = require('jquery');
require("@/assets/js/turn.min");
let pdfjsLib = require("pdfjs-dist/webpack");
pdfjsLib.GlobalWorkerOptions.workerSrc ="https://cdn.jsdelivr.net/npm/pdfjs-dist@2.0.943/build/pdf.worker.min.js";
export default {
    methods:{
        name: 'FlipBook',
        async setcanvas(page, pdf, canvasId){
            let pageObj = await pdf.getPage(page);
            let viewport = pageObj.getViewport({scale: this.scale});
            let canvas = document.querySelector("#"+canvasId);
            try{
                let context = canvas.getContext('2d');
                canvas.height = viewport.height;
                canvas.width = viewport.width;

                // Render PDF page into canvas context
                let renderContext = {
                    canvasContext: context,
                    viewport: viewport
                };
                pageObj.render(renderContext);
            }catch(err){
                console.log(err);
                throw new Error(err);
            }
        },
        async loadPdf(){
            let load = pdfjsLib.getDocument(this.url);
            let pdfObj = await load.promise;
            for(let i = 1; i <= pdfObj.numPages; i++){
                let id = this.canvasIdPrefix + i;
                let div = document.createElement('div');
                div.innerHTML = '<canvas id="' + id + '"></canvas>';
                this.flipbook.append(div);
            }
            for(let i = 1; i <= pdfObj.numPages; i++){
                let id = this.canvasIdPrefix + i;
                await this.setcanvas(i, pdfObj, id);
            }
        },
        async loadApp(){
            let turnObj = $(this.flipbook).turn({
                autoCenter: this.autoCenter
            });
            turnObj;
        }
    },
    props:{
        scale:{
            type: Number,
            default: 3
        },
        autoCenter: {
            type: Boolean,
            default: true
        },
        url:{
            type: String,
            required: true
        },
        height:{
            type: String,
            default: "776px"
        },
        width:{
            type: String,
            default:"1200px"
        }
    },
    data(){
        return {
            flipbook: undefined,
            canvasIdPrefix: "canvaspage"
        }
    },
    async created() {
    },
    async mounted(){
        this.flipbook = document.querySelector("#flipbook-container");
        this.flipbook.style.height = this.height;
        this.flipbook.style.width = this.width;
        await this.loadPdf();
        await this.loadApp();
    }
}
</script>
<style lang="css">
#flipbook-container{
    width: 1200px;
    height: 776px;
    margin: auto;
    background-color:white;
}
#flipbook-container .page{
    width:100%;
    height:auto;
    background-color:white;
    font-size:20px;
    text-align:center;
}

#flipbook-container .hard{
    background:#ccc !important;
    color:#333;
    font-weight:bold;
}

#flipbook-container .odd{
    background:-webkit-gradient(linear, right top, left top, color-stop(0.95, #FFF), color-stop(1, #DADADA));
    box-shadow:inset 0 0 5px #666;
    
}

#flipbook-container .even{
    background:-webkit-gradient(linear, left top, right top, color-stop(0.95, #fff), color-stop(1, #dadada));
    box-shadow:inset 0 0 5px #666;
}

canvas{
    /*max-width: 100%;*/
    width:  100%;
    height: 100%;
}
</style>