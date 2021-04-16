<template>
    <form class="sign-up" @submit.prevent="checkForm">
        <div class="form-group">
            <label for="familiya">Фамилия:</label>
            <div class="input">
                <input
                        id="familiya"
                        class="form-control"
                        :class="$v.form.familiya.$error ? 'is-invalid' : ''"
                        v-model.trim="form.familiya"
                >
                <p v-if="$v.form.familiya.$dirty && !$v.form.familiya.required" class="invalid-feedback">
                    *Обязательное поле
                </p>
            </div>
        </div>
        <div class="form-group">
            <label for="name">Имя:</label>
            <div class="input">
                <input
                        id="name"
                        class="form-control"
                        :class="$v.form.name.$error ? 'is-invalid' : ''"
                        v-model.trim="form.name"
                >
                <p v-if="$v.form.name.$dirty && !$v.form.name.required" class="invalid-feedback">
                    *Обязательное поле
                </p>
            </div>
        </div>
        <div class="form-group">
            <label for="name">Отчество:</label>
            <div class="input">
                <input
                        id="surname"
                        class="form-control"
                        v-model.trim="form.surname"
                >
            </div>
        </div>
        <div class="form-group">
            <label for="name">Дата рождения:</label>
            <div class="input">
                <input
                        id="birthday"
                        class="form-control"
                        type="date"
                        min="1960-01-01"
                        :class="$v.form.birthday.$error ? 'is-invalid' : ''"
                        v-model.trim="form.birthday"
                >
                <p v-if="$v.form.birthday.$dirty && !$v.form.birthday.required" class="invalid-feedback">
                    *Обязательное поле
                </p>
            </div>
        </div>
        <div class="form-group">
            <label for="name">Номер телефона:</label>
            <div class="input">
                <input
                        id="phone"
                        class="form-control"
                        type="text"
                        v-model.trim="form.phone"
                        placeholder="7 (555) 555-5555"
                        @input="acceptNumber($event)"
                        :class="$v.form.phone.$error ? 'is-invalid' : ''"
                >
                <p v-if="$v.form.phone.$dirty && !$v.form.phone.required" class="invalid-feedback">
                    *Обязательное поле
                </p>
            </div>
        </div>
        <div class="flex">
            <div class="form-check">
                <input class="form-check-input" type="radio" value="male" name="exampleRadios" id="male" v-model="form.gendere" >
                <label class="form-check-label" for="male">
                    Мужчина
                </label>
            </div>
            <div class="form-check">
                <input class="form-check-input" type="radio" value="female" name="exampleRadios" id="female" v-model="form.gendere">
                <label class="form-check-label" for="female">
                    Женщина
                </label>
            </div>
        </div>
        <div class="form-group">
            <label for="country">Группа клиентов:</label>
            <select id="country" class="form-control"
                    :class="$v.form.client.$error ? 'is-invalid' : ''"
                    v-model="form.client">
                <option
                        v-for="(client, index) in clients"
                        :value="client.value"
                        :key="index"
                >
                    {{ client.label }}
                </option>
            </select>
            <p v-if="$v.form.client.$dirty && !$v.form.client.required" class="invalid-feedback">
                *Обязательное поле
            </p>
        </div>
        <div class="form-group">
            <label for="themes">Лечащий врач:</label>
            <select id="themes" style="min-height: 60px!important;"
                    class="form-control" v-model="form.favoriteDoctor" multiple>
                <option
                        v-for="(doctor, index) in doctors"
                        :value="doctor.value"
                        :key="index"
                >
                    {{ doctor.label }}
                </option>
            </select>
        </div>
        <div class="form-group form-check">
            <div class="flex">
                <input type="checkbox" class="form-check-input" id="notification" v-model="form.agreeWithSendEmail">
                <label class="form-check-label" for="notification">
                    Не отправлять СМС:
                </label>
            </div>
        </div>
        <button type="submit" class="btn btn-primary">Сохранить</button>
    </form>
</template>

