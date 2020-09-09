<template>
  <div :class="['edit_container']">
      <h2>开奖号码</h2>
      <div class="open">
          <div>
               <input type="text" v-model="value['1']" @focus="currentIndex=1"  @input="(e)=>{inputchange(e,'1')}">
                <selectContent :insert="[submitVal['1']]" :key="submitVal['1']" class="the49"></selectContent>
          </div>
         <div>
             <input type="text" v-model="value['2']"  @focus="currentIndex=2"  @input="(e)=>{inputchange(e,'2')}">
            <selectContent :insert="[submitVal['2']]"  class="the49"></selectContent>
         </div>
          <div>
              <input type="text" v-model="value['3']" @focus="currentIndex=3"  @input="(e)=>{inputchange(e,'3')}">
           <selectContent :insert="[submitVal['3']]"  class="the49"></selectContent>
          </div>
          <div>
              <input type="text" v-model="value['4']" @focus="currentIndex=4"  @input="(e)=>{inputchange(e,'4')}">
           <selectContent :insert="[submitVal['4']]"  class="the49"></selectContent>
          </div>
          <div>
              <input type="text" v-model="value['5']" @focus="currentIndex=5"  @input="(e)=>{inputchange(e,'5')}">
           <selectContent :insert="[submitVal['5']]"  class="the49"></selectContent>
          </div>
          <div>
              <input type="text" v-model="value['6']" @focus="currentIndex=6"  @input="(e)=>{inputchange(e,'6')}">
           <selectContent :insert="[submitVal['6']]"  class="the49"></selectContent>
          </div>
          <div>+</div>
          <div>
              <input type="text" v-model="value['7']" @focus="currentIndex=7"  @input="(e)=>{inputchange(e,'7')}">
           <selectContent :insert="[submitVal['7']]"  class="the49"></selectContent>
          </div>
          
          <button :disabled="inputFlag" @click="confirmOpen">确认开奖号码</button>
          <!-- <span v-if="inputFlag">本期开奖号码:{{}}</span> -->
      </div>
      <selectContent :insert="the_49" :select="true" v-if="currentIndex" @closeModal="closeModal" :style="{left:`${currentIndex*80+100}px`}" @editorselect="setinputdata"  class="allthe49"></selectContent>
      <h2 class="h222">资料区内容编辑</h2>
      <div class="buttonGrop">
          <button v-for="item in buttonGrop" :key="item.id" @click="buttonHandle(item)" :class="{'buttonActive':current==item.id}">
              {{item.name}}
          </button>
      </div>
      <div class="buttonGrop">
        <button v-on:click="saveHtml">保存当前编辑栏</button>
      </div>
        <quill-editor 
            v-model="content" 
            ref="myQuillEditor" 
            :options="editorOption"
            @change="onEditorChange($event)"
            class="myQuillEditor"
            >
        </quill-editor>
        <input
        style="display:none"
            ref="file"
            type="file"
            accept="image/png, image/gif, image/jpeg, image/bmp, image/x-icon"
            @change="onFileChange">
        <div class="modal" v-if="passwordSwitch&&passwordTrue">
            <div class="bg"></div>
            <div class="content">
                
                <h2>请输入密码</h2>
                <input v-model="password" type="text">
                <button @click="confirmPassword">确认密码</button>
            </div>
        </div>
        
    </div>  
</template>

