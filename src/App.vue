<template>
  <ejs-treegrid :dataSource="data" childMapping="subtasks" 
    :treeColumnIndex="1" :height='450'>
    <e-columns>
        <e-column field='ID' headerText='S.No' width='90' textAlign='Right'></e-column>
        <e-column field='Name' headerText='ShipMent Name' width='220'></e-column>
        <e-column field='category' headerText='Category' width='220'></e-column>
        <e-column field='unitPrice' headerText='Unit Price($)' format='c2' width='120' textAlign='Right'></e-column>
        <e-column field='price' headerText='Price($)' width='100' format='c' textAlign='Right'></e-column>
    </e-columns>
    <e-aggregates>
      <e-aggregate>
        <e-aggregate-columns :showChildSummary="false">
          <e-aggregate-column field="price" type="Sum" :footerTemplate="footerSum"></e-aggregate-column>
          <e-aggregate-column field="unitPrice" type="Max" :footerTemplate="footerMax"></e-aggregate-column>
        </e-aggregate-columns>
      </e-aggregate>
      <e-aggregate>
        <e-aggregate-columns :showChildSummary="true">
          <e-aggregate-column field="category" type="Custom" :footerTemplate="customFooter" :customAggregate="customAggregateFn"></e-aggregate-column>
        </e-aggregate-columns>
      </e-aggregate>
    </e-aggregates>
  </ejs-treegrid>
</template>
<script>
import { TreeGridComponent, ColumnsDirective, ColumnDirective, Aggregate, AggregatesDirective, 
  AggregateDirective, AggregateColumnsDirective, AggregateColumnDirective } from "@syncfusion/ej2-vue-treegrid";
import {summaryData} from './data.js';
import {createApp} from 'vue/dist/vue.esm-bundler';
import {getObject} from '@syncfusion/ej2-vue-grids';
const app = createApp();
var footerTemp = app.component("footerSum",{
  template: "<span>Sum: {{data.Sum}}</span>",
  data: () => ({})
})
var footerTemp1 = app.component("footerMax",{
  template: "<span>Maximum: {{data.Max}}</span>",
  data: () => ({})
})
var customAggregate = app.component("customFooter",{
  template: "<b>Count of Frozen Seafood: {{data.Custom}}</b>",
  data: () => ({})
})
export default{
  name: 'App',
  components: {
    'ejs-treegrid': TreeGridComponent,
    'e-columns': ColumnsDirective,
    'e-column': ColumnDirective,
    'e-aggregates': AggregatesDirective,
    'e-aggregate': AggregateDirective,
    'e-aggregate-columns': AggregateColumnsDirective,
    'e-aggregate-column': AggregateColumnDirective
  },
  data: () => {
    return {
      data: summaryData,
      footerMax(){
        return{
          template: footerTemp1
        }
      },
      customFooter(){
        return{
          template: customAggregate
        }
      },
      footerSum(){
        return{
          template: footerTemp
        }
      }
    }
  },
  methods:{
    customAggregateFn: function(data){
      let sampleData = getObject('result', data);
      let countLength = 0;
      sampleData.filter((item) => {
          let data = getObject('category', item);
          if (data === 'Frozen seafood') {
              countLength++;
          }
      });
      return countLength;
    }
  },
  provide:{
    treegrid: [Aggregate]
  }
}
</script>

<style>
  @import "../node_modules/@syncfusion/ej2-base/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-calendars/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-dropdowns/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-navigations/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-popups/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-splitbuttons/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-vue-grids/styles/material.css";
  @import "../node_modules/@syncfusion/ej2-vue-treegrid/styles/material.css";

  .btn{
    margin: 1%;
  }

  .e-image {
    height: 13px;
    width: 14px;
    vertical-align: middle;
  }
</style>
