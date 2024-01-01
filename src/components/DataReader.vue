<template>
  <div class="row m-2" :class="`row-cols-${cols}`">
    <div class="accordion col" v-for="(jData, i) in chunkedData"
         :key="`row_${i}`">
      <div class="accordion-item mb-2 rounded darkable" v-for="(item, index) in jData" :key="`col_${i}_${index}`">
        <h2 class="accordion-header" :id="panelHead(i, index)">
          <button class="accordion-button fw-bold rounded bg-info" type="button" data-bs-toggle="collapse"
                  :data-bs-target="'#' + contentId(i, index)" aria-expanded="true"
                  :aria-controls="contentId(i, index)">
            {{ item.title }}
            <font-awesome-icon v-if="item.icon" style="margin-left: 5px;" :icon="item.icon"/>
          </button>

        </h2>
        <div :id="contentId(i, index)" class="accordion-collapse collapse show"
             :aria-labelledby="panelHead(i, index)">
          <div class="accordion-body">
            <ul class="list-group">
              <li class="list-group-item text-truncate darkable" :title="record.title"
                  v-for="(record, key) in item.urls" :key="`record_${i}_${index}_${key}`">
                <a :href="record.url" v-html="highlight(record.title)"
                   :class="isDark ? 'text-light' : 'text-primary'"></a>
                <a title="Open in new tab" class="fa-pull-right" :class="isDark ? 'text-light' : 'text-primary'"
                   :href="record.url" target="_blank">
                  <font-awesome-icon icon="fa-solid fa-up-right-from-square"/>
                </a>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  props: {
    cols: {
      type: Number,
      required: false,
      default: 3
    },
    jsonData: {
      type: Array,
      required: true
    },
    searchQuery: {
      type: String,
      required: false,
      default: ''
    },
    isDark: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  methods: {
    panelHead(index1, index2) {
      return `panelHeading_${index1}_${index2}`;
    },
    contentId(index1, index2) {
      return `content_${index1}_${index2}`;
    },
    highlight(value) {
      if (!this.searchQuery) {
        return value;
      } else {
        let regEx = new RegExp(this.searchQuery, "ig");
        return value.replace(regEx, '<mark>' + this.searchQuery + '</mark>');
      }
    },
  },
  computed: {
    chunkedData() {
      const arr = this.jsonData;
      const chunkSize = Math.round(arr.length / this.cols);
      const chunkData = [];
      let counter = 0;
      let breakMainLoop = false;
      for (let i = 0; i < this.cols; i++) {
        if (breakMainLoop) {
          break;
        }
        chunkData[i] = [];
        for (let j = 0; j < chunkSize; j++) {
          chunkData[i].push(arr[counter++]);
          if (counter === arr.length) {
            breakMainLoop = true;
            break;
          }
        }
      }
      const chunkDiff = arr.length - counter;
      if (chunkDiff > 0) {
        for (let i = 0; i < chunkDiff; i++) {
          chunkData[i].push(arr[counter++]);
          if (counter === arr.length) {
            break;
          }
        }
      }

      return chunkData;
    }
  }
};
</script>
