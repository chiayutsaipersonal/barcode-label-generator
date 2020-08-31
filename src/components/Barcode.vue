<template>
  <img v-if="!!value" :id="id" class="barcode" />
  <div v-else class="warning">BARCODE VALUE MISSING</div>
</template>

<script>
import JsBarcode from "jsbarcode";

export default {
  name: "Barcode",
  props: {
    id: { type: String, required: true },
    value: { type: String, default: () => null },
  },
  watch: {
    value() {
      if (!!this.value) {
        this.$nextTick(() => {
          this.render();
        });
      }
    },
  },
  mounted() {
    if (!!this.value) {
      this.render();
    }
  },
  methods: {
    render() {
      const options = { fontSize: 20, textMargin: 0 };
      JsBarcode(`#${this.id}`)
        .options({
          margin: 0,
          marginBottom: 20,
          height: 60,
          font: "monospace",
          fontOptions: "bold",
          textAlign: "left",
        })
        .code128(this.value, options)
        .render();
    },
  },
};
</script>