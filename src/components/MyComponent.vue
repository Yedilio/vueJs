<template>
    <div class="root">
        <label>
            Enter email:
            <input type="email"
                   :value="email"
                    @input="emailInput($event.target.value)"
            />
        </label>

        <label>
            Enter email:
            <input type="password"
                   :value="password"
                   @input="passwordInput($event.target.value)"
            />
        </label>
    </div>
</template>

<script>
    export default {
        name: 'MyComponent',
        data() {
            return {
                email: 'example@mail.com',
                password: '222222'
            }
        },
        methods: {
            emailInput(value) {
                this.email = value;
            },
            passwordInput(value) {
                this.password = value;
            }
        }
    }
</script>