<script>
import selectContent from './select'
export default {
  name: 'App',
  props:['defaultdata','id'],
  data(){
     return {
            content: ``,
            editorOption: {
                modules:{
                    toolbar:[
                        ['bold', 'underline', 'strike'],    //加粗，斜体italic，下划线，删除线
                        // ['blockquote', 'code-block'],     //引用，代码块
                        [{ 'header': 1 }, { 'header': 2 }],        // 标题，键值对的形式；1、2表示字体大小
                        [{ 'list': 'ordered'}, { 'list': 'bullet' }],     //列表
                        [{ 'script': 'sub'}, { 'script': 'super' }],   // 上下标
                        [{ 'indent': '-1'}, { 'indent': '+1' }],     // 缩进
                        [{ 'direction': 'rtl' }],             // 文本方向
                        [{ 'size': ['small', false, 'large', 'huge'] }], // 字体大小
                        [{ 'header': [1, 2, 3, 4, 5, 6, false] }],     //几级标题
                        [{ 'color': [] }, { 'background': [] }],     // 字体颜色，字体背景颜色
                        // [{ 'font': [] }],     //字体
                        [{ 'align': [] }],    //对齐方式
                        ['clean'],    //清除字体样式
                        ['image']    //上传图片、上传视频 ‘video’
                    ]
                }
            },
            currentIndex:0,
            currentId:'',
             the_49 :["AR01", "AR13", "AB25", "AB37", "AG49",
                    "BR12", "BR24", "BB36", "BB48",
                    "CG11", "CR23", "CR35", "CB47",
                    "DB10", "DG22", "DR34", "DR46",
                    "EB09", "EG21", "EG33", "ER45",
                    "FR08", "FB20", "FG32", "FG44",
                    "GR07", "GR19", "GB31", "GG43",
                    "HG06", "HR18", "HR30", "HB42",
                    "IG05", "IG17", "IR29", "IB41",
                    "JB04", "JG16", "JG28", "JR40",
                    "KB03", "KB15", "KG27", "KG39",
                    "LR02", "LB14", "LB26", "LG38" 
                ],
                value:{
                    '1':'',
                    '2':'',
                    '3':'',
                    '4':'',
                    '5':'',
                    '6':'',
                    '7':'',
                },
                submitVal:{},
                inputFlag:false,
                buttonGrop:[
                    {name:'精典五肖',id:'0'},
                    {name:'新加坡传真',id:'1'},
                    {name:'民间高手',id:'2'},
                    {name:'精英好料',id:'3'},
                    {name:'公式规律',id:'4'},
                    {name:'传真20码',id:'5'},
                    {name:'一肖一码',id:'6'},
                    {name:'解玄机诗',id:'7'},
                    {name:'解藏宝图',id:'8'},
                    {name:'新彩图库',id:'9'},
                ],
                current:'0',
                textAreaData:{},
                password:'',
                passwordTrue:true
        }
    },
    components:{
        selectContent
    },
    computed: {
        editor() {
            return this.$refs.myQuillEditor.quill;
        },
         baseApi() {
            return baseApi()+ '/sys';
        },
        baseUpurl(){
            return baseApi();
        },
        passwordSwitch(){
            return passwordSwitch
        }
    },methods: {
        onEditorReady(editor) { // 准备编辑器
        },
        onEditorChange(){// 内容改变事件
           
        }, 
        saveHtml:function(event){
          this.settextArea(this.current,this.content)
        },
        onFileChange(event) {
            //通过input本身的change钩子，对图片进行处理
            let tar = event.target;
            let files = tar.files;
            var formData = new FormData();
            formData.append("filename", files[0]);
            this.$http({
                headers:{'Content-Type': 'multipart/form-data'},
                method: 'post',
                url: this.baseUpurl,
                data:formData
            }).then(res=>{
                if(res.data.code=='01'){
                    let url =this.baseUpurl+res.data.msg;//将图片插入需要获取当前光标的位置，参数为是否要求焦点在编辑器上
                    let selection = this.$refs.myQuillEditor.quill.getSelection(true);//调用insertEmbed 将图片插入到编辑器
                    this.$refs.myQuillEditor.quill.insertEmbed(selection.index, "image", url);
                }else{
                    alert('图片上传失败，请稍后重试')
                }
            })
        },
        imgHandler(){
            this.$refs.file.click();
        },
        setinputdata(e){
            this.value[`${parseInt(this.currentIndex)}`]=parseInt(e.target.value);
            this.inputchange(e)
        },
        confirmPassword(){
            if(this.password==password){
                this.passwordTrue=false;
               
            }else{
                 alert('密码错误!');
            }
        },
        inputchange(e,key){
            if(e.target.value>49){
                this.$set(this.value,key,'');
                alert('输入数字不能大于49，请重新输入')
            }
            let valAry=Object.values(this.value);
            valAry.forEach((item,index)=>{
                let twoItem=item>=10?item:'0'+item;
                this.the_49.forEach((item1,index1)=>{
                    if(item1.slice(2,4)==twoItem){
                        this.$set(this.submitVal,`${index+1}`,item1)
                    }
                })
            });
        },
         dataInput(){
            let data={ "tag": "input_number","data": this.submitVal}
            this.$http.post(this.baseApi+'/api',data).then(function (res) {
                if(res.data.code=='01'){
                    alert('保存成功!');
                }
            })
        },
        dataInit(){
            let _that=this;
            let data={ "tag": "output_number","data": { }}
            this.$http.post(this.baseApi+'/api',data).then(function (res) {
                if(res.data.code=='01'){
                    _that.inputFlag=true;
                }
            })
        },
        closeModal(){
            this.currentIndex=null
        },
        confirmOpen(){
            let flag=true;
            let stringVal=[];
            let valAry=Object.values(this.value);
            valAry.forEach((item,index)=>{
                if(!item){
                    flag=false
                }
                let twoItem=item>=10?item:'0'+item;
                this.the_49.forEach((item1,index1)=>{
                    if(item1.slice(2,4)==twoItem){
                        this.$set(this.submitVal,`${index+1}`,item1)
                    }
                })
            });
            if(!flag){
                alert('获奖数字不能为空，请输入！')
                return 
            };
            let alertString=Object.values(this.submitVal).join(',');
            if(confirm('确定获奖数字为当前数字吗？确认后不可更改')){
                this.dataInput()
            }
        },
        buttonHandle(item){
            this.content='';
            this.current=item.id;
            this.gettextArea(item.id);
        },
        settextArea(mark,textHtml){
            let data={ "tag": "set_article","data":{mark:mark,text:textHtml}}
            this.$http.post(this.baseApi+'/api',data).then(function (res) {
                if(res.data.code=='01'){
                    alert('保存成功!');
                }
            })
        },
        gettextArea(start){
            if(this.textAreaData[start]){
                this.content=this.textAreaData[start];
            }else{
                let data={ "tag": "get_article","data":{mark_start:start,mark_finish:start}}
                this.$http.post(this.baseApi+'/api',data).then(res=> {
                    if(res.data.code=='01'){
                        this.$set(this.textAreaData,start,res.data.msg[start]);
                        this.content=res.data.msg[start];
                    }
                })
            }
        }
    },
    mounted(){
        this.$refs.myQuillEditor.quill.getModule('toolbar').addHandler('image', this.imgHandler);
        this.dataInit();
        this.gettextArea('0')
    }
}
</script>

