<template>
    <div class="question" @click="clickQuestion()">
        <div class="flexing">
            <div class="no">{{question.questionNo}}</div>
            <div>{{ question.questionName }}</div>
        </div>
    </div>
</template>
  
<script>
export default {
    props:['question'],
    methods:{
        clickQuestion(){
            this.$emit("clicked",this.question)
        },
    },
}
</script>

<style>
.question{
    padding: 20px;
    background-color: #f2f2f2;
    color: #666666;
    margin: 10px;
    border-radius: 8px;
    font-weight: bold;
}
.question:hover{
    cursor: pointer;
    background-color: #666666;
    color: #f2f2f2;
}
.flexing{
    display: flex;
    justify-content: left;
    align-items: center;
}
.no{
    padding-right:20px;
}
</style>