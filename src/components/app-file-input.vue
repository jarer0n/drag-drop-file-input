<template>
    <div class="app_file_input__wrapper">
        <div
            @drop.prevent="handleDrop"
            @dragover.prevent="isDragover = true"
            @dragenter.prevent="isDragover = true"
            @dragleave.prevent="isDragover = false"
            class="app_file_input__body"
            :class="{ app_file_input__dragover: isDragover }"
        >
            <img src="../assets/img/download.svg" alt="download" />

            <p>Перенесите свои файлы сюда или нажмите чтобы выбрать их!</p>
            <span v-if="uploadedFiles.length">
                Выбрано файлов: {{ uploadedFiles.length }}</span
            >

            <input multiple @change="handleSelect" type="file" />
        </div>

        <transition name="fade">
            <div v-if="uploadedFiles.length">
                <TransitionGroup
                    name="fade"
                    class="app_file_input__list"
                    tag="ul"
                >
                    <li
                        v-if="uploadedFiles"
                        v-for="i in uploadedFiles"
                        :key="i.name"
                    >
                        <div>{{ i.name }}</div>
                        <span @click="removeFile(i)">Удалить[X]</span>
                    </li>
                </TransitionGroup>
                <div class="app_file_input__button">
                    <button @click="submit">Загрузить выбранное</button>
                </div>
            </div>
        </transition>
    </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';

const uploadedFiles = reactive<File[]>([]); //список файлов
const isDragover = ref<boolean>(false);

//!Emits

const emit = defineEmits<{
    (e: 'filesUploaded', fileList: File[]): void;
}>();

//! methods

const findFile = (file: File): number => {
    return uploadedFiles.findIndex((e) => e.name === file.name);
};

const removeFile = (fileForRemove: File): void => {
    const index = findFile(fileForRemove);
    if (index > -1) uploadedFiles.splice(index, 1);
};

const handleDrop = (e: DragEvent): void => {
    const files = e.dataTransfer?.files as FileList;
    checkingAndAddningFiles(files);
};

const handleSelect = (e: Event): void => {
    const fileInputElement = e.target as HTMLInputElement;
    const files: FileList | null = fileInputElement.files;
    if (files) {
        checkingAndAddningFiles(files);
    }
    fileInputElement.value = '';
};

const checkingAndAddningFiles = (fileList: FileList): void => {
    for (let i = 0; i < fileList.length; i++) {
        if (findFile(fileList[i]) > -1)
            alert(`Файл ${fileList[i].name} уже добавлен!`);
        else uploadedFiles.push(fileList[i]);
    }
};

const submit = (): void => {
    emit('filesUploaded', uploadedFiles);
};
</script>

<style lang="scss" scoped>
.app_file_input {
    &__wrapper {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        font-size: 13px;
    }

    &__body {
        input {
            position: absolute;
            display: block;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            opacity: 0;
            cursor: pointer;
        }

        padding: 30px 5px;
        border: 1px dashed $main-text-color;
        border-radius: 5px;
        position: relative;
        transition: all 0.2s ease-in-out;
        text-align: center;
        img {
            margin-bottom: 8px;
            transition: all 0.2s ease-in-out;
            width: 35px;
        }

        span {
            margin-top: 7px;
            font-size: 11px;
            display: inline-block;
        }
        &:hover {
            img {
                transform: scale(1.1);
            }

            background: rgba(255, 255, 255, 0.096);
        }
    }

    &__dragover {
        img {
            transform: scale(1.1);
            transform: translateY(-7px);
        }

        background: rgba(255, 255, 255, 0.096);
    }

    &__list {
        border: 1px solid $main-text-color;
        border-radius: 5px;
        padding: 5px;
        max-height: 100px;
        overflow-y: scroll;
        margin-top: 7px;
        li {
            border-bottom: 1px solid $main-text-color;
            padding: 5px 0px;
            display: flex;
            justify-content: space-between;
            span {
                transition: all 0.2s ease;
                cursor: pointer;
                &:hover {
                    color: lighten($main-text-color, 15%);
                }
            }
        }
    }

    &__button {
        display: flex;
        justify-content: flex-end;
        button {
            font-size: 12px;
            background: transparent;
            color: $accent-color;
            border: 1px solid $accent-color;
            padding: 3px 2px;
            margin-top: 5px;
            transition: all 0.2s ease;
            &:hover {
                color: darken($accent-color, 10%);
                border: 1px solid darken($accent-color, 10%);
            }
        }
    }
}
</style>
