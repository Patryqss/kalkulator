<template>
  <div>
    <div class="title" @click="getTotalValue">
      Odpowiedz na poniższe pytania, aby skonfigurować usługę
    </div>
    <div class="toggles q-gutter-sm">
      <div class="row">
        <div class="info col-8">Czy jesteś podatnikiem VAT?</div>
        <q-toggle
          v-model="togglesValues.vat.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
      <div class="row">
        <div class="info col-8">Czy jesteś płatnikiem ZUS?</div>
        <q-toggle
          v-model="togglesValues.zus.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
      <div class="row">
        <div class="info col-8">
          Czy chcesz uwzględnić rozliczenie roczne w opłacie miesięcznej?
        </div>
        <q-toggle
          v-model="togglesValues.pit.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
      <div class="row">
        <div class="info col-7">
          <span>Czy chcesz, abyśmy wystawiali Twoje faktury sprzedaży?</span>
        </div>
        <div class="col-1">
          <InfoMark
            description="Opłata miesięczna obejmuje wystawienie jednej faktury. Każda kolejna +7.50zł"
          />
        </div>
        <q-toggle
          v-model="togglesValues.sell_service.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
      <div class="row">
        <div class="info col-7">Czy chcesz mieć prowadzoną obsługę kadrowo-płacową?</div>
        <div class="col-1">
          <InfoMark
            description="Opłata obejmuje dostęp do modułu kadrowo-płacowego. Obsługa każdej umowy płatna odrębnie wg cennika."
          />
        </div>
        <q-toggle
          v-model="togglesValues.hr.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
      <div class="row">
        <div class="info col-7">Czy prowadzisz gospodarkę magazynową?</div>
        <div class="col-1">
          <InfoMark
            description="Opłata obejmuje dostęp do modułu magazynowego. Obsługa dokumentów magazynowych płatna odrębnie według cennika."
          />
        </div>
        <q-toggle
          v-model="togglesValues.store.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
      <div class="row">
        <div class="info col-7">Czy chcesz skorzystać z oferty rabatowej "Klient online 1.0"?</div>
        <div class="col-1">
          <InfoMark
            description="Klient samodzielnie pobiera informacje o kwotach zobowiązań podatkowych i ZUS z systemu."
          />
        </div>
        <q-toggle
          v-model="togglesValues.sale.status"
          class="col-1"
          color="orange"
          @input="$emit('switch')"
        />
      </div>
    </div>
  </div>
</template>

<script>
import InfoMark from './InfoMark';

export default {
  name: 'Toggles',
  components: {
    InfoMark,
  },
  props: {
    chosen_evidence_type: {
      required: true,
      type: Number,
    },
  },
  data() {
    return {
      togglesValues: {
        vat: { status: false, values: [25, 25] },
        zus: { status: false, values: [10, 10] },
        pit: { status: false, values: [3.96, 5.54] },
        sell_service: { status: false, values: [15, 15] },
        hr: { status: false, values: [10, 10] },
        store: { status: false, values: [15, 15] },
        sale: { status: true, values: [-16.26, -16.26] },
      },
    };
  },
  methods: {
    getTotalValue() {
      let sum = 0;
      Object.keys(this.togglesValues).forEach((el) => {
        if (this.togglesValues[el].status)
          sum += this.togglesValues[el].values[this.chosen_evidence_type];
      });
      return sum;
    },
    isVatActive() {
      return this.togglesValues.vat.status;
    },
  },
};
</script>

<style lang="scss" scoped>
.title {
  font-weight: 600;
  font-size: 16px;
}
.toggles {
  margin: 10px auto;
}
.row {
  align-items: center;
  margin-left: 30px;
}
</style>
