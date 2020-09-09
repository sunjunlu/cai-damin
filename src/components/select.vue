<template>
    <div>
        <div data-v-7bf92f17="" class="current-info">
            <div data-v-7bf92f17="" class="current-info-inner">
                <div data-v-7bf92f17="" flex="cross:center" class="open-num lhc">
                    <ul data-v-7bf92f17="" flex="" class="open-num-list">
                        <li data-v-7bf92f17="" @click="listhandle(item,index)" v-for="(item,index) in valueAry" :class="{'symbol':item==null}" :key="index" flex="dir:top" class="open-ball">
                            <span data-v-7bf92f17="" v-if="item!=null" flex="main:center cross:center" :class="{'redBg':item.color=='R','greenBg':item.color=='G','blueBg':item.color=='B'}" class="number ">{{item.number}}</span>
                            <b data-v-7bf92f17="" v-if="item!=null" class="sheng-xiao">{{item.name}}</b>
                            <!-- <span class="nullAdd" v-if="item==null">+</span> -->
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <div v-if="select" @click="closeModal" class="close">X</div>
    </div>
</template>
<script>
export default {
    name:'showNumber',
    props:['insert','select'],
    data() {
        return {
            value:{},
            valueAry:[],
            values1:[],
            twelve:{
                "A":'鼠',
                "B":"牛",
                "C":"虎",
                "D":"兔",
                "E":"龙",
                "F":"蛇",
                "G":"马",
                "H":"羊",
                "I":"猴",
                "J":"鸡",
                "K":"狗",
                "L":"猪",
            }
        }
    },
    methods: {
        changeValue(){
            let values
            let keys=Object.keys(this.value);
            if(this.values1.length>0){
                values=this.values1;
            }else{
                values=Object.values(this.value);
            }
            this.valueAry=[]
            values.forEach((item,index)=>{
                if(!item){
                    return
                }
                if(index==6){
                    // this.valueAry.push(null);
                }
                let itemVal={};
                this.$set(itemVal,'name',this.twelve[item.slice(0,1)]);
                this.$set(itemVal,'color',item.slice(1,2));
                this.$set(itemVal,'number',item.slice(2,4));
                this.valueAry.push(itemVal);
                
            });
        },
        listhandle(item,index){
            if(this.select){
                let data={target:{value:item.number}};
                console.log(data)
                this.$emit('editorselect',data);
            }
        },
        closeModal(){
            this.$emit('closeModal')
        }
    },
    computed: {
        getinsert(){
            return this.insert
        }
    },
    mounted() {
        if(this.insert){
            if(typeof this.insert=='string'){
                this.values1=this.insert.split(',');
            }else{
                this.value=JSON.parse(JSON.stringify(this.insert));
            }
            
        }
        this.changeValue();
        
    },
    watch: {
        getinsert(){
            this.value=JSON.parse(JSON.stringify(this.insert));
             this.changeValue();
        }
    },
}
</script>
<style >
    .nullAdd{
        font-weight: 700;
    }
      .current-info .current-info-inner .open-num .open-num-list .open-ball .number.redBg[data-v-7bf92f17] {
        background: url(data:image/png;base64,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) no-repeat;
        background-size: cover;
        display: inline-block;
        width: 50px;
        height: 50px;
        text-align: center;
        line-height: 50px;
    }
    
    .current-info .current-info-inner .open-num .open-num-list .open-ball .number.blueBg[data-v-7bf92f17] {
        background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAF8AAABfCAMAAAC5kuvHAAACalBMVEUAAAC61OZuhJO91eXL3+0GBwmvytzN4O4DBAXL3+0DBAQCAwMFBwgFBwjL3+3K3uwBAgIBAQIBAQEBAQECAwMAAAB0osAAAAAAAAAGCAkAAQECAwW/1ue1z+ABAQIBAQG71OUAAABBaIIAAABZlb0CBgkBAgOev9ZamsWTts4ob56lwtS+1OMRFhkeY5EscZ6cvdKtxteUuNBWlcAwT2ShwNQobJogUXFVkrxtocMRKTkUNkuCrMmqx9uGrMaFrciMr8cdUnSIss3D1eEVNUtelLlRlMGKrMNdmcEkaZV2nLd9mawiXYR1pcQvgLawy9xPirIrc6OXus+jwtVoncAtb5uHrMSxzN0ob5/B1+ZPdIwlN0Ti7fSKrMJfg5snSV/////7/P4nb53w9vo4fqxNkb5ipNHY5vAtcqDr8vfj7vVFibgyeaj8/v9zrdUyd6XF2Obn7/Xf6/TP3+k+hrU1fKpNl8lUmchSm8xpqNNFjsA5gLD3+/3G3u281OQjaphYns7e6PDU4+3J2+irzeVsqtQteq0udaQocKDa6fLL4e98s9h4sdhboM+lyuSBttpCirs8grHA2uy11Oivy95Kk8S61+qVwd6Lu9zQ5PGaxOCQv98qc6Tz+PtBgaufx+JypMZlm8Coxtx6q8xNibGSvNhBhLAdZ5cWYpSv0eeGudujwdSItdSBsNBfnsmcwdpuqM9kmLtdlboPXZC+0t5opMyIr8lEk8hWkLhHhq83eKMydJ89ibwgbJ5snr84h7wxf7MsaZGtx9hXjK8+eaAjYYqbvNGVuNA7jcRYl8EVUXhulK0jWoCEpbxfE6EDAAAAYHRSTlMAAwIGDAcJFhoRDyMLFBwlNEAuRDp6FpBdKlaIMf5QSj5mFW/xqpll/nnwurUz76ikkYbhJ/DX0c24sohjU0suJN7dzcu2qZyPYFc/7Ojl4eHd29nVwbuoh2lZRtDAfnBy0n0xAAAMg0lEQVRo3uyW2W8SURSHWSqEF3jzCZLGmNRa467RxC0a9xj37Q2LnXFGwWFQFi0jZYlSyqAFWW0RWkxAtNq41CWa2tbd/8lz7pTSjq1CYnzyR1t4oN93zpl774zif/7nX0WJ+Zs8CUqirgU+KutCFb6aQM7+7mxsSy0a+EUJCcrU9ZZUqgb5dTjgIDqdFqKDd+Rr6i6NRq3BnjD43w3B63gga/UkuhatwdhKYjRo1QqlZAQhetAg5XctqLDBOh7oCDcY4K+xbe3ezSfWLcUc2bR7X/tWA7glNVhqhkYXACkeage4Qa/funbzuuHhsdGnBcjT0bGx4cFLl4/sbm/V6QzkC9AICGpjWsBQL15F+FCewWgwtLafWjo6Wrg/NDT0CDM0dP9+oTAKjsHBzW1G+I4RFFpsYUahUv6ePz0cqO3s6RNjo/eBXK2WSqVUaqRUqlbR8Q4beTW4qb3VCAbsgVwHDDHMy1fN8NUanP3WFcIHgKdLqWxeFIMkophNlarQB1EMnz+066zBQAQ6NNRX08JREbzWsMbEOYRqKS8G4/FkMtkvJRkvB8URUIABevjEmU634iDJdcYsZKiPB/i6FmOH4PDT3Egw2e/19s6KFxTxYL5UJYKiP8AdbtNLl3nOcpXRZ69N2DhbD3P+cA/vFXozUpBMgj0k48FsGgSFiYe83x8Q1qBgZkfgkOSCWXjctG1FzhFmed5Ph8NhOgNYpELhZQhehnwqXXn0pPDqhZ0POzhh9Q4t8jEoWJhPhq9pLwqBMGu3szw4AI9guLT5fDabSuUw6UosZntSmJhwMLyfE0wdO3TTAtIB8uXTIRIcjnptAvB2hrEwjJ2ne5NlEcC5dLoC0Eikj8Rms4Wox2OTHy2snzOZhI7FOu10fhUgvVY+VJ8QuLDdAmHYcG9/XAR2BcEEGgp5ICEPBXn8ZXCix8IHhETC1LG4ZZaAnKvyELx6S5Hjwgzi7UgP5nOxSJ/N46GiPp8b44tSUqJRV9ebF932noCpmDCt1uhmNzAvHhbmtg1cgGYsAxYL6xDiQTGbjtg8vmgolsWl2p8Mpvoot9NHRSE+V9fg5LNzIEiAoF39+/px9ou3HwM8i3ieM5F10kdFQ6l+tttcyzk2maOcTh/yoYEJttvOYweJberf8FWI1+pXw7ZiLRBeSBTz2Vws5LOVGcRaredIiIMpe146fT5oYHj828AAwwZMCeH4Yo2uzpcJ8EjWa9sSNM0zBF8sZrH4UHwA2N0A7safGcdAnHrp9rlhQuM/ukEAu4Dbq9JhWghfXr0aqjce94cJnuUSxZF0LBRNsTAQTM2AL0lhT0ML7q6u8+M/rN0MSzsCwkGlhNfIx0O2rV6/VujhWQbxJsDD5JNIr+OlFiSB1WyOO19CAxcnp34MWBk7S2c6NPUjaM72Qr5O27ohjOUzvENIjFRtUU8G8daaYU6sVhBkfETweupbj9kCLQQOqGA0El5+Nwf1Wo6F8nHHEzzFm63nai9ZzFYz8M08CJxdryanJp+ZrUwP3UFuAfPw8Za1YyceZzwcCQLgKcQDeZpE3mqpf+Sd04Lv3xxmK+s4oPj1dJ5+1FFsGXvxMBHo4TPeYKmP8nnNSP9TvC+dIHgwOf71+7eHXGavQq2Q8YGuJPzdY+9eJPw87Y1nIyGniFU2kPxnNwpej3//+nX8zSq4uyvBIKse+IrtXYX7JSEc7k2KaZvbBvMAfgPxEMGdN+NTYFi5XoGPpHK8Evhbup4PjXjDdL+YilHuDOIbSgYm5L57+zwIpqZu7Jc9hwJZI/E3dz2P5L3hTDxbsTkrOJ0GE8MGrl25eqHzxo0bq4A5F092m1qpW+fyREQv7Q3mYh43DfhGQ0sNXL90obOz8+gi2dJpkfiK1vdwCIve3mS+0ueLmJuJ7TPwr925evNWZ+e99fLyUQDjaXtLhWL5ZH85FbPdjTfFj78nAyL8zv3z8TVqxb67lCeSLcfFXIR6zDbFt+Mmu/b2POGfmYePE9rjBH4qWM6nIz6bubk8+ex2wgq6jPxVsr1F+C1K3SYnFYrkRBFWjyvVJL/03unEC3wP+Cfl57KGbF79Ohfy8yKM31Vukh98K/EvQAMbNbIGIMA3LHVRtkgaHnQiT1zeJvnet3edd69B/cBfvmQOHzPD76uk4J74fBndJJ9emI8Kwl8G8+mL5XIVG+Xim+TzPyux+58k4jgO4FcGK6IiU0IrDV32ZM+Pq9bzWs9b/dAPPQd3HXAk0J2x0EUHi7OnBVtttZBZkbBopoEsSJKoaf5XfT73zVisw+Ndc1rb683nw5ebd2M+8COVPok8AL5+9F++ftY1xMCVvzZf8L1Q9nECaiH6XY8/vESfrdFnwfcp+GQEat42OP9dT559eDy8hgvW6Ad7wK84PxV+3e4XT+ENfvwM/ec17v950efzVZz/yhwD/0EX/JI8PCRM1uh/DIFf8fmtzHHffVgQBPyBGv03obDPJ1Rcfyqyb9x7EwYAf5SJ22virYPFsC9Mrp99JxT85UXv05t3HzwYHh6dct6ryb9XEMAnxydxQMGvO3vnPvp3R4em6Hc1+QPghwVyfDbWUQo55vPevAsBn4uJNfCiy4k+jX5iE6WUk+M4APprGClbg59N9AhhLtIt2WA9FxX9eWe99+HeCgrWMDHaoZp32KIRDtcjWQqJjUZFf9bht28/PX4Kd29rmHjfN9X+SJKOcJxARxMFVx7WM1vJ73TzvPtt133vGoaLFdR+hktJiRaA73N9zqZKWyjlB4org6LIBtzvvd4pJudysap41lLo7hGEUGgkxQd493K81iilU7Tjfc6nIYaJ5ywxuwrenk+66IgQGpzkWUgnNbtO2V/udsBdNcunoCAX8/+wzshb42nJ2dMT+lziA/DAQlxJacFXzGa3B2422WBqionH6OSMlwl7PG1z0XTozQqexScKnZRWW+2B7rwdvL2338O6JxguR+eTP91VeXceeCfwGV6Eu0K7eABePvjKWZ0SrVAQyGQ5Dh5DJv2TVfhJf9Jmc3WHRkpB0d5v9Xi2II9+lQ1lPL03+h18ZkAYpPNf/em40gjueDoh819Kbhb520eMuJ1q/CxqznnYUK9VDJamoMD11ZL0j2T+o2dGEmkL8s5cCnirFc7dfq2xDv2qWbSDtfb22ln3JMdF6Dw0pBM/Uvw/OJ/6kUj7Zb67O+sOeIAP8Fu1Ru2s6j5OsDKFRwHegoExbpB25r/aLMm0P/dtYkWQFdngiolvOX866bfIPB39luFlPrDZiDyuf4aC1RNwGuAePsWEucEYjAAN/mQ6mUjA7U8ikUQcdQl452CJv22H5QTWHzTOqSN8dR8KJt2sw8MGh30MByPQeZvNgvFj8BuiO7tpOjrAw/I94sNOo9GIZwfXM9MA1P5sBj6N4qcXDMNwg7AkGIFUEJzowDtjz0VcpqPTCMtXxct336cOBx12zz3vCy+DI0SgQbKV4yI63V0YkR9G9W8xwvLVvXx8Cqel2i44btj5UZ/XW26ASPjFiTjqTqmQ9Xist3rXUsBrVSyfPKzR6Nuo2VcfXXeMjjNYgA1CpAdEDNqow0h9fRMe+/XLp6i2traKj9ZsBR15jV63QEdduvJqqMgxJBw0QMWfyLoL3pLCxPX+Ldv129HXlPlqWyL83PnzdQuunfs1FuY44mMBVEQisu6Uora+aKhw5sjFBXObts9d0KbXqHz5hG+qN9TPNezdOf5LEDgMsQnukiTUQzv3XtrQtGFD0/y5C3R6DRTMJrTy24zb+cM3LF6y2NCydxc9PhaWtw802E5JkqJSKBRat2tvc8OSJYsbGqYLyr4iP+3LfGuzyWQyL9uza13P2HixWAz9SbEY3Xhoz7J2s9nU3NK6uMGwoeyjMcPhkX3kW5pN7Y2NjcuWdew5uuvQ6Y3w4fV/33j60NGjezqWLoM0mk0trTABDiD7BK/BN7cDvxTS0bFqOh0d8BMWNLabm//1KxevvH/DdIG5vR1GwJJy0G4E3UR4Q315P9V9cvrBl98AbIAKE5Zgy3TgBzPuHnWZbyr7VQPt0wvCE2RogCGWtLZiC8ZE0gxwS0sr2osbQK+fT3jwVV0d5AJsaKqHDiiBSTBLSPBbgEE2gF3fBLpqHgcgBdAAFdABJRADNpVTj4H/mw846DJPfJUT6HVQAR3YohCUCa7TqdbJCBqIHjuwBHsqgyxGB9HrNcir9zEaUoElfwMc+UOCNNqIk5OpfoByCYm+HCDhL3GJTXTCq56BNJAaKMIv5ZB/wBBbEf8Ns/3Q32XMqMsAAAAASUVORK5CYII=) no-repeat;
         background-size: cover;
        display: inline-block;
        width: 50px;
        height: 50px;
        text-align: center;
        line-height: 50px;
    }
    
    .current-info .current-info-inner .open-num .open-num-list .open-ball .number.greenBg[data-v-7bf92f17] {
        background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAF8AAABfCAMAAAC5kuvHAAACglBMVEUAAADU4c4QHQnw9e3v9O3j7N8AAQA0ZRsCBAEAAQABAwAECQLe6doAAQCfu5Hp8OYBAgABAwEAAQAAAAAAAQABAgEAAAAAAAAAAAAAAAC90rQ9cyEAAAAAAABXgT4AAADn7uQzbhaStIM8ax4CBgAnQBuDpXMpWwtTgznr8OfP3sgmVwgpWgqlwJhOhDOvxqKvxaLD1bpWkj0gSgceQAlmkU+6zK+Rr4COrX65za6RroKmwZkqWwvB07aYt4gRIgcqXQqnvJsCAwKOsH2as40VLQeivpMrXgx8o2p8nGw3USdJeTLV4s9fjUefvJFRgTcnVgtvlliWtYbj694mbwVIdSyMsXkUKAiHrHZ1mWEqXApKdTTI2MBzkGRollK90bN3mWJXkT7///8pXAjs8ukwaxlko0wpXxex0KXn7+Pb5tYsYQ1Kky57sWdxq1sxahSdxI52rmFqp1NSljdNljL09/Lj69/g6NvY49IwZxEgVBDD1LvA2bWMuno8eh+myphBhyXw9e1Eiig2chlVmzssZhglXBTQ3cmGuHMjWBIrXgvV4c6sy5+717Grw6CSv4FHjys4dyDL2cSXwIiIsHZfn0ZDgis8fyIuZQ7R4srF3bxHejE/giMybh05dhvH1760yaqjxZVanUHe6ti31KyBs20/fCg2cCE8cSgbUAv4+/e+0rS6zbEwZR8WTAbK4MJvpFpQfjdHhy80bhV2nmFdh0VMkjElZQpxml05iRsvfA8KQwB7pWhjjE9ZlENOhDpLizEEPQDU5c2evo9pkVYQSQEcVQCXsI6SuIJfmUpZj0U1fRltmFZmmVFSjjxCjyVmn1BFcDgpbwuBqm4hXgNOd0IcKJfYAAAAYnRSTlMAAgYFCA4SDBsxIw0aQQkSOipGWVONYE2TahYWfHT9hSIREP6rF/354y4m8ua5rXxPN/Dn1LGsc2Zj/OjXzrKyp6Sel5CJXltTPDgj59TSzMiRgurl38/GtbSHhYBrMunnyawOnDUAAAwVSURBVGje7Jbpb9JgHMdbofENwfeYmAmJ8b4Sr3hEo/FKPBNN9IVH1zQFKQKFgYFaQBKIA7pQppYZx5kYISIec1vmETQ6dTqj/kH+ngcRqceGxzs/g2Ys7PP9Pb9fn7bEf/7z7yEJEg4Y9Al9+DtS+Gmi05E6fERg/x9ntMQY/RfgNx2kdPC7dh3Z8lIIJG7+vZWFIXXtxnVhhxeyU/MwJKEz9CzB9Bj08AU9TqRwjrG1nhkCSE35SG4wGCCkZ/WqI+sOLF68YMGCxYsPrTyyabUBZM1oiDEaZxGgketAj+yUfs2GE4stlqH6ggcPwF+vD920XO49tm7jEpI0IHCErqtZgN+on4f+demmE/V6/cHzqdHR+4jRqalHKGfIYrGs292jN/T0oAgIAJAf3jO1B76EOg/2TTsODD2ZGr0/MfEx84WPE5DxHGXULZZDG9YYcIKJwvP+1SrmaHpvotasD+SeT01kMuLkpKI8cyKeKZMizsARQxfObN1kMCyFFeAAxMxzmIP0emrV6bQ/nckoTklKcG0SeacyCRGQAGt4ma6uX2PCAZAw0xpa5Rspcun6QL+PiSkSV0oG2ySTJYiQnJOZidEpCEj504Gtqyk8g1YC2RyEVvuN3kis3jxsszJelcvWarUsgN1gL4EfBYjlidG3D+rPrH51uLrKbIIRINpL0No79BtTgX6r1+ttqI2GqtZw2dgL5PMwB1ks83fevh5aZPX5Y4HqDpMZ/K0E7AfTj8YLepLYkKqk+7y36LCtWLQ1asESmKHtiizLoijGgTLPu+68f/z0db/Vlq5UcyvMRhTQbFL7MvgduPqjqepwEfQ009dnU7NJLu+UQVtGUpfLgXG73ezg43fXnzG2WDWRQAEUph3ww/bMQc2pBGxhmqa9fUUonpMUcPOuL1LEILwGBwuFqBB6bbP2BxL5VG6bEQI6VkBowHozsbepB/oatSSXgFHyDig2Kng8dsDjEaIFcAOCMPJmMlxUK6ln+dxOohWA/QitHfTkvs2BYT+D9dAZKa/EeQcrRFleTiThNCpJsqNgn/YguQCJoVAlbFMreQjYQJjbHWr5NXqz6SDS3wI9E0TnoRx3DUZZOWml21hLYmF6GukFz8jYYx/j64eAVGofofVrR2sy7qrEmvpwEuyKWGajDo6htTAJdmzaA4zYx+8zKCCVzx00k9/4Qajdtib96pzNx3ixXsLFF9wlSPsBt7joGA4YGM9AgD+Qy1V2oRFo+t9ujpEymQ76mTDSe4Noj/JsVLbSP8MaR0sYGYm8yXghID0cqJ4izHqEdgPg6qH8VQHGi5uTReeNqxAt0b+Cm0ZLsIfGPzJhn80fS68gjEYo/kflk7C0pZv7wkhvzZY4Z9wVZRv0r1GFMXTOhj5M+WGz+GPDewnjV/23NxRATxGrAqh6b7EWRPoCW6RnwuZBAQOhD3er3rCvP7aCIEHe6Scx8GBjOm5jGCvs2WwyD6dlAfQzUhz4EvBwNH0rXIztwzqtHfuJ3U8zeS5Yg+tlQnSxQpaeDUEYsn0gcv3ep/P3U8OxXUTr4qn164h1grssZVU1W3LybrtEzw7nuB0t4OKlc7d7LZaVSNl5M2/pl15hXTJXA70kOjwOerawY+CPXL10/lrvixfL5uOHM82zDjoSGyPgL6m1pCTzrEedtV8dG0ANunHv2rnLl3v3aB+SdUCzPeBXkmqWU+IOe5yePfw4bhDy9/Zu7/SDWI/9JLVYYHlnsBaUYN8KjS78jdB0s0HIv3xOZ4BO3/QTa8YL4E/CcOMugae74Q5MABp0/hr4e/dr6m/e8vXEpjcFN69wyYRSdkRKXfm5VoOQf88P/ABxJDII/gSHpvvY2pXfGgkNIP+Fc+A/+b0f3iS58in4ZQld1wQX3R3vxwfQAJBfO2AdgPzU2RHkz0sKtH+yS7/4jf+wpn7sp0jDAeQXnXm5fOddrkt/4k0EdkDzBNoyVxOA/UTPWoF18KKiiLz7XbpLfwD7bzRP0IWd+wto++My3BPfv/N36e9/E4q0/HCFaIOvo9i/FPldcVGOO16983XpL37uxNx/kgrDOE5UptLFLmrOnHMLa+uerlrXdb9ute5blx/aAQJLV60DQjEIkYrGwgooBKwWqClDrGiRKEska7Zw9v/0fV88qxiXc/oe3ATl83ne57zvgfP+xV8hzXKXO4vWf/PGm7t33zSPD7MC+WpvZ0b9mdcg0dyNOL83rt8HPxYW3B+T857hT/+zCEQLVj5+hhPw5v6bUMwh9Pz6ZE6DwcnNn6z30zOP33slxwDwvT7mFzo/bZPgc/M/x83jWkPbs5vN+AIeGtdvFch/GnEaOvXc+s2CB/+C4dYreXPzjebQuP2jQP7QlLOzk7v+ZMGTx4FP4N9sbg6FJtwDjCA8E5gE38tdP7PwkQ2TH9qeyXELgfsqc58gftdYwNnp5K7/WfiIaP7paf7PiW+ez4L4H8fsTqee+/zKzscJ/o4GycEf/+buVgtZXQoF+F60/2GO00sFhz59eIU7Kzn4w+31Avj1CZndqZ9uz8Gc/NmnDdfQIcrv7md549n2do9d7zW14OvPWANWV84GhULytja5HHy3dpT/5EzIAuC3jI11j9ZvPZqdL8IK3qHzvX15o61tPDbs7u6x8V270XaZ3W53Tbm3vu2wHKsW5cxFn0altnx59iH2Y8CtUGh44TXaoNljt0cG630qq1V9ktSaIwt0r5WsWv02NPKDDKCb4bO0+hMKmccUGbBpNCqWPVWN7Z08A2AZhlX7PoeHsc0W/GUsiDemogqzRxYZ6rNqWEZ5e51InIc/e1sHdk2wY/HZgQH0R1NMoepT8QcKmSwyqrNiO4cxnqqbMUcsyp0DXyxMLwS+dw7wFdF+XV68TgG8GdW/teDOB+VvEc1B+Xly/tHr29jX0PRN6P2BfkWiJ5kHnwxGH7QrzJH3XR0qxsiwyk3oTn7+3Hof0wuB1TZsxwAUwXhKl6v4VDwBvCzitlG8WnXsshjdyb+Fu/6Jxnil16iyJP36AATaaHAo28W0bygR7yHVuzxJnxW9f221XhLXiQtuEe+1qY29vYyq4529mwjatfHEr6TlH7gl+Qt0LcGbI+90FhZ4i3VvXTW6D0Z+wc6rKgaTSNP1Y1rwoCcaD6ZGk10dGpWmoys5mgrGoz1aipdFhvssakaptlh2VgNfaIsYf5610/acJVsiX51+f8BDDdpgNB5NBEkS+C2opXTgWyJbLZiZ6ufWNUer6wriEQjOftFpWKVaF+t0+DGEfggA7OGipXTSG8z8AR2az2pUFI/u8NjaFs08/7XPqlSqnxrCDoc/kG4SFFwecHRZy1S9Ct1h2d3V6eaLeBgg2PtShxZ1hUfCYQwBTTIr2qFAwAac0oFvarWxSszNk3UEj+aI+WzOzxeJNt/B2ywxw8jISNogMyu4mAmc4lun3D4lc0V5SMSdWx4Ri+dglRzeceW2auJ7mAocWAseD4hpNEdvbRpzdxivbDoqkkrrSHf44iVSqVi6z3h7YrLTEUYg8NvtATjSIfSW1iZ8nLe+OHZJIimWSiUC+JKi4uLSYsmR3Wc+6fV+B0JGoMeHlClgMpko3AX6Q1fr6gtHikpLi4sJ/y/BjIL4hSUlpaVl+7eb9IjfT/FewGljKL3J5dq+v6y0pKRkIQRF4Iv5VC+WFBUV411lZfMWLSpbvP+EzAsD0IODssGWljQcbNeKxv1L8D/zyiAAnwyAb/montArF1dUVVSV72pcbfJ6wQcbceGnaVXjrvKqiorFSyohwAA4fsH8xa9csriqZlltbW35nl3nGhsaVq0gWdXQ0Hhu155yvL6sqiLNL/lffgURlC9durR86Z7lXPbgOV6pXVYDfkb9M3jzOUFVTc0yOBB4KBipBRx02p4/fAjEfPhUgBEQAxQVVAJNOjUgV5HeEzqpnuKL5mT0p+D8hIEocJorK5fAQwIqgmd4bREpHXSC58rndQIgIAaioBJoEKhw4IGAS8kUXorqeePpCkgLiAISWLJm4UKCJmtXCJ4TwEAcnAWBavqgoWDKJnQOz98ABZVwnowUETBlS4TSwacGOCDhNBmRAEzIYGfQhThgwYM7JDjATD+jaCRj0gt04MiXguDfMUsH0HWLB+sAAAAASUVORK5CYII=) no-repeat;
        background-size: cover;
        display: inline-block;
        width: 50px;
        height: 50px;
        text-align: center;
        line-height: 50px;
    }
    
    .current-info .current-info-inner .open-num .open-num-list .open-ball .number.grayBg[data-v-7bf92f17] {
        background: url(data:image/png;base64,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) no-repeat;
         background-size: cover;
        display: inline-block;
        width: 50px;
        height: 50px;
        text-align: center;
        line-height: 50px;
    }
    
    .current-info .current-info-inner .open-num .open-num-list .open-ball .sheng-xiao[data-v-7bf92f17] {
        font-size: 24px;
        line-height: 50px;
        color: #000;
        display: block;
        width: 50px;
        height: 50px;
        text-align: center;
         
    }
    .open-num-list li{
        width: 50px;
        height: 100px;
        list-style: none;
        display: flex;
        flex-wrap: wrap;
    }
    .open-num-list{
        padding: 0;
    }
    .close{
        position:absolute;
        right: -10px;
        top: -10px;
        width: 20px;
        height: 20px;
        border: 1px solid #333;
        border-radius: 20px;
        text-align: center;
        line-height: 20px;
        background-color: #333;
        color: #fff;
        cursor: pointer;
        user-select: none;
    }
</style>