<script setup>
import { ref } from 'vue'
import { Form, Field, ErrorMessage } from 'vee-validate';
import * as yup from 'yup'
import moment from 'moment'

const height = ref(180)
const logoUrl = `${import.meta.env.BASE_URL}esx-logo.png`

const onSubmit = (values) => {
    fetch("http://esx_identity/register", {
            method: "POST",
            body: JSON.stringify({
                firstname: values.firstname,
                lastname: values.lastname,
                dateofbirth: moment(values.dob).format("DD/MM/YYYY"),
                sex: values.gender,
                height: values.height,
            }),
        });
}

const schema = yup.object({
    firstname: yup.string().required('Firstname is required').min(3, 'Firstname must be at least 3 characters'),
    lastname: yup.string().required('Lastname is required').min(3, 'Lastname must be at least 3 characters'),
    dob: yup.date()
    .required('Date of Birth is required')
    .min(new Date("1900-01-01"), "Date is too early")
    .max(moment().subtract(1, 'years').toDate(), "You need to be atleast 1 year old"),
    gender: yup.string().required('Gender is required'),
    height: yup.number().required('Height is required').min(120, 'Minimum height is 120cm').max(220, 'Maximum height is 220cm').typeError('Amount must be a number'),
})

</script>

<template>
    <header class="identity-heading">
        <img class="identity-heading__logo" :src="logoUrl" alt="ESX logo">
        <div class="identity-heading__copy">
            <h1 data-text="IDENTITY CREATION">IDENTITY CREATION</h1>
            <p>CREATE YOUR IDENTITY AND START YOUR NEW ADVENTURE</p>
        </div>
    </header>

    <div class="dialog">
        <Form v-slot="{ meta }" class="dialog__body-form" id="register" action="#" novalidate
            @submit="onSubmit" :validation-schema="schema">
                <div class="dialog__form-group dialog__form-group--field">
                    <div class="dialog__field-content">
                        <label for="firstname">FIRST NAME</label>
                        <Field id="firstname" type="text" name="firstname" placeholder="John" validateOnInput />
                    </div>
                    <i class="fas fa-address-card dialog__form-icon" aria-hidden="true"></i>
                    <ErrorMessage name="firstname" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group dialog__form-group--field">
                    <div class="dialog__field-content">
                        <label for="lastname">LAST NAME</label>
                        <Field id="lastname" type="text" name="lastname" placeholder="Doe" validateOnInput />
                    </div>
                    <i class="fas fa-address-card dialog__form-icon" aria-hidden="true"></i>
                    <ErrorMessage name="lastname" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group dialog__form-group--field">
                    <div class="dialog__field-content">
                        <label for="dob">DATE OF BIRTH</label>
                        <Field id="dob" type="date" name="dob" validateOnInput />
                    </div>
                    <i class="fas fa-calendar-days dialog__form-icon" aria-hidden="true"></i>
                    <ErrorMessage name="dob" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group dialog__form-group--stacked">
                    <label for="height">HEIGHT</label>
                    <div class="dialog__height-control">
                        <Field v-model="height" id="height" type="range" name="height" min="120" max="220"
                            :style="{ '--height-progress': `${height - 120}%` }" validateOnInput/>
                        <output for="height">{{ height }}</output>
                        <i class="fas fa-arrows-up-down dialog__form-icon" aria-hidden="true"></i>
                    </div>
                    <ErrorMessage name="height" class="dialog__form-message dialog__form-message--error" />
                </div>
                <div class="dialog__form-group dialog__form-group--stacked dialog__form-group--gender">
                    <label>GENDER</label>
                    <div class="dialog__gender-row">
                      <div class="dialog__form-group--radio">
                        <div class="dialog__form-radio">
                            <Field type="radio" id="male" value="m" name="gender" validateOnInput />
                            <label for="male" aria-label="Male"><i class="fas fa-mars"></i></label>
                        </div>
                        <div class="dialog__form-radio">
                            <Field type="radio" id="female" value="f" name="gender" validateOnInput />
                            <label for="female" aria-label="Female"><i class="fas fa-venus"></i></label>
                        </div>
                      </div>
                      <i class="fas fa-venus-mars dialog__form-icon" aria-hidden="true"></i>
                    </div>
                    <ErrorMessage name="gender" class="dialog__form-message dialog__form-message--error" />
                </div>
                <button class="dialog__form-submit" id="submit" type="submit" :disabled="!meta.valid || !meta.dirty">
                    CREATE MY CHARACTER
                </button>
        </Form>
    </div>
</template>

<style scoped>
</style>
