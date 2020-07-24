<template>
    <transition name="modal">
        <div class="modal__wrapper" @click.self="closeModal">
                <div class="modal-content">
                    <div class="modal-header">
                        <span class="modal-title">{{ title }}</span>
                        <span
                                class="button-close"
                                @click="closeModal"
                        >×</span>
                    </div>
                    <div class="modal-body">
                        <slot name="body">Default text</slot>
                    </div>
                </div>
        </div>
    </transition>
</template>

<script>
    export default {
        name: "SimpleModal",
        props: {
            title: {
                type: String,
                required: true
            }
        },
        mounted() {
            document.body.addEventListener('keyup', elem => {
                if (elem.key === 'Escape') this.closeModal()
            })
        },
        methods: {
            closeModal () {
                this.$emit('close')
            }
        }
    }
</script>

<style scoped lang="scss">
    .modal__wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        transition: opacity .4s ease;
        z-index: 998;
        background-color: rgba(00, 00, 00, .48);
    }

    .modal-content {
        position: relative;
        max-width: 600px;
        padding: 20px 18px;
        background-color: #fff;
        border: 1px solid #dcdfe6;
        transition: all .4s ease;
        border-radius: 8px;
        z-index: 999;
        overflow: hidden;
        @media screen and (min-width: 900px) {
            min-width: 500px;
        }
    }

    .modal-header {
        display: flex;
        align-self: center;
        justify-content: space-between;
        padding-bottom: 20px;

        span {
            font-size: 24px;
        }

        .button-close {
            cursor: pointer;
        }
    }

    .modal-body {
        text-align: center;
    }


    /*ANIMATION*/
    .modal-enter-active {
        transition: all .3s ease;
    }

    .modal-leave-active {
        transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }

    .modal-enter, .modal-leave-to {
        opacity: 0;
    }

    .modal-enter .modal-content {
        transform: translateY(30px);
    }

    .modal-leave-to .modal-content {
        transform: translateY(30px);
    }

</style>
