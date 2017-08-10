<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1 v-html="msg"></h1>

        <!-- <input type="text" id="acomp-search-input"> -->
        <input type="search" id="acomp-search-input" class="aa-input-search" placeholder="Search for a course" name="search" autocomplete="on" />
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Test data msg returned &#10004; <br /> Vue is running, &#127852;'
    }
  },
  mounted(){
        
        var self = this; 

        var algoliasearch = require('algoliasearch');
        // -> corporate alg
        var client = algoliasearch("HT7VYJG3KU", "d37bbf3291b226676c9f3f1937e865d3");
        var index = client.initIndex('dev_COURSES');

        // init the algolia autocomplete
        var autocomplete = require('autocomplete.js');
        // console.log(autocomplete);


        //initialize autocomplete on search input (ID selector must match)
        autocomplete('#acomp-search-input',
        { hint: false, /* testing */debug: true, openOnFocus: true /* .testing */ }, {
            source: autocomplete.sources.hits(index, {hitsPerPage: 100}),
            //value to be displayed in input control after user's suggestion selection
            displayKey: 'courseTitle',
            //hash of templates used when rendering dataset
            templates: {
                empty: 'Nowt found duck',
                footer: '<p><!-- #keelebecause 8-) --> <hr /></p>',
                //'suggestion' templating function used to render a single suggestion
                suggestion: function(suggestion) {
                  var kAutoCourseResult = '<a href="https://www.keele.ac.uk' + suggestion.urlPath + '"><span class="course_title">' +
                    suggestion._highlightResult.courseTitle.value + '</span><span class="levelName"> | ' +
                    // suggestion._highlightResult.courseLevel.value + '</span>';
                    suggestion.courseLevelName + '</span></a>';
                    return kAutoCourseResult;

                }
            }
        }).on('autocomplete:selected', function(event, suggestion, dataset) {
            location.href = 'https://www.keele.ac.uk/' + suggestion.urlPath;
        });




  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

a {
  color: #42b983;
}


/* DEFAULT ALGOLIA TAKEN FROM https://github.com/algolia/autocomplete.js#installation */
.levelName {
  text-transform: capitalize;
}

#algolia-autocomplete-listbox-0 {
/*  max-height: 210px;*/ /* note that keyboard arrow down doesn't select items */
    // overflow: scroll;
    // max-height: 160px;
    // overflow-y: auto;
}

.aa-input-container {
  display: inline-block;
  position: relative; }
.aa-input-search {
  width: 300px;
  border: 1px solid rgba(228, 228, 228, 0.6);
  padding: 12px 28px 12px 12px;
  box-sizing: border-box;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none; }
  .aa-input-search::-webkit-search-decoration, .aa-input-search::-webkit-search-cancel-button, .aa-input-search::-webkit-search-results-button, .aa-input-search::-webkit-search-results-decoration {
    display: none; }
.aa-input-icon {
  height: 16px;
  width: 16px;
  position: absolute;
  top: 50%;
  right: 16px;
  -webkit-transform: translateY(-50%);
          transform: translateY(-50%);
  fill: #e4e4e4; }
.aa-dropdown-menu {
  background-color: #fff;
  border: 1px solid rgba(228, 228, 228, 0.6);
  min-width: 300px;
  margin-top: 10px;
  box-sizing: border-box; }
.aa-suggestion {
  padding: 12px;
  cursor: pointer;
}
.aa-suggestion + .aa-suggestion {
    border-top: 1px solid rgba(228, 228, 228, 0.6);
}
  .aa-suggestion:hover, .aa-suggestion.aa-cursor {
    background-color: rgba(241, 241, 241, 0.35); }
/* end */


</style>
