<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12 col-lg-10">
                <div class="wrap d-md-flex">
                    <div class="text-wrap p-4 p-lg-5 text-center d-flex align-items-center order-md-last">
                        <div class="form-filter"></div>
                        <div class="text w-100">
                            <h2>Ubezpieczenia OC</h2>
                            <p>Wypełnij formularz a wyślemy Ci wniosek przystąpienia do ubezpieczenia.</p>
                        </div>
                    </div>
                    <div class="login-wrap p-4 p-lg-5">
                        <div class="d-flex">
                            <div class="w-100">
                                <h3 class="mb-4">Przystąp do ubezpieczenia OC</h3>
                            </div>

                        </div>
                        <form class="signin-form" @submit.prevent="addInsurance" method="post">
                            <div class="form-group mb-3">
                                <v-select
                                    :items="items"
                                    filled
                                    label="Wybierz typ ubezpieczenia OC"
                                    dense
                                    required
                                    :menu-props="{'offset-y':true}"
                                    v-model="lead.ins_type"
                                ></v-select>
                            </div>
                            <div class="form-group mb-3">
                                <label class="label" for="email">Adres email *</label>
                                <v-text-field
                                    single-line
                                    id="email"
                                    type="email"
                                    filled
                                    required
                                    v-model="lead.email"
                                    placeholder="jan.kowalski@gmail.com"
                                ></v-text-field>
                            </div>
                            <div class="form-group mb-3">
                                <label class="label" for="profession_number">Numer prawa wykonywania zawodu *</label>
                                <v-text-field
                                    single-line
                                    type="text"
                                    id="profession_number"
                                    filled
                                    required
                                    v-model="lead.profession_numb"
                                    placeholder="63225234 R"
                                ></v-text-field>
                            </div>

                            <div class="form-group d-flex">
                                <div class="mr-3 text-left">
<!--                                    <v-checkbox-->
<!--                                        style="margin-top: -4px;"-->
<!--                                        color="#453CFF"-->
<!--                                        value="primary"-->
<!--                                        hide-details-->
<!--                                        :ripple=false-->
<!--                                        v-model="lead.is_checked"-->
<!--                                    ></v-checkbox>-->
                                    <input type="checkbox" required id="agreement" v-model="lead.is_checked">
                                </div>
                                <label class="text-md-left" for="agreement">
                                    Adres poczty elektronicznej podaję dobrowolnie w celu przystąpienia do Programu
                                    ubezpieczenia dla lekarzy członków OIL w Warszawie i WIL oraz zgadzam się na kontakt
                                    przez INS Services Sp. z o.o. KRS nr 0000754412, jako podmiot obsługujący ten
                                    program w podanym celu.
                                </label>
                            </div>
                            <div class="form-group mb-3 mt-4">
                                <button type="submit" class="form-control btn btn-primary submit px-3">Wyślij</button>
                                <p v-if="success" class="text-success mt-2 text-center">Formularz przesłany Prawidłowo</p>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Request from '../Request';

export default {
    name: "InsuranceForm",
    data: () => {
        return {
            success: false,
            errors: {},
            lead: {
                ins_type: "obowiazkowe",
                is_checked: 0
            },
            items: ['Ubezpieczenie dobrowolne', 'Ubezpieczenie obowiązkowe'],

        }
    },
    created() {
    },
    methods: {
        addInsurance() {
            this.errors = {};
            Request({
                url: '/api/insurances',
                method: 'POST',
                data: this.lead
            }).then(res => {
                this.success = true;
                this.lead.email = '';
                this.lead.is_checked = '';
                this.lead.profession_numb = '';
                this.lead.ins_type = '';
                this.lead.is_checked = false;
            }).catch(e => {
                console.log(e);
                load.error = 'Niepowodzenie!';
            })

        }
    }
}
</script>

<style scoped>
 .container {
     border-radius: 5px;
 }
</style>