<style>
.edit_container{
    margin: 20px 
}
.edit_container div.ql-editor{
    height: 600px;
}
.ql-editor img{
    width: 200px;
}
.myQuillEditor{ 
    width: 900px;
}
.open {
    margin: 20px 0;
    width: 900px;
    display: flex;
    margin-left: 100px;
}
.open>div{
    display: inline-block;
    width: 80px;
    position: relative;
    line-height: 36px;
}
.the49{
    position: absolute;
}
.open input{
    width: 20px;
    height: 30px;
    text-align: center;
}
.open button{   
    width: 100px;
    height: 36px;
    margin-left: 20px;
}
.open button[disabled]{
    cursor: not-allowed;
}
.buttonGrop button{
  
    display: inline-block;
    margin: 20px 5px;
    margin-top: 0;
    font-weight: 400;
    text-align: center;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
    background-image: none;
    border: 1px solid transparent;
    white-space: nowrap;
    line-height: 1.5;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    padding: 6px 15px;
    font-size: 12px;
    border-radius: 4px;
    transition: color .2s linear,background-color .2s linear,border .2s linear,box-shadow .2s linear;
    color: #657180;
    background-color: #f7f7f7;
    border-color: #d7dde4;
}
button.buttonActive{
    color: #09c;
    border: 1px solid #09c;
}
.buttonGrop button:focus{
    box-shadow: 0 0 0 2px rgba(0,153,204,.2);
    outline: 0;
}
.buttonGrop button:active{
     outline: 0;
}
.h222{
    margin-top: 120px;
}
.the49{
    position:absolute;
    left: -11px;
}
.allthe49{
    width: 650px;
    border: 1px solid #ddd;
    background-color: #fff;
    position:absolute;
    left: 100px;
    padding: 10px 20px;
    border-radius: 10px;
    z-index: 9999;
}
.allthe49 ul{
    display: flex;
    flex-wrap: wrap;
}
.allthe49 li{
    cursor: pointer;
    user-select: none;
}
.modal{
    position: fixed;
    width: 100%;
    height: 100%;
    z-index: 100;
    left: 0;
    top: 0;
}
.bg{
    width: 100%;
    height: 100%;
    position:absolute;
    left: 0;
    top: 0;
    background: rgba(0,0,0,.5);
}
.content{
    width: 600px;
    height: 400px;
    position:absolute;
    background-color: #fff;
    top: 200px;
    left: calc(50% - 340px);
    padding: 20px 40px;
}
.content input{
    width: 100%;
    height: 40px;
    box-shadow: none;
}
.content button{
    width: 200px;
    height: 40px;
    position:absolute;
    right: 20px;
    bottom: 20px;
}
</style>