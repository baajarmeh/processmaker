<template>
    <div class="modal" tabindex="-1" role="dialog">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">{{ title }}</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close" @click="onClose">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body text-center">
                    <div class="my-3" :class="classMessage" v-html="message"></div>
                </div>
                <div class="modal-footer">
                    <button id="cancel" type="button" class="btn btn-outline-secondary" data-dismiss="modal" @click="onDeny">{{ $t('Cancel') }}</button>
                    <button id="confirm" type="button" class="btn btn-secondary" @click="onConfirm">{{ $t('Confirm') }}</button>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
    export default {
        props: ["title", "message", "variant", "callback"],
        data() {
            return {
                'classMessage': '',
                'classButtonCancel': '',
                'classButtonConfirm': ''
            }
        },
        watch: {
            variant(value) {
                this.styles()
            }
        },
        methods: {
            styles() {
                this.classMessage = '';
                this.classButtonCancel = 'btn btn-outline-success btn-sm text-uppercase';
                this.classButtonConfirm = 'btn btn-success btn-sm text-uppercase';
                if (this.variant) {
                    this.classMessage += ' text-' + this.variant;
                    this.classButtonCancel += ' btn-outline-' + this.variant;
                    this.classButtonConfirm += ' btn-' + this.variant;
                }
            },
            onClose() {
                this.$emit('close');
            },
            onConfirm() {
                this.$emit("confirm", true);
                if (this.callback) {
                    this.callback();
                }
                this.onClose();
            },
            onDeny() {
                this.$emit("confirm", false);
                this.onClose();
            }
        },
        mounted() {
            this.$emit("show");
            this.styles();
        }
    }
</script>

<style scoped>

    .modal {
        position: fixed;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        background: rgba(0, 0, 0, .5);
        z-index: 1060;
        display: flex;
        min-width: 30%;
    }

    .modal-dialog {
        min-width: 400px;
        top: 20%;
    }

</style>