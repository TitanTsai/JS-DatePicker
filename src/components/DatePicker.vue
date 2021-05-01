<template>
    <div>
        <input class="dp-input" type="text" v-model="dateValue" @click="showModal=!showModal;" placeholder="Due Date" readonly>
        
        <div class="dp-container" v-if="showModal">
            <div class="dp-month-header">
                <div class="dp-change-month" @click="changeMonth(-1)">&#10094;</div>
                <div class="dp-month-display">{{this.monthName[this.selected.getMonth()]}}, {{this.selected.getFullYear()}}</div>
                <div class="dp-change-month" @click="changeMonth(1)">&#10095;</div>
            </div>
            <div class="dp-weeks-container">
                <div v-for="name in weekName"  class="dp-weekday" :key="name">{{name}}</div>
                <div v-for="prevday in prevWeekDays" :key="prevday"></div>
                <div v-for="(day,index) in calender" class="dp-day" :class="{active:day.isActive}" :key="day" @click="selectDate(index)">{{day.num}}</div>
            </div>
            <div class="dp-footer">
                <div class="dp-done_button" @click="showModal=false">Done</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['modelValue'],
    emits: ['update:modelValue'],
    data(){
        return{
            selected: new Date(),
            weekName:['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
            monthName:['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'],
            showModal:false,
            calender: [],
            prevWeekDays:[],
        }
    },
    created(){
        this.renderCalender(),
        this.calender[this.selected.getDate()-1].isActive = true
    },
    methods:{
        renderCalender(){
            let daysofMonth = new Date(this.selected.getFullYear(),this.selected.getMonth() +1,0).getDate();//取得該月份天數,月份0-11
            let firstWeekDay = new Date(`${this.selected.getFullYear()}-${this.selected.getMonth() +1 }-01`).getDay()
            // let prevMonthDay = new Date(this.selected.getFullYear(), this.selected.getMonth(),0).getDate()//取得上個月天數

            this.prevWeekDays.length = firstWeekDay

            for(let day=1; day<= daysofMonth; day++){
                this.calender.push({'num':day,isActive:false,isToday:false})
            }
        },
        changeMonth(value){
            this.selected.setMonth(this.selected.getMonth() + value);
            this.prevWeekDays=[];
            this.calender=[];
            this.renderCalender();
        },
        selectDate(value){
            this.calender.forEach(item=> item.isActive=false)
            this.calender[value].isActive = true;
            this.dateValue =new Date(this.selected.setDate(value+1)).toLocaleDateString()
        }
    },
    computed: {
        dateValue: {
            get() {
                return this.modelValue
            },
            set(value) {
                this.$emit('update:modelValue', value)
            }
        },
    },

    }
    
</script>

<style>
    .dp-input{
        width:340px;
        font-size:1em;
        border:1px solid #707070;
        border-radius: 4px;
        padding:10px;
        background: url('../assets/images/calender.svg') no-repeat 95% 10px;
        cursor: pointer;
    }

    .dp-container{
        margin:1em auto;
        display:block;
        width:20em;
        background-color:var(--background);
        border-radius: 0.5em;
        box-shadow: 0 3px 6px rgba(0,0,0,0.16);
        padding:1em;
        position:relative;
        z-index: 1000;
    }

    .dp-instant-container{
        display:flex;
        justify-content: flex-start;
    }

    .dp-month-header{
        display:flex;
        justify-content: space-between;
        margin:1em 0;
    }

    .dp-month-display{
        color:var(--caption);
        font-size:20px;
        font-weight: 500;
        
    }

    .dp-change-month{
        cursor:pointer;
        width:36px;
        height:36px;
        border-radius: 0.5em;
        display:flex;
        align-items: center;
        justify-content: center;
    }
    
    .dp-change-month{
        box-shadow:0 1px 3px rgba(0,0,0,0.16)
    }

    .dp-change-month:hover{
        background-color:var(--primary);
        color:var(--white);
    }

    .dp-weeks-container{
        display: grid;
        grid-template-columns: repeat(7,1fr);
        text-align:center;
    }

    .dp-day, .dp-weekday{
        width:36px;
        height:36px;
        font-size: 18px;
        font-weight: 500;
        margin-bottom:6px;
        border-radius: 8px;
        justify-self: center;
        line-height: 36px;
    }
    
    .dp-day:hover{
        background-color:var(--hover);
    }

    .today{
        border:2px solid var(--primary);
    }

    .active{
        background-color:var(--primary);
        color:var(--white)
    }

    .active:hover{
        background-color:var(--primary);
    }

    .dp-footer{
        margin-top:1em;
    }

    .dp-done_button{
        display:block;
        width:100%;
        background-color:var(--primary);
        color:var(--white);
        font-size:18px;
        font-weight:500;
        border-radius:8px;
        padding:8px 0;
        cursor: pointer;
    }

    @media screen and (max-width:768px) {
        .dp-input{
            width:240px;
            font-size:1em;
            background: url('../assets/images/calender.svg') no-repeat 210px 10px var(--background);
        }
    }
</style>