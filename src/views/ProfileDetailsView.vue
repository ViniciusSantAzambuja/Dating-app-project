<script setup>
import ButtonComponent from '@/components/ui-components/ButtonComponent.vue';
import { ref } from 'vue';
import { Calendar, CameraFilled, DArrowLeft, ArrowLeftBold, ArrowRightBold, DArrowRight } from "@element-plus/icons-vue"
import {useUserStore} from "@/stores/user"
import { useRouter } from 'vue-router';

const store =  useUserStore();
const router = useRouter();

const calendar = ref('');
const isCalendarOpen = ref(false);

const selectDate = (val) => {
    calendar.value.selectDate(val)
}

const formatDate = (date) => {
    return `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}`
}

// todo - salvar essas informações no backenderson quando estiver pronto, também ver como salvar a porcaria da foto
const onConfirm = () => {
    if(store.firstName.length === 0 || store.lastName.length ===0 ){
        alert(`Por favor, insira uma nome e sobrenome válidos`)
        return
    }else if(!store.bdayDate){
        alert("Por favor, Selecione sua data de nascimento");
        return
    }

    const formattedBdayDate = formatDate(store.bdayDate)
    alert(`${store.firstName} - ${store.lastName} - ${formattedBdayDate}`)
    router.push('/i-am')
}


</script>

<template> 
<main>
    <form class="profile-detail-container" :class="{unfocused: isCalendarOpen}">
            <h2>Profile Details</h2>
            <div class="user-profile-picture">
                <img class="user-image"  src="../assets/onboard-image-03.png" alt="foto">
                <label for="user-image"><el-icon><CameraFilled /></el-icon></label>
                <input type="file" name="user-image" id="user-image">
            </div>
            <div class="profile-info" >
                <label for="first-name">First name</label>
                <input type="text" name="first-name" id="first-name" v-model="store.firstName" required>

                <label for="last-name">Last name</label>
                <input type="text" name="last-name" id="last-name" v-model="store.lastName"  required>

                <div class="bday-btn" @click="isCalendarOpen = true" >
                    <el-icon :size="30"><Calendar /></el-icon> 
                    <span>Choose birthday date</span> 
                </div>

            </div>
            <div class="calendar-container" v-if="isCalendarOpen">
                        <el-calendar ref="calendar" v-model="store.bdayDate" >
                            <template #header="{ date }">
                                <span>Birthday</span>
                            <el-button-group>
                                <el-button size="small" @click="selectDate('prev-year')">
                                    <el-icon><DArrowLeft /></el-icon>
                                </el-button>
                                <el-button size="small" @click="selectDate('prev-month')">
                                    <el-icon><ArrowLeftBold /></el-icon>
                                </el-button>
                                <el-button size="small" class="bday-date-show" disabled><span>{{ date }}</span></el-button>
                                <el-button size="small" @click="selectDate('next-month')">
                                    <el-icon><ArrowRightBold /></el-icon>
                                </el-button>
                                <el-button size="small" @click="selectDate('next-year')">
                                    <el-icon><DArrowRight /></el-icon>
                                </el-button>
                            </el-button-group>
                            </template>
                        </el-calendar>

                        <ButtonComponent title="Save" @click="isCalendarOpen = false"/>
                    </div>
            <ButtonComponent title="Confirm" @click="onConfirm()" />
    </form>
</main>
</template>

<style >
.profile-detail-container{
    display: flex;
    flex-flow: column wrap;
    align-items: center;
    justify-content: space-around;
    height: 100vh;
    background-color: #FFFFFF;
    padding: 12px;
}

.profile-detail-container h2{
    font-weight: 700;
    font-size: 36px;
    text-align: left;
    color:#000000;
    margin-bottom: -24px;
}

.user-profile-picture{
    display: flex;
    flex-flow: column wrap;
    align-items: center;
    justify-content: center;
    margin-bottom: -100px;
}

.user-profile-picture .user-image{
    height: 120px;
    width: 120px;
    border-radius: 24px
}

.user-profile-picture input{
    display: none
}

.user-profile-picture label{
    position: relative;
    bottom: 18px;
    left: 50px;
    background-color: #E94057;
    border: 1px solid #FFFFFF;
    border-radius: 50%;
    width: 33px;
    width: 33px;
}

.user-profile-picture label > .el-icon{
    position: relative;
    left: 8px;
    top: 1px;
    color: #FFFFFF;
}

.profile-info{
    display: flex;
    align-items: center;
    width: 100%;
    flex-flow: column wrap;
}

.profile-info  label{
    color:#00000066;
    font-size: 14px;
    font-weight: 400;
    background-color: #FFFFFF;
    position: relative;
    top: 10px;
    right: 88px;
    width: 78px;
    text-align: center;
}

.profile-info input{
    border: 1px solid #00000066;
    border-radius: 14px;
    height: 60px;
    width: 75%;
    padding: 5px 10px 5px 10px;
    outline: none;
    font-size: 14px;
    font-weight: 400;
}

.bday-btn{
    display: flex;
    flex-flow: row wrap;
    align-items: center;
    justify-content: center;
    gap: 12px;
    background-color: #ffc2ca;
    width: 75%;
    border: 1px solid #ffc2ca;
    border-radius: 10px;
    height: 60px;
    margin-top: 22px;
    padding: 10px;
    color: #E94057;
}

.bday-btn span{
    font-weight: 700;
    font-size: 14px;
}

.calendar-container{
    display: flex;
    flex-flow: column wrap;
    align-items: center;
    position: fixed;
    bottom: 0px;
    background: #FFFFFF;
    padding: 8px;
    border-top-left-radius: 26px;
    border-top-right-radius: 26px;
}

.calendar-container .el-calendar{
    color: #E94057;
}

.el-calendar__header{
    display: flex;
    align-items: center;
    flex-flow: column wrap;
    justify-content: center;
    padding: 12px 20px;
}   

.el-calendar__header > span {
    color: #000000;
}

.el-button{
    border: none;
}

.el-button:hover{
  border: 1px solid #E94057;
  border-radius: 50%;
  background-color: #E94057;
  color: #FFFFFF;
  transition: all ease-in-out 180ms;
}

.bday-date-show, .bday-date-show.is-disabled{
    color: #E94057; 
    font-size: 20px;
}

.bday-date-show:hover{
    border: none;
    background: #FFFFFF;
}

.unfocused{
    background: #555555;
}

</style>