<script>
    import { validationMixin } from 'vuelidate'
    import { required } from 'vuelidate/lib/validators'

    export default {
        mixins: [validationMixin],
        data() {
            return {
                form: {
                    familiya: '',
                    name: '',
                    surname: '',
                    birthday: '',
                    phone: '',
                    client: '',
                    favoriteDoctor: [''],
                    agreeWithSendEmail: false,
                    gendere: 'male'
                },
                clients: [
                    {
                        label: 'VIP',
                        value: 'VIP'
                    },
                    {
                        label: 'Проблемные',
                        value: 'Problems'
                    },
                    {
                        label: 'ОМС',
                        value: 'OMC'
                    }
                ],
                doctors: [
                    {
                        label: 'Иванов',
                        value: 'Ivan'
                    },
                    {
                        label: 'Захаров',
                        value: 'Zahar'
                    },
                    {
                        label: 'Чернышева',
                        value: 'Cher'
                    }
                ]
            }
        },
        validations: {
            form: {
                familiya: { required },
                name: { required },
                birthday: { required },
                phone: { required },
                client: { required },
                surname: { },
            }
        },
        methods: {
            checkForm() {
                this.$v.form.$touch()
                if (!this.$v.form.$error) {
                    window.alert('Пользователь успешно создан!');
                    this.form.surname = '';
                    this.form.name = '';
                    this.form.phone = '';
                    this.form.birthday = '';
                    this.form.agreeWithSendEmail = '';
                    this.form.client = '';
                    this.form.familiya = '';
                    this.form.favoriteDoctor = '';
                }
            },
            acceptNumber(e) {
                let x = e.target.value.replace(/\D/g, '').match(/(\d{0,1})(\d{0,3})(\d{0,3})(\d{0,4})/);
                this.form.phone = x[1] != 7 ? 7 + ' ' + (!x[3] ? x[2] : '(' + x[2] + ') ' + x[3] + (x[4] ? '-' + x[4] : ''))
                    : x[1] + ' ' + (!x[3] ? x[2] : '(' + x[2] + ') ' + x[3] + (x[4] ? '-' + x[4] : ''));
            }
        }
    }
</script>

<style lang="scss" scoped>
    form {
        height: calc(100% - 20px);
        padding: 10px 0;

        .form-group {
            display: flex;
            flex-direction: column;
            width: 500px;
            margin-bottom: 10px;
            position: relative;

            label {
                margin-bottom: 5px;
                color: #ccc;
                font-weight: 600;
            }

            .input {
                width: 100%;
                display: flex;
                position: relative;
                align-items: center;
            }

            .invalid-feedback {
                color: rgba(255, 0, 0, .5)!important;
                position: absolute;
                right: -150px;
            }

            input[type="text"] {
                display: flex;
                align-items: center;
                border: 1px solid #4d4d4d;
                padding: 0 .5rem;
                height: 32px;
                background-color: #212121;
                color: #ccc;
                width: calc(100% - 18px);
                outline: 0;
            }

            input[type="checkbox"] {
                height: 20px;
                border: 1px solid #4d4d4d;
                background-color: #212121;
                color: #ccc;
                width: 20px;
                outline: 0;
            }

            .is-invalid {
                border-color: rgba(255, 0, 0, .5)!important;
            }
        }
    }
    .form-control {
        border: 1px solid transparent;
        width: 100%;
        transition: all ease-in-out 0.5s;
        background: #212121;
        color: #808080!important;
        outline: none;
        height: 34px;
        padding: 0 8px;
    }
    .form-check {
        margin-right: 10px;
    }
    button {
        margin-top: 15px;
        background: rgba(0, 102, 51, 0.7);
        color: #E6E6E6;
        border: none !important;
        -moz-user-select: none;
        -ms-user-select: none;
        cursor: pointer;
        outline: 0;
        margin: 0;
        min-width: 64px;
        line-height: 36px;
        padding: 0 16px;
    }

    .flex {
        display: flex;
        align-items: center;
    }
</style>
