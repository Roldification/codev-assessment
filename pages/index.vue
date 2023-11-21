<template>
    <nav>
        <v-app-bar :title="'Trendpop'" :elevation="2">
            <template v-slot:prepend>
                <v-app-bar-nav-icon></v-app-bar-nav-icon>
            </template>
        </v-app-bar>
    </nav>
    <v-main>

        <div style="padding-top: 15px; padding-left: 50px; padding-right: 50px; width: 100%; display: flex; flex-direction: column; align-items: center;">
            <span style="font-weight: bold;">Please provide your PO # for the Invoice</span>
            <v-text-field style="width: 100%; margin-top:15px;" label="Workspace name" variant="outlined" v-model="invoiceDetails.workspaceName"></v-text-field>
            <v-text-field style="width: 100%;" label="Subscription Plan" variant="outlined"></v-text-field>
            <v-text-field :prepend-inner-icon="'mdi-currency-usd'" style="width: 100%;" label="Billing Amount" variant="outlined"></v-text-field>
            <v-text-field style="width: 100%;" label="Billing Period" variant="outlined"></v-text-field>
            <v-text-field style="width: 100%;" label="PO #" variant="outlined"></v-text-field>
            <div style="font-size: smaller; margin-top: -10px;">
                <span style="color: darkgrey;">This PO# will be attached to your invoice. Please check your PO# before submitting as this cannot be changed afterwards.</span>
            </div>
            <div style="width: 100%; margin-top: 10px;">
                <v-btn block color="black"  height="48">
                    Submit PO #
                </v-btn>
            </div>
        </div>
    </v-main>
</template>

<script setup lang="ts">
    const invoiceDetails = ref({
        workspaceName: '123',
        subscriptionPlan: '',
        billingAmount: '',
        billingPeriod: '',
        poNumber: ''
    })
</script>