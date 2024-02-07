<template >
    <transition name="modalTransition" class="modalPosition">
        <div v-show="modalActive">
            <transition>
                <div class="modal text-green-700">
                    <h1>{{ message }}</h1>

                </div>

            </transition>
        </div>
    </transition>
</template>
<script setup>
const props = defineProps({
    modalActive: {
        type: Boolean,
        default: false
    },
    message: {
        type: String,
        default: ''
    }
});





</script>
<style scoped>

.modal{
    @apply h-20 w-60  bg-gray-200  flex border rounded-md font-bold justify-center pt-[10%] text-xl


}

.modalPosition{
    @apply flex justify-center absolute top-[15%] left-[42%];
    transition: transform 1000ms;
 
}



</style>