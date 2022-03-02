<template>
  <div class="order">
    <form>
        <p class="section-title">Seus dados</p>
        <div class="input-field">
            <label for="">{{formData.name.label}}</label>
            <input type="text" :placeholder="formData.name.placeholder" v-model="formData.name.value" 
                @blur="formData.name.isValid()" :class="{'error' : !formData.name.valid}">
            <p class="error-message" v-if="!formData.name.valid">{{formData.name.errorMessage}}</p>
        </div>
        <div class="input-field">
            <label for="">{{formData.cellphone.label}}</label>
            <input type="text" :placeholder="formData.cellphone.placeholder" v-model="formData.cellphone.value" 
                @blur="formData.cellphone.isValid()" :class="{'error' : !formData.cellphone.valid}" v-mask="'(##) # ####-####'">
                                
            <p class="error-message" v-if="!formData.cellphone.valid">{{formData.cellphone.errorMessage}}</p>
        </div>
    </form>
    <button class="primary-button" @click="orderItens()">Concluir pedido</button>
  </div>
</template>

<script>
export default {
    data() {
        return {
            formData: {
                name: {
                    value: '',
                    errorMessage: 'O nome é obrigatório',
                    label: 'Nome*',
                    placeholder: 'Digite seu nome',
                    valid: true,
                    isValid: () => {
                        this.formData.name.valid = this.formData.name.value.length;
                    }
                },
                cellphone: {
                    value: '',
                    errorMessage: 'O número de telefone é obrigatório',
                    label: 'Celular*',
                    placeholder: 'Digite seu número de telefone',
                    valid: true,
                    isValid: () => {
                        this.formData.cellphone.valid = (this.formData.cellphone.value.length === 16);
                    }
                },

            }
        }
    },
    methods: {
        triggerValidations() {
            this.formData.name.isValid();
            this.formData.cellphone.isValid();
        },
        orderItens() {
            this.triggerValidations();
        }
    }
}
</script>

<style lang="less" scoped>
.order {
    width: 720px;
    margin: 30px auto;
    background: white;
    border-radius: 8px;
    padding: 30px 50px;

    form {
        display: flex;
        flex-direction: column;

        .input-field {
            display: flex;
            flex-direction: column;

            & + .input-field {
                margin-top: 10px;
            }
        }

        .section-title {
            font-weight: 600;
            font-size: 22px;
            margin-bottom: 20px;
        }

        label {
            font-weight: 500;
            font-size: 16px;
            margin-bottom: 5px;
        }

        .error-message {
            font-size: 12px;
            color: @error-color;
        }
    }

    button {
        margin: 30px auto;
    }
}
</style>