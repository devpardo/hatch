<template>
    <v-container>
        <v-row>
            <v-col>
                <h2 class="mb-5">Project</h2>

                <v-stepper v-model="step" flat class="rounded-0 transparent">
                    <v-stepper-header class="mb-10 white">
                        <v-stepper-step
                            :complete="step > 1"
                            step="1"
                        >
                            Configuration
                        </v-stepper-step>

                        <v-divider></v-divider>

                        <v-stepper-step
                            step="2"
                        >
                            Summary
                        </v-stepper-step>
                    </v-stepper-header>

                    <v-stepper-items class="transparent">
                        <v-stepper-content step="1" class="p-0">

                            <v-data-table
                                class="transparent mb-10"
                                hide-default-footer
                                :headers="headers"
                                :items="items"
                            >

                                <template v-slot:[`item.plan_name`]="{ item, index }">
                                    <div class="mt-5">
                                        <v-text-field
                                            :data-text="item"
                                            label="Product Name"
                                            outlined
                                            dense
                                            v-model="items[index].plan_name"
                                        ></v-text-field>
                                    </div>
                                </template>

                                <template v-slot:[`item.amount`]="{ item, index }">
                                    <div class="mt-5">
                                        <v-text-field
                                            type="number"
                                            :data-text="item"
                                            label="Dollar Amount"
                                            outlined
                                            dense
                                            v-model="items[index].amount"
                                        ></v-text-field>
                                    </div>
                                </template>

                                <template v-slot:[`item.job_owner`]="{ item, index }">
                                    <div class="mt-5">
                                        <v-select
                                            :items="job_owner"
                                            :data-text="item"
                                            label="Job Owner"
                                            outlined
                                            dense
                                            v-model="items[index].job_owner"
                                        ></v-select>
                                    </div>
                                </template>

                                <template v-slot:[`item.actions`]="{ item, index }">
                                    <div class="pa-0 ma-0">
                                        <v-btn text small :data-text="item" :data-index="index" @click="deleteItem(index)">
                                            <v-icon>mdi-delete</v-icon>
                                        </v-btn>
                                    </div>
                                </template>

                        
                            </v-data-table>


                            <div class="d-flex justify-space-between">
                                <v-btn small plain text class="primary--text" @click="addRow">
                                    + add row
                                </v-btn>

                                <v-btn small class="primary rounded-xl" @click="nextStep">
                                    Next
                                </v-btn>
                            </div>

                        </v-stepper-content>

                        <v-stepper-content step="2" class="p-0">
                            <v-row>
                                <v-col sm="6">
                                    <v-data-table
                                        class="transparent mb-10"
                                        hide-default-footer
                                        :headers="headersStep2"
                                        :items="items"
                                    >

                                        <template v-slot:[`item.job_owner`]="{ item }">
                                            {{ jobOwner(item.job_owner) }}
                                        </template>
                                    </v-data-table>
                                    
                                    <div class="d-flex justify-space-between">
                                        <v-btn small plain text class="primary--text" @click="step = 1">
                                            &lt; back
                                        </v-btn>
                                    </div>
                                </v-col>
                            </v-row>
                        </v-stepper-content>
                    </v-stepper-items>
                </v-stepper>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>

export default {
  name: 'TableStepper',

  data: () => ({
    step : 1,
    headers: [
        { text: 'Add plan name', value: 'plan_name', width: '30%' },
        { text: '$ Amount', value: 'amount', width: '30%' },
        { text: 'Job Owner', value: 'job_owner', width: '30%' },
        { text: '', value: 'actions', width: '10%' }
    ],
    headersStep2: [
        { text: '$ Sum', value: 'amount', width: '30%' },
        { text: 'Job Owner', value: 'job_owner', width: '30%' },
    ],
    items: [],
    next: false,
    job_owner: []

  }),
  mounted() {
    this.fetchOwners();
  },
  methods: {
    async fetchOwners() {
        const res = await fetch('https://demo-api.bettercommissions.com/interview-data/users');
        const data = await res.json();
        
        let users = data.users.map((item) => {
           return {
                text : item.name,
                value: item.id
           }
        })

        this.job_owner = users;
    },
    addRow() {
        this.items.push({
            plan_name: '',
            amount: '',
            job_owner: ''
        })
    },
    deleteItem(index) {
        this.items.splice(index, 1);
    },
    nextStep() {
        if(!this.items.length) {
            return
        }

        this.step = 2;
    },
    jobOwner(id) {
        if(this.step == 2) {
            return this.job_owner.filter(item => item.value == id)[0].text;
        }
    }
  }
};

</script>