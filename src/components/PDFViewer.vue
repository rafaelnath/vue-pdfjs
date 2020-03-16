<template>
  <div class="pdf-viewer">
      <header>
          <PDFZoom
            :scale="scale"
            @change="updateScale"
            @fit="updateFit"
          />
          <PDFPaginator
            v-model="currentPage"
            :pageCount="pageCount"
          />
      </header>

      <PDFData
        :url="url"
        @page-count="updatePageCount"
        @page-focus="updateCurrentPage"
        >
        <template v-slot:document="{pages}">
          <PDFDocument
            v-bind="{pages, scale, optimalScale, fit, currentPage, pageCount}"
            @scale-change="updateScale"
          />
        </template>
      </PDFData>
  </div>
</template>

<script>
import PDFZoom from './PDFZoom';
import PDFPaginator from './PDFPaginator';

import PDFData from './PDFData';
import PDFDocument from './PDFDocument';


function floor(value, precision){
    const multiplier = Math.pow(10, precision || 0);
    return Math.floor(value * multiplier) / multiplier;
}

export default {
    name: 'PDFViewer',

    components: {
        PDFZoom,
        PDFPaginator,
        PDFData,
        PDFDocument
    },

    data(){
        return{
            url: 'https://cdn.filestackcontent.com/wcrjf9qPTCKXV3hMXDwK',
            scale: undefined,
            optimalScale:undefined,
            fit: undefined,
            currentPage: 1,
            pageCount: undefined, //total page
        };
    },

    methods:{
        updateScale({scale, isOptimal = false}){
            const roundedScale = floor(scale, 2);
            if (isOptimal) this.optimalScale = roundedScale;
            this.scale = roundedScale;
        },

        updateFit(fit){
            this.fit = fit;
        },

        updatePageCount(pageCount){
            this.pageCount = pageCount;
        },

        updateCurrentPage(pageNumber){
            this.currentPage = pageNumber;
        },
    },

    watch:{
        url() { //when url empty
            this.currentPage = undefined;
        },
    },

    mounted(){
        document.body.classList.add('overflow-hidden');
    }
    
}
</script>

<style>

</style>