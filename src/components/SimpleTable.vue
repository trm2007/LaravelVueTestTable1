<template>
  <div class="hello">
    <table class="table table-bordered table-hover table-sm mb-5">
      <thead class="thead-light">
        <tr>
            <th scope="col">№</th>
            <th scope="col" v-for="(ColNum, Num) in ColNums" :key="'H-'+ColNum">
                <button @click="changeCols(Num, -1)" class="btn btn-secondary btn-sm">&lt;</button>
                <span @click="sort(ColNames[ColNum])" style="cursor: pointer;">{{ ColNames[ColNum] }}</span>
                <span v-if="SortColumn[0]==ColNames[ColNum]" class="red bold">
                  {{ SortColumn[1] > 0 ? "^" : "v" }}
                </span>
                <button @click="changeCols(Num, 1)" class="btn btn-secondary btn-sm">&gt;</button>
            </th>
            <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(Item, i) in ItemNums" :key="Item" @click="setCurrentItem(Item)">
            <th scope="row">
                <button @click="changeItems(i, -1)" class="btn btn-secondary btn-sm">^</button>
                {{Item}}
                <button @click="changeItems(i, 1)" class="btn btn-secondary btn-sm">v</button>
            </th>
            <td v-for="(ColNum) in ColNums" :key="ColNum+'-'+Item">{{ TableData[Item][ColNames[ColNum]] }}</td>
            <td><button @click="CurrentMessage=Item;" class="btn btn-alert btn-sm">Info</button></td>
        </tr>
      </tbody>
    </table>
    <new-data v-if="CurrentItem===null" @new-data="onNewData" />
    <new-data v-else @new-data="onUpdateData" 
      :originalDate="TableData[CurrentItem].date"
      :originalText="TableData[CurrentItem].text"
      :originalDigits="TableData[CurrentItem].digits"
    />
    <modal-info v-if="CurrentMessage!==null" 
      :originalMessages="ItemMessages[CurrentMessage] ? ItemMessages[CurrentMessage] : []"
      @new-message="onNewMessage"
      @close="CurrentMessage=null"
    />
  </div>
</template>

<script>
import NewData from './NewData.vue'
import ModalInfo from './ModalInfo.vue'

export default {
  name: 'SimpleTable',
  props: {
    msg: String
  },
  data() {
    return {
        TableData: [
          {date: "2020-01-01", text: "old", digits: 123},
          {date: "2020-09-22", text: "now", digits: 456},
          {date: "2020-09-30", text: "next", digits: 789},
        ],
        ItemMessages: [],
        ItemNums: [],
        ColNames: ["date", "text", "digits"],
        ColNums: [],
        SortColumn: ["date", 1],
        CurrentMessage: null,
        CurrentItem: null
    }
  },
  components: {
    NewData,
    ModalInfo
  },
  mounted(){
    this.resetItemNums();
    this.resetColNums();
  },
  computed: {
      realColNames(){
          return [
              this.ColNames[this.ColNums[0]], 
              this.ColNames[this.ColNums[1]], 
              this.ColNames[this.ColNums[2]]
            ];
      },
      realTableData(){
          return this.TableData.map( (item, i) => {
              return this.TableData[this.ItemNums[i]];
          });
      }
  },
  methods: {
      resetItemNums(){
        let i = 0;
        this.ItemNums = [];
        for(i=0; i<this.TableData.length;i++)
        {
          this.ItemNums.push(i);
        }
      },
      resetColNums() {
        let i = 0;
        this.ColNums = [];
        for(i=0; i<this.ColNames.length;i++)
        {
          this.ColNums.push(i);
        }
      },
      sort(Column){
        if(this.SortColumn[0] == Column) { this.SortColumn[1] *= -1; }
        else { this.SortColumn[0] = Column;  this.SortColumn[1] = 1; }

        this.TableData.sort( (a,b) => {
          if( a[this.SortColumn[0]] > b[this.SortColumn[0]] ) return this.SortColumn[1];
          if( a[this.SortColumn[0]] < b[this.SortColumn[0]] ) return -this.SortColumn[1];
          return 0;
        } )
        this.resetItemNums();
      },
      setCurrentItem(Item){
        this.CurrentItem = Item;
      },
      addData(MyDate, Text, Digits){
        this.TableData.push({"date": MyDate, "text": Text, "digits": Number(Digits)});
        this.ItemNums.push(this.ItemNums.length);
      },
      onNewData(NewData){
        this.addData(NewData.date, NewData.text, NewData.digits);
      },
      onUpdateData(NewData){
        this.TableData[this.CurrentItem].date = NewData.date; //new Date(NewData.date);
        this.TableData[this.CurrentItem].text = NewData.text;
        this.TableData[this.CurrentItem].digits = Number(NewData.digits);
        this.CurrentItem = null;
      },
      onNewMessage(NewMessage){
        if(!this.ItemMessages[this.CurrentMessage]) { this.ItemMessages[this.CurrentMessage] = []; }
        this.ItemMessages[this.CurrentMessage].push(NewMessage);
        this.CurrentMessage = null;
console.log(this.ItemMessages);
      },
      changeCols(Num, Count){
          let NewNum = Num + Count;
          if( NewNum < 0 ) { NewNum = this.ColNames.length-1; }
          else if( NewNum >= this.ColNames.length ) { NewNum = 0; }
          if(NewNum == Num) { return; }
          this.ColNums = this.ColNums.map( (item, i) => {
              if(i==NewNum) return this.ColNums[Num];
              if(i==Num) return this.ColNums[NewNum];
              return item;
          } );
/*
********** VUE отслеживает изменения в массиве, сделанные только через функции splice, sort, map...
********** или путем присвоения переменной всего массива нового значения
********** поэтому код ниже не реактивен...
          let Tmp = this.ColNums[NewNum];
          let TmpArr =[];
          this.ColNums[NewNum] = this.ColNums[Num];
          this.ColNums[Num] = Tmp;
*/
      },
      changeItems(Num, Count){
          let NewNum = Num + Count;
          if( NewNum < 0 ) { NewNum = this.TableData.length-1; }
          else if( NewNum >= this.TableData.length ) { NewNum = 0; }
          if(NewNum == Num) { return; }
          this.ItemNums = this.ItemNums.map( (item, i) => {
              if(i==NewNum) return this.ItemNums[Num];
              if(i==Num) return this.ItemNums[NewNum];
              return item;
          } );
      }
  }
}
</script>

