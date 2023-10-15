<template>
    <h5>မေးခွန်းကို အာရုံစူးစိုက်ပါ။ မေးခွန်း၏အဖြေကို စိတ်ဖြင့်တောင်းတပါ။ မျက်စိမှိတ်ထားပါ။</h5>
    <div class="table">
        <div class="number" v-for="number in numbers" @click="clickAnswer(number)">
            {{ number }}
        </div>
    </div>
</template>

<script>

export default {
    data() {
        return {
            numbers: [],
            apiQuestions: "http://localhost:3000/numberList",
        }
    },
    mounted() {
        fetch(this.apiQuestions)
            .then((response) => {
                return response.json()
            })
            .then((datas) => {
                this.numbers = datas
            })
            .catch((err) => {
                console.log(err)
            })
    },
    methods: {
        clickAnswer(AnswerNumber) {
            var nums = { '၀': '0', '၁': 1, '၂': 2, '၃': 3, '၄': 4, '၅': 5, '၆': 6, '၇': 7, '၈': 8, '၉': 9 };
            let engAnswerNumber = AnswerNumber.replace(/([၀-၉])/g, function (s, key) {
                return nums[key] || s;
            });
            this.$emit("clicked",engAnswerNumber)
        },
    }
}
</script>

<style>
.number {
    display: inline-grid;
    justify-content: center;
    align-items: center;
    padding: auto;
    background-color: #f2f2f2;
    color: #666666;
    text-align: center;
    font-weight: bold;
    font-size: larger;
}

.number:hover {
    cursor: pointer;
    background-color: #666666;
    color: #f2f2f2;
}

.table {
    display: grid;
    grid-template-columns: auto auto auto auto auto auto auto auto auto;
    height: 500px;
}
</style>