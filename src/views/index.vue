<style scoped>
    .index {
        width: 100%;
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        text-align: center;
    }

    .index h1 {
        font-size: 50px
    }

    .index h2 {
        color: #666;
        margin-bottom: 20px;
    }
    .textList {
        display: block;
        width: 40%;
        margin: 0 auto;
        height: 30px;
        font-size: 20px;
    }
    .textList li {
        margin-top: 10px;
        display: block;
        text-align: left;
    }
    .ivu-checkbox-wrapper {
        float: left;
    }
</style>
<template>
    <div class="index">
        <Row type="flex" justify="center" align="top">
            <Col span="24">
                <h1>
                    toDoList
                </h1>
                <h2>
                    <p>快计划你的行程吧！</p>
                </h2>
                    <Input 
                    v-model="inputValue" 
                    icon="close" 
                    placeholder="请输入你的行程计划..." 
                    style="width: 300px;margin-bottom: 20px"
                     @on-keyup.enter="add"
                     @on-click="deleteIcon"
                     ></Input>
                    <ul class="textList">
                        <li v-for="item in items" >
                            <div>
                                <Checkbox v-model="item.single"></Checkbox>
                                <label><del v-show=item.single>{{item.text}}</del><span v-show=!item.single>{{item.text}}</span></label>
                                <ButtonGroup style="float: right">
                                    <Button type="primary" @click="updateItem(item)" :disabled="item.single">修改</Button>
                                    <Button type="error" @click="deleteItem(item)" :disabled="item.single">删除</Button>
                                </ButtonGroup>
                            </div>
                        </li>
                    </ul>
            </Col>
        </Row>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                inputValue:'',
                items: this.fetch(),
                value: '',
            }
        },
        methods: {
            add() {
                if(this.inputValue === ''){
                    return
                }
                this.items.unshift({text: this.inputValue,single: false});
                this.inputValue="";
            },
            deleteIcon() {
                this.inputValue="";
            },
            deleteItem(item) {
                this.items.splice(this.items.indexOf(item), 1)
            },
            updateItem(item) {
                this.$Modal.confirm({
                    render: (h) => {
                        return h('Input', {
                            props: {
                                value: this.value,
                                autofocus: true,
                                placeholder: '活动日程修改为...'
                            },
                            on: {
                                input: (val) => {
                                    this.value = val;
                                }
                            }
                        })
                    },
                     onOk:()=>{
                                    if(this.value === '') {
                                        return
                                    }
                                    this.items[this.items.indexOf(item)].text = this.value;
                            }
                })
            },
            fetch () {
                var todos = JSON.parse(localStorage.getItem("todo") || '[]')
                return todos
            },
           },
           watch:{
                items:{
                    handler:function(items){
                         localStorage.setItem('todo', JSON.stringify(items) )
                    },
                    deep:true
                }
            }
    };
</script>     
