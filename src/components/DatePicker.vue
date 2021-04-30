<template>
    <div>
        <input class="dp-input" type="text" v-model="dateValue" @click="openModal=!openModal" placeholder="Due Date" readonly>
        <div class="dp-container" v-show="openModal">
            
            <div class="dp-month-header">
                <div class="dp-change-month" @click="changeMonth(-1)">&#10094;</div>
                <div class="dp-month-display">{{current.getMonth()+1}}, {{current.getFullYear()}}</div>
                <div class="dp-change-month" @click="changeMonth(1)">&#10095;</div>
            </div>
            <div class="dp-weeks-container">
                <div class="dp-weekdays">Su</div>
                <div class="dp-weekdays">Mo</div>
                <div class="dp-weekdays">Tu</div>
                <div class="dp-weekdays">We</div>
                <div class="dp-weekdays">Th</div>
                <div class="dp-weekdays">Fr</div>
                <div class="dp-weekdays">Sa</div>
                <div class="dp-day-disable" v-for="prevDay in prevWeekDays" :key="prevDay">{{prevDay}}</div>
                <div class="dp-day" :class={active:calenderday.isActive} v-for="(calenderday,index) in calender" :key="index" @click="setValue(index);">{{calenderday.day}}</div>
            </div>
            <div class="dp-footer">
                <div class="dp-done_button" @click="openModal=false">Done</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
            modelValue:{type:String,
                        default: new Date().toLocaleDateString()}
                        },
    emits: ['update:modelValue'],
    data(){
        return{
            current : new Date(),
            calender: [],
            prevWeekDays:[],
            isActive:false,
            openModal:true,
        }
    },
    created(){
        this.dateValue = new Date().toLocaleDateString();
        setTimeout(this.renderCalender(),0);
        
    },
    methods:{
        renderCalender(){

            const FirstWeekDay = new Date(this.current.getFullYear(), this.current.getMonth(), 1).getDay();
            const PrevMonthDay = new Date(this.current.getFullYear(), this.current.getMonth(),0).getDate();
            const MonthDay = new Date(this.current.getFullYear(),this.current.getMonth()+1,0).getDate();
        
            for(let prevDays=FirstWeekDay; prevDays>0; prevDays--){
                this.prevWeekDays.push(PrevMonthDay-prevDays+1)
            }

            for(let x=1; x<=MonthDay; x++){
                this.calender.push({'day':x,isActive:false})
            }
        },
        changeMonth(value){
            this.current.setMonth(this.current.getMonth() + value);
            this.prevWeekDays=[];
            this.calender=[];
            this.renderCalender();
        },
        setValue(index){
            this.calender.forEach(item=> item.isActive = false);
            this.calender[index].isActive = true;
            this.dateValue= new Date(this.current.setDate(index+1)).toLocaleDateString();
        },
        setToday(){
            this.dateValue = new Date().toLocaleDateString();
        },
        
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
        text-align: center;
        color:inherit;
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