<template>
    <div class="container">
        <h2>Vue custom File Input by <span>Vinokurov Danil</span></h2>
        <AppFileInput
            @filesUploaded="shiftUploadedFiles"
            class="main_file_input"
        />
    </div>
</template>

<script setup lang="ts">
import AppFileInput from '~/components/app-file-input.vue';

//!methods

const shiftUploadedFiles = (list: File[]) => {
    console.log(list);
};
</script>

<style lang="scss">
@import url('~/styles/index.scss');

h2 {
    text-align: center;
    font-size: 16px;
    font-weight: 600;
    padding-top: 40px;
    span {
        color: $accent-color;
        font-size: 14px;
    }
}

.main_file_input {
    max-width: 400px;
    margin: 40px auto;
}
</style>
