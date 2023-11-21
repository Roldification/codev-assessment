<template>
    <div style="padding-top: 15px; padding-left: 50px; padding-right: 50px; width: 100%; display: flex; flex-direction: column; align-items: center;">
        <span style="font-weight: bold;">Please provide your PO # for the Invoice</span>
        <v-text-field ref="workspace" :rules="[rules.required]" style="width: 100%; margin-top:15px;" label="Workspace name" variant="outlined" v-model="invoiceDetails.workspaceName"></v-text-field>
        <v-text-field :disabled="true" v-model="invoiceDetails.subscriptionPlan" style="width: 100%;" label="Subscription Plan" variant="outlined"></v-text-field>
        <v-text-field ref="amount" v-model="invoiceDetails.billingAmount" :rules="[rules.amountOnly]" :prepend-inner-icon="'mdi-currency-usd'" style="width: 100%;" label="Billing Amount" variant="outlined"></v-text-field>
        <v-text-field v-model="invoiceDetails.billingPeriod" style="width: 100%;" label="Billing Period" :disabled="true" variant="outlined"></v-text-field>
        <v-text-field v-model="invoiceDetails.poNumber" ref="poNumber" :rules="[rules.required, rules.numberOnly]" style="width: 100%;" label="PO #" variant="outlined"></v-text-field>
        <div style="font-size: smaller; margin-top: 10px;">
            <span style="color: darkgrey;">This PO# will be attached to your invoice. Please check your PO# before submitting as this cannot be changed afterwards.</span>
        </div>
        <div style="width: 100%; margin-top: 10px;">
            <v-btn :loading="isFormSubmitting" @click="submitPO" block color="black"  height="48">
                Submit PO #
            </v-btn>
        </div>
    </div>
    <div style="padding-left: 50px; margin-bottom: 50px;">
        <Goback></Goback>
    </div>
</template>

<script setup lang="ts">
    import { z } from 'zod'
    const invoiceDetails = ref({
        workspaceName: '',
        subscriptionPlan: 'Teams',
        billingAmount: '',
        billingPeriod: 'Jan 11, 2023 - Feb 11, 2023',
        poNumber: ''
    })

    useHead({
        title: 'Invoice Submission'
    })

    const amount = ref()
    const poNumber = ref()
    const workspace = ref()
    const isFormSubmitting = ref(false)

    const rules = {
        numberOnly: (val: string) => {
            const schema = z.coerce.number({
                invalid_type_error: 'PO # must only include numbers',
            }).int({
                message: 'PO # only accepts Integers'
            })

            try {
                schema.parse(val)
                return true
            } catch (error: any) {
                if (error instanceof z.ZodError) {
                    return error.errors[0].message
                } return 'something is not right'
            }
        },
        required: (val: string) => {
            const schema = z.string({
                required_error: 'This field is required.'
            }).min(1, 'This field is required.')

            try {
                schema.parse(val)
                return true
            } catch (error: any) {
                if (error instanceof z.ZodError) {
                    return error.errors[0].message
                } return 'something is not right'
            }
        },
        amountOnly: (val: string) => {
            const schema = z.coerce.number({
                invalid_type_error: 'Amount must be a number'
            }).gt(0, "Must be greater than 0")

            try {
                schema.parse(val)
                return true
            } catch (error) {
                if (error instanceof z.ZodError) {
                    return error.errors[0].message
                } return 'something is not right'
            }
        }

    }

    async function submitPO() {
        amount.value.validate()
        poNumber.value.validate()
        workspace.value.validate()

        if (amount.value.isValid && poNumber.value.isValid && workspace.value.isValid) {
            isFormSubmitting.value = true
            try {
                const response = await $fetch(`http://127.0.0.1:1323/save-po`, {
                method: 'POST',
                body: invoiceDetails.value
            })

            await navigateTo('/thankyou')
            
            } catch (error: any) {
                console.log(error.data ?? error)
            } finally {
                isFormSubmitting.value = false
            }
        }

    }
</script>