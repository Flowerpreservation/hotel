<template>
    <div class="test">
      <top></top>
      <div class="dist">
            <div class="test_container"></div>
            <div class="test_wrapper">
              <!--头部图标-->
                  <div class="test_wrapper_top">
                    <div class="test_left">
                      <img :src="img">
                    </div>
                    <div class="test_right">
                      <p>已选:“{{stype.code}}” <span>{{stcolor}}</span></p>
                      <p class="test_price">
                        <span>&yen;<b>{{stype.Price}}</b></span>
                        <span>x<b>{{stype.Count}}</b></span>
                      </p>
                    </div>
                    <span class="test_close" @click="return_b()"></span>
                  </div>
              <!--中部-->
              <div class="middle_test">
                <h4>尺码</h4>
                <p><span>尺码对照表</span>></p>
              </div>
              <div class="test_type">
                <ul class="test_type_ul" >
                  <li v-for="item in sproduct" :class="{'checked':item.isDefault}" @click="select(item)">{{item.code}}</li>
                </ul>
              </div>
              <!--尺码-->
              <div class="test_code">
                <ul class="test_code_ul">
                  <li class="test_code_ul_head">
                    <dl>
                      <dt>英码</dt>
                      <dd>{{stype.size}}</dd>
                      <dd>{{stype.size4}}</dd>
                    </dl>
                  </li>
                  <li>
                    <span>尺码二</span>
                    <span>{{stype.size2}}</span>
                  </li>
                  <li>
                    <span>尺码</span>
                    <span>{{stype.sizeof}}</span>
                  </li>
                  <li>
                    <span>胸围(cm)</span>
                    <span>{{stype.chest}}</span>
                  </li>
                  <li>
                    <span>腰围(cm)</span>
                    <span>{{stype.waistline}}</span>
                  </li>
                  <li>
                    <span>臀围(cm)</span>
                    <span>{{stype.circumference}}</span>
                  </li>
                  <li>
                    <span>尺码三</span>
                    <span>{{stype.size3}}</span>
                  </li>
                </ul>
              </div>
              <!--颜色-->
              <div class="test_color">
                <p>颜色</p>
                <span v-for="it in scolor" :class="{'select':activeColor == it}" @click="select_color(it)">{{it.name}}</span>
              </div>
              <!--数量-->
              <div class="test_count">
                <p class="test_count_name">数量</p>
                <p class="test_count_type">
                  <span class="count_rd" @click="changenumber(typecount,-1)">-</span>
                  <span class="count_nb" v-model="typecount">{{typecount}}</span>
                  <span class="count_add" @click="changenumber(typecount,1)">+</span>
                </p>
              </div>
            </div>
      </div>
    </div>
</template>

