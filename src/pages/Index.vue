<template>
  <q-page class="flex flex-center">
    <q-card class="page-container">
      <img src="../../public/logo_calc.png" alt="logo" class="logo" />
      <div class="title">
        <div>Kalkulator Usług Księgowych</div>
        <q-separator color="secondary" />
        <q-separator color="secondary" />
        <q-separator color="secondary" />
      </div>
      <div class="select q-gutter-sm">
        Wybierz formę ewidencji:
        <q-select
          class="selector"
          outlined
          dense
          v-model="chosen_evidence_type"
          :options="evidence_types"
          @input="calc_total_amount"
        />
      </div>
      <div class="toggles">
        <Toggles
          ref="togglesData"
          :chosen_evidence_type="evidence_types.indexOf(chosen_evidence_type)"
          @switch="calc_total_amount"
        />
      </div>
      <div class="slider row">
        <div class="col-7 invoices-question">Ile masz średnio zapisów w ewidencji podatkowej?</div>
        <q-slider
          v-model="invoices"
          class="col-3"
          :min="0"
          :max="50"
          label
          label-always
          color="orange"
          @input="calc_total_amount"
        />
      </div>
      <div class="sum">
        <div>Całkowity miesięczny koszt: {{ total }}zł</div>
        <q-separator color="secondary" />
        <q-separator color="secondary" />
      </div>
      <div class="sum brutto">
        <div>Całkowity miesięczny koszt brutto: {{ total_brutto }}zł</div>
        <q-separator color="red" />
        <q-separator color="red" />
      </div>
    </q-card>
  </q-page>
</template>

<script>
import Toggles from '../components/Toggles';

export default {
  name: 'PageIndex',
  components: {
    Toggles,
  },
  data() {
    return {
      pro_digital: 0,
      evidence_types: [
        { label: 'Ewidencja przychodów', base_amount: 84.26 },
        { label: 'Ewidencja przychodów (plus)', base_amount: 100 },
        { label: 'Podatkowa księga przychodów i rozchodów (plus)', base_amount: 120 },
      ],
      chosen_evidence_type: 0,
      invoices: 0,
      total: 0,
      total_brutto: 0,
    };
  },
  mounted() {
    (this.chosen_evidence_type = this.evidence_types[0]), this.calc_total_amount();
  },
  methods: {
    calc_total_amount() {
      this.total =
        Math.round(
          (this.chosen_evidence_type.base_amount +
            this.$refs.togglesData.getTotalValue() +
            this.invoices * 5) *
            100
        ) / 100;
      this.total_brutto = Math.round(this.total * 123) / 100;
    },
  },
};
</script>

<style lang="scss" scoped>
.page-container {
  margin-top: 5vh;
  margin-bottom: 5vh;
  min-height: 90vh;
  min-width: 60vw;
  background-color: white;
  .logo {
    width: 200px;
    margin: 0 auto;
  }
  .select {
    width: 95%;
    font-weight: 600;
    font-size: 16px;
    margin: 20px auto;
    .selector {
      width: 60%;
      margin-left: 30px;
    }
  }
  .title,
  .sum {
    width: 95%;
    color: orange;
    font-weight: 600;
  }
  .title {
    margin: 0 auto;
    font-size: 20px;
  }
  .slider {
    width: 95%;
    margin: 30px auto;
    .invoices-question {
      margin-left: 30px;
    }
  }
  .sum {
    margin: 30px auto;
    font-size: 16px;
  }
  .brutto {
    color: red;
  }
  .toggles {
    width: 95%;
    margin: 30px auto 0 auto;
  }
}
</style>
