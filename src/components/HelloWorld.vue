<template>
    <div id="Demo">
        <el-form ref="form" label-width="100px">
            <el-form-item>
                <div class="check-group" v-for="(item, index) in JdInfo" :key="index">
                    <el-tag @click="handleClick">{{item.name}}</el-tag>
                    <el-radio-group
                        v-model="checkedEquipments[index]"
                        @change="handleChange(item.id,$event)"
                    >
                        {{checkedEquipments[index]}}
                        <el-radio
                            v-for="(data,id) in item.address"
                            :label="data.address"
                            :key="id"
                        >{{data.address}}</el-radio>
                    </el-radio-group>
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
                checkedEquipments: [], //随意修改后的checked项（即要传到后台的变更数据）
                equipments: [
                    // 所有数据
                    {
                        id: "1",
                        menu: "设备1",
                        childMenu: [
                            {
                                id: "1-1",
                                menu: "暖通一"
                            },
                            {
                                id: "1-2",
                                menu: "照明一"
                            },
                            {
                                id: "1-3",
                                menu: "取暖一"
                            },
                            {
                                id: "1-4",
                                menu: "应急一"
                            }
                        ]
                    },
                    {
                        id: "2",
                        menu: "设备2",
                        childMenu: [
                            {
                                id: "2-1",
                                menu: "暖通二"
                            },
                            {
                                id: "2-2",
                                menu: "照明二"
                            },
                            {
                                id: "2-3",
                                menu: "取暖二"
                            },
                            {
                                id: "2-4",
                                menu: "应急二"
                            }
                        ]
                    }
                ],
                JdInfo:[]
            };
        },
        methods: {
            handleChange(id,event) {
                console.log(id,event);
                
                this.checkEquipArr = [];
                let arr = this.checkedEquipments;
                for (let i = 0; i < arr.length; i++) {
                    let equipment = arr[i];
                    if (equipment.length > 0) {
                        let obj = {
                            id: this.JdInfo[i].id,
                            equips: equipment
                        };
                        console.log(this.equipments[i].id);
                        this.checkEquipArr.push(obj);
                    }
                }
                console.log(this.checkEquipArr);
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


