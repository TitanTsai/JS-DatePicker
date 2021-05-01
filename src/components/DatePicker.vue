<template>
    <div>
        <input class="dp-input" type="text" v-model="dateValue" @click="openModal=!openModal" placeholder="Due Date" readonly>
        <div class="dp-container">
            
            <div class="dp-month-header">
                <div class="dp-change-month">&#10094;</div>
                <div class="dp-month-display"></div>
                <div class="dp-change-month">&#10095;</div>
            </div>
            <div class="dp-weeks-container">
                <div v-for="name in weekName" :key="name">{{name}}</div>
                <div v-for="prevday in prevWeekDays" :key="prevday">{{prevday}}</div>
                <div v-for="day in calender" :key="day">{{day}}</div>
            </div>
            <div class="dp-footer">
                <div class="dp-done_button" @click="openModal=false">Done</div>
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
            current: null,
            weekName:['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa'],
            isActive:false,
            openModal:true,
            calender: [],
            prevWeekDays:[]
        }
    },
    created(){
        this.renderCalender()
    },
    methods:{
        renderCalender(){
            let selected = new Date();
            let daysofMonth = new Date(selected.getFullYear(),selected.getMonth() +1,0).getDate();//取得該月份天數,月份0-11
            let firstWeekDay = new Date(`${selected.getFullYear()}-${selected.getMonth() +1 }-01`).getDay()
            let prevMonthDay = new Date(selected.getFullYear(), selected.getMonth(),0).getDate()//取得上個月天數

            for(let prev=firstWeekDay;prev>0;prev--){
                this.prevWeekDays.push(prevMonthDay-prev+1)
            }

            for(let i=1; i<= daysofMonth; i++){
                this.calender.push(i)
            }
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
        }
    }
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
        height:23em;
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

    .dp-change-month, .dp-day{
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

    .dp-weekdays{
        font-size:18px;
        color:var(--caption);
        font-weight: 500;
    }

    .dp-day{
        color:var(--caption);
        font-size:18px;
        font-weight:500;
        margin-bottom:3px;
    }

    .dp-day-disable{
        color:var(--disabled);
        font-size:18px;
        font-weight:300;
        margin-bottom:3px;
        width:36px;
        height:36px;
        display:flex;
        align-items: center;
        justify-content: center;
    }

    .dp-day:hover{
        background-color:var(--primary);
        color:var(--white)
    }

    .active{
        background-color:var(--primary);
        color:var(--white)
    }

    .dp-footer{
        bottom:0;
        left:0;
        padding:1em 0;
        width:100%;
    }

    .dp-done_button{
        align-self: center;
        display:inline-flex;
        background-color:var(--primary);
        color:var(--white);
        padding:8px 16px;
        position: absolute;
        bottom:1em;
        left:50%;
        transform: translateX(-50%);
        border-radius: 8px;
        font-weight:500;
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