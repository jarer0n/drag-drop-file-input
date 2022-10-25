<template>
    <div class="container">
        <h2>Vue custom File Input by <span>Vinokurov Danil</span></h2>
        <div class="app_link">
            <a
                href="https://github.com/jarer0n/drag-drop-file-input"
                target="_blank"
            >
                GitHub
            </a>
        </div>
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
    margin-top: 40px;
    span {
        color: $accent-color;
        font-size: 14px;
    }
}

.app_link {
    display: flex;
    justify-content: center;
    margin: 40px 0px;
    a {
        text-align: center;
        color: $main-text-color;
        transition: all 0.2s ease;
        display: inline-block;
        padding: 3px 8px;

        border: 1px solid $main-text-color;
        &:hover {
            border: 1px solid lighten($main-text-color, 10%);
            color: lighten($main-text-color, 10%);
            box-shadow: 0px 0px 2px $main-text-color;
        }
    }
}

.main_file_input {
    max-width: 400px;
    margin: 0 auto;
}
</style>
