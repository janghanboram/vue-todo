<template>
    <section>
        <transition-group name="list" tag="ul">
            <li v-for="(todoItem,index) in propsdata" :key="todoItem" 
                class="shadow">
                <i class="checkBtn fa fa-check" aria-hidden="true"></i>
                {{ todoItem }}
                <span class="editBtn" type="button" @click="showEditModal(todoItem,index)">
                    <i class="fa fa-edit" aria-hidden="true"></i>
                </span>
                <span class="removeBtn" type="button" @click="removeTodo(todoItem,index)">
                    <i class="fa fa-trash-o" aria-hidden="true"></i>
                </span>
            </li>
        </transition-group>
        <modal v-if="showModal" @close="showModal = false">
            <h3 slot="header">수정</h3>
            <span slot="body" class="inputBox">
                <input type="text"
                v-bind:placeholder="editData.current" v-model="editData.new">
            </span>
            <button slot="footer" @click="initData" class="btnModal btnCancel">
                취소
            </button>
            <button slot="footer" @click="editTodo" class="btnModal btnConfirm">
                확인
            </button>
    </modal>          
    </section>
</template>

<script>
    import Modal from './common/Modal.vue';

    export default{
        props:['propsdata'],
        data(){
            return {
                showModal: false,
                editData:{
                    current: '',
                    new: '',
                    index: 0
                }
            }
        },
        methods:{
            removeTodo(todoItem, index){
                this.$emit('removeTodo',todoItem,index);
            },
            initData(){
                this.showModal = false;                
                this.editData.current='';
                this.editData.new='';
                this.editData.index=0;
            },
            editTodo(){
                this.$emit('editTodo',this.editData);
                this.initData();
            },
            showEditModal(todoItem, index){
                this.showModal = true;
                this.editData.current = todoItem;
                this.editData.index = index;
            },
        },
        components:{
            modal:Modal
        }
    }
</script>

<style scoped>
    .list-item{
        display: inline-block;
        margin-right: 10px;
    }
    .list-move{
        transition: transform 1s;
    }
    .list-enter-active, .list-leave-active{
        transition: all 1s;
    }
    .list-enter, .list-leave-to{
        opacity: 0;
        transform: translateY(30px);
    }
    ul{
        list-style-type: none;
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
        max-height: 430px;
        overflow-y: scroll;
    }

    li{
        display: flex;
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
    }

    .checkBtn{
        line-height: 45px;
        color:#62acde;
        margin-right: 5px;
    }
    .editBtn{
        margin-left: .6rem;
        color:#42b983;
    }
    .removeBtn{
        margin-left: auto;
        color:#de4343;
    }
    input:focus{
        outline:none;
    }
    .inputBox{
        background: white;
        height: 50px;
        line-height: 50px;
        border-radius: 5px;
    }
    .inputBox input{
        border: 0;
        padding: 0.5rem;
        border-radius: 5px;
        border-bottom:solid 2px gray;
        font-size:1.2rem;
    }
    .btnModal{
        padding: 0.5rem 2rem;
        margin: 0 1rem;
        background-color: white;
        font-size: .8rem;
    }
    .btnCancel{
        color: #de4343;
        border: solid 2px #de4343;
    }
    .btnConfirm{
        color: #42b983;
        border: solid 2px #42b983;        
    }
</style>