<script>
  import top from '../../components/malltop/top.vue'
    export default {
      components: {
        top
      },
      name: 'hello',
      data () {
        return {
          msg: 'Welcome to Your Vue.js App',
          img: ['./static/test/001.jpg'],
          t: false,
          typecount: '',
          sproduct: [],
          stype: [],
          stcolor: '',
          scolor: [],
          activeColor: ''
        }
      },
      created() {
        this.$http.get('static/skus.json').then((res) => {
          this.sproduct = res.data.skus;
          this.stype = this.sproduct[0];
          this.scolor = this.sproduct[0].color;
          this.typecount = this.sproduct[0].Count;
          console.log(this.scolor);
        })
      },
      methods: {
        select(item) {
//          console.log(item);
          this.stype = item;
          this.scolor = item.color;
          this.typecount = item.Count;
          this.stcolor = '';
          console.log(this.stype);
          console.log(this.scolor[0].name);
          for (let i = 0; i < this.sproduct.length; i++) {
            if (this.sproduct[i].id === item.id) {
              this.sproduct[i].isDefault = true;
            } else {
              this.sproduct[i].isDefault = false
            }
          }
          for (let i = 0; i < this.scolor.length; i++) {
            if (this.scolor[i].no === item.color.no) {
              this.scolor[i].isDefault = true;
            } else {
              this.scolor[i].isDefault = false
            }
          }
        },
        select_color(it) {
          console.log(it);
          this.activeColor = it;
          this.stcolor = it.name;
        },
        changenumber(typecount, way) {
          if (way > 0) {
            this.typecount++;
            console.log(this.typecount)
          } else if (way < 0) {
            this.typecount--;
            console.log(this.typecount)
            if (this.typecount < 1) {
              this.typecount = 1;
              console.log(this.typecount)
            }
          }
        },
        return_b() {
            history.back(-1);
        }
      },
        mounted() {
          this.$store.dispatch('pageChange', {pageName: 'test', pageTitle: 'test'});
          console.log(this.$store.state.currentPage);
        }
    }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .checked{
    border: 1px solid #ff6060!important;
  }
  .select{
    border: 1px solid #ff6060!important;
  }
  .test{
    position: fixed;
    top:45px;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .dist{
    width: 100%;
    height: 100%;
  }
  .test_container{
    height: 20%;
    background: rgba(0,0,0,0.6);
  }
  .test_wrapper{
    height:80%;
    padding:0 10px 0 10px;
    background: #fff;
  }
  .test_wrapper_top{
    padding: 10px 0 10px 0;
    height: 100px;
    border-bottom: 1px solid #ccc;
    position: relative;
  }
  .test_left{
    width:100px;
    height:100px;
    float:left;
    margin-top:-20px;
    padding:5px;
    background-color:#fff;
    border:1px solid #ccc;
    border-radius:5px;
  }
   .test_left img{
    width:100%;
    height:100%;
  }
  .test_right{
    margin-left: 110px;
    margin-right: 20px;
    height: 100%;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .test_price span, .test_price b{
    color: #ff6060;
  }
  .test_close{
    display: inline-block;
    width: 20px;
    height: 20px;
    position: absolute;
    right:0;
    top:5px;
    background: url('img/x.png') center top no-repeat;
    background-size: cover;
  }
  .middle_test{
    display: flex;
    justify-content: space-between;
    padding: 10px 0 10px 0;
    width: 100%;
  }
  .middle_test h4{
    font-size: 16px;
    font-weight: 600;
  }
  .middle_test span{
   text-decoration: underline;
  }
  .test_type_ul{
    display: flex;
    padding: 10px;
  }
  .test_type_ul li{
    align-content: flex-start;
    text-align: center;
    font-size: 14px;
    line-height: 14px;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 3px;
    margin-right: 10px;
  }
  .test_code{
    padding: 10px 0 10px 0;
  }
  .test_code_ul{
    display: flex;
    background: #ddd;
    border-radius: 8px;
  }
  .test_code_ul li{
    flex: 1;
    text-align: center;
    display: flex;
    justify-content: space-between;
    flex-direction: column;
  }
  .test_code_ul_head{
    background: #757575;
    border-bottom-left-radius: 8px;
    border-top-left-radius: 8px;
  }
  .test_code_ul li span{
    font-size: 12px;
  }
  .test_color p{
    font-size: 14px;
    line-height: 20px;
    margin-bottom: 10px;
  }
  .test_color span{
    font-size: 14px;
    line-height: 20px;
    color: #000;
    border: 1px solid #ccc;
    border-radius: 3px;
    padding: 3px;
    margin-right: 10px;
  }
  .test_count{
    margin-top: 10px;
  }
  .test_count_type{
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  .count_rd{
    border-right:1px solid #ccc;
    display:inline-block;
    width:24px;
    height:30px;
    line-height:30px;
    text-align:center;
    font-weight: 700;
  }
  .count_add {
    border-left:1px solid #ccc;
    display:inline-block;
    width:24px;
    line-height:30px;
    text-align:center;
    font-weight: 700;
  }

  .count_nb{
    display:inline-block;
    width:24px;
    text-align:center;
    font-size:14px;
    height:30px;
    line-height:30px;
  }
</style>
