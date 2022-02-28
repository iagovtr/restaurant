<template>
    <div class="item--quantity">
        <button class="buttons" @click="onDecreaseButton()" :disabled="item.quantity == 0">-</button>
        <span class="number">{{item.quantity}}</span>
        <button class="buttons" @click="onIncreaseButton()">+</button>
        <Modal :show="showModal">
            <div class="modal-content">
                <h2>Deseja remover este item do carrinho?</h2>
                <div class="buttons">
                    <button class="primary-button" @click="onRemoveButton()">Sim, remover</button>
                    <button class="secondary-button" @click="onCancelButton()">Cancelar</button>
                </div>
            </div>
        </Modal>  
    </div>
</template>

<script>
import {mapActions} from 'vuex'
import Modal from './Modal.vue'
export default {
    components: {
        Modal
    },
    props: {
        item: {},
        useStore: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            showModal: false
        }
    },
    methods: {
        ...mapActions([
            'increaseQuantity',
            'decreaseQuantity'
        ]),
        onDecreaseButton() {
            if(this.useStore) {
                this.decreaseQuantity(this.item.id);
                if(!this.item.quantity) this.showModal = true;
                return;
            }
            --this.item.quantity;
        },
        onIncreaseButton() {
            if(this.useStore) {
                this.increaseQuantity(this.item.id);
                return;
            }
            ++this.item.quantity;
        },
        onCancelButton() {
            this.increaseQuantity(this.item.id);
            this.showModal = false;
        },
        onRemoveButton() {
            this.showModal = false;
            this.$nextTick(() => {
                this.$store.dispatch('removeFromCart', this.item.id);
            });
        }
    },    
}
</script>

<style lang="less" scoped>
        .item--quantity {
            display: flex;
            align-items: center;
            padding-right: 40px;

            .number {
                font-weight: 500;
                font-size: 18px;
                color: @yellow;
                width: 28px;
                text-align: center;
            }

            .buttons {
                font-weight: 600;
                font-size: 18px;
                cursor: pointer;
                background: none;
                border: 0;

                &:focus {
                    outline: 0;
                }
            }

            .modal-content {
                text-align: center;
        
                button {
                    margin-left: 10px;
                    margin-top: 20px;
                }
            }
        }
</style>