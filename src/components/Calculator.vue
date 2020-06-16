<template>
    <div class="calculator">
        <div class="display">{{current || '0'}}</div>
        <div class="btn" @click="clear">AC</div>
        <div class="btn" @click="sign">+/-</div>
        <div class="btn" @click="percent">%</div>
        <div class="btn operator" @click="divide">/</div>
        <div class="btn" @click="append('7')">7</div>
        <div class="btn" @click="append('8')">8</div>
        <div class="btn" @click="append('9')">9</div>
        <div class="btn operator" @click="times">X</div>
        <div class="btn" @click="append('4')">4</div>
        <div class="btn" @click="append('5')">5</div>
        <div class="btn" @click="append('6')">6</div>
        <div class="btn operator" @click="minus">-</div>
        <div class="btn" @click="append('1')">1</div>
        <div class="btn" @click="append('2')">2</div>
        <div class="btn" @click="append('3')">3</div>
        <div class="btn operator" @click="add">+</div>
        <div class="btn zero" @click="append('0')">0</div>
        <div class="btn" @click="dot">.</div>
        <div class="btn operator" @click="equal">=</div>
        <div v-if="statistic" id="current">current={{current}}</div>
        <div v-if="statistic" id="previous">previous={{previous}}</div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            previous: null,
            current: '',
            operator: null,
            operatorClicked: false,
            temp: null,
            flag:false,
            flag2:false,
            statistic: false,

        }
    },
    created() {
        var keys=['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
        window.addEventListener('keydown', (e) => {
            console.log(e.key);
            if(keys.indexOf(e.key)>=0){
                this.append(keys[keys.indexOf(e.key)]);
            } else if(e.key == '.'){
                this.dot();
            } else if(e.key == 'Backspace'){
                this.current = this.current.slice(0, -1);
            } else if(e.key == 'Escape'){
                this.clear();
            } else if(e.key == '+'){
                this.add();
            } else if(e.key == '-'){
                this.minus();
            } else if(e.key == '*'){
                this.times();
            } else if(e.key == '/'){
                this.divide();
            } else if(e.key == '%'){
                this.percent();
            } else if(e.key == '='){
                this.equal();
            } else{
                this.append(undefined);
            }
        });
    },
    methods: {
        clear() {
            this.current = '';
        },
        sign() {
            if(this.current || number !== '0'){
                this.current = this.current.charAt(0) === '-' ?
                    this.current.slice(1) : `-${this.current}`;
            }
        },
        percent () {
            if(this.current){
                this.current = `${parseFloat(this.current) / 100}`;
            }
        },
        setPrevious () {
            this.previous = this.current;
            this.operatorClicked = true;
            this.flag = true;
        },
        divide () {
            this.operator = (a, b) => a / b;
            this.setPrevious();
        },
        times () {
            this.operator = (a, b) => a * b;
            this.setPrevious();
        },
        minus () {
            this.operator = (a, b) => a - b;
            this.setPrevious();
        },
        add () {
            this.operator = (a, b) => a + b;
            this.setPrevious();
        },
        equal () {
            if(this.previous){
                if(this.flag){
                    this.temp = this.current;
                    this.flag = false;
                }
                if(!this.operatorClicked){
                    this.current = `${this.operator(
                        parseFloat(this.previous),
                        parseFloat(this.temp))}`;
                    this.previous = this.current
                }else{
                    this.current = `${this.operator(
                        parseFloat(this.previous),
                        parseFloat(this.current))}`;
                }
            }
            this.flag2 = true;
        },
        append (number) {
            if(number !== undefined && (this.current || number !== '0') ){
                if (this.operatorClicked || this.flag2) {
                    this.current = '';
                    this.operatorClicked = false;
                    this.flag2 = false;
                }
                if(this.current[0] !== '0' || this.current.indexOf('.') === 1){
                    this.current = `${this.current}${number}`;
                }else{
                    this.current = `${number}`;
                }
            }
        },
        dot () {
            if(!this.current){
                this.current = '0.';
            }else if (this.current.indexOf('.') === -1 || this.current[0] === '0') {
                this.current = `${this.current}${'.'}`;
            }
        }

    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .calculator {
        display: grid;
        font-size: 40px;
        grid-template-columns: repeat(4,1fr);
        grid-auto-rows: minmax(80px,auto);
        width: 400px;
        margin: 0;
        position: absolute;
        top: 50%;
        left: 50%;
        margin-right: -50%;
        transform: translate(-50%, -50%);
    }
    .display {
        /*start at column 1 and end at column 5*/
        grid-column: 1 / 5;
        background-color: #333;
        color: white;
        padding: 20px 20px;
    }
    .zero {
        /*start at column 1 and end at column 3*/
        grid-column: 1 / 3;
    }
    .btn {
        background-color: #eee;
        border: 1px solid grey;
        padding: 20px 20px;
    }
    .operator {
        background-color: orange;
        color: white;
    }
    .btn:hover{
        background-color: #333;
        color: white !important;
    }
    .btn:active {
        background-color: #3e8e41;
        transform: translateX(1px) translateY(1px);
    }
    #current {
        grid-column: 1/3;
        font-size: 20px;
        text-align: left;
    }
    #previous {
        grid-column: 3/5;
        font-size: 20px;
        text-align: left;
    }

</style>
