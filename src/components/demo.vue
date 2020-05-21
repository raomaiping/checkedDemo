<template>
    <div id="Demo">
        <el-form ref="form" label-width="100px">
            <el-form-item>
                <div class="check-group" v-for="(item, index) in JdInfo" :key="index">
                    <el-checkbox @change="handleA(item.id,$event)">{{item.name}}</el-checkbox>
                    <el-checkbox
                        v-show="showId.indexOf(item.id) > -1"
                        @change="handleChange(item.id,data.address,data.id,$event)"
                        v-for="(data,id) in item.address"
                        :label="data.address"
                        :key="id"
                    >{{data.address}}</el-checkbox>
                </div>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
    import axios from "axios";
    export default {
        data() {
            return {
                arrs: [],
                JdInfo: [],
                showId: []
            };
        },
        methods: {
            handleA(id, event) {
                if (event) {
                    this.showId.push(id);
                } else {
                    let index = this.showId.indexOf(id);
                    this.showId.splice(index, 1);
                }
            },
            handleChange(id, address, addressId, event) {
                if (event) {
                    let obj = {
                        id,
                        address,
                        addressId,
                        isChecked:event
                    };
                    this.arrs.push(obj)
                }else{
                    let i ;
                    this.arrs.map((item,index) =>{
                         if(item.id === id){
                             i = index
                         }
                         return item
                    })
                    this.arrs.splice(i, 1);
                }
                // if (this.arrs.indexOf(id) === -1) {
                //     this.arrs.push(obj);
                // } else {
                //     let index = this.arrs.indexOf(id);
                //     this.arrs[index] = obj;
                // }
                console.log(
                    this.arrs,
                    "----------------------",
                    id,
                    address,
                    addressId,
                    event
                );
            }
        },
        created() {
            axios
                .get("http://rap2.taobao.org:38080/app/mock/247162/demo")
                .then(res => {
                    let adddressInfo = res.data.adddressInfo;
                    let customerCode = res.data.customerCode;
                    let arr = [];
                    for (let prop in customerCode) {
                        let obj = {};
                        obj.name = prop;
                        obj.id = customerCode[prop];
                        obj.address = adddressInfo[customerCode[prop]];
                        arr.push(obj);
                    }
                    this.JdInfo = arr;
                    console.log(this.JdInfo);
                });
            // 初始化默认选中状态
        }
    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
</style>


