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
      <div class="digitals q-gutter-sm">
        Wybierz wariant usługi:
        <br />
        <q-btn-toggle
          v-model="chosen_pro_digital"
          class="digital"
          color="primary"
          text-color="black"
          toggle-color="orange"
          toggle-text-color="black"
          rounded
          unelevated
          glossy
          no-caps
          :options="digitalOptions"
          @click="calc_total_amount"
        />
      </div>
      <div class="select q-gutter-sm">
        Wybierz formę ewidencji:
        <br />
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
      <div class="sum">
        <div>Wartość usług ryczałtowych: {{ lump_sum }}zł</div>
        <q-separator color="secondary" />
        <q-separator color="secondary" />
        <div class="closure">
          Opłata zawiera wartość wpisów w ewidencji do kwoty {{ included_invoices_sum }}zł
        </div>
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
        <div>Wartość usługi księgowej z uwzględnieniem ilości wpisów: {{ total_sum }}zł</div>
        <q-separator color="secondary" />
        <q-separator color="secondary" />
      </div>
      <div class="closure">
        Powyższy kalkulator ma charakter informacyjny i nie stanowi oferty handlowej w rozumieniu
        art. 66 §1 Kodeksu Cywilnego.
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
      pro_digitals: [1, 10, 30],
      chosen_pro_digital: 0,
      evidence_types: [
        { label: 'Ewidencja przychodów', base_amount: [100, 120, 160] },
        { label: 'Podatkowa księga przychodów i rozchodów', base_amount: [120, 140, 180] },
      ],
      chosen_evidence_type: 0,
      invoices: 0,
      included_invoices_sum: 0,
      lump_sum: 0,
      total_sum: 0,
    };
  },
  computed: {
    digitalOptions() {
      let options = [];
      return (options = this.pro_digitals.map((type) => ({
        label: `Pro-digital (${type})`,
        value: this.pro_digitals.indexOf(type),
      })));
    },
  },
  mounted() {
    this.chosen_evidence_type = this.evidence_types[0];
    this.calc_total_amount();
  },
  methods: {
    async calc_total_amount() {
      await this.$refs.togglesData.getTotalValue();
      const icluded_invoices = this.pro_digitals[this.chosen_pro_digital];
      const invoice_cost = this.$refs.togglesData.isVatActive() ? 4 : 3.5;

      this.included_invoices_sum = icluded_invoices * invoice_cost;

      this.lump_sum =
        Math.round(
          (this.chosen_evidence_type.base_amount[this.chosen_pro_digital] +
            this.$refs.togglesData.getTotalValue()) *
            100
        ) / 100;

      const invoices_amount =
        icluded_invoices > this.invoices ? 0 : (this.invoices - icluded_invoices) * invoice_cost;
      this.total_sum = this.lump_sum + invoices_amount;
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
  .select,
  .digitals {
    width: 95%;
    font-weight: 600;
    font-size: 16px;
    margin: 20px auto;
    .selector {
      width: 60%;
      margin-left: 30px;
    }
    .digital {
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
    margin: 40px auto;
    .invoices-question {
      margin-left: 30px;
    }
  }
  .sum {
    margin: 20px auto 40px auto;
    font-size: 16px;
  }
  .closure {
    color: grey;
    font-size: 10px;
    text-align: end;
    margin-right: 5px;
  }
  .toggles {
    width: 95%;
    margin: 30px auto 0 auto;
  }
}
</style>
