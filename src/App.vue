<template>
  <div id="label-mockup" ref="label-mockup">
    <barcode :id="'vendor'" :value="'GENTRY HARDWARE'" />
    <barcode :id="'model'" :value="'GT-9403D-4H-KD'" />
    <barcode :id="'lot-number'" :value="lotNumber" />
    <div>GENTRY HARDWARE PRODUCTS CO., LTD.</div>
    <div>PRODUCT: ALUM. IV STAND</div>
    <div>REF: GT-9403D-4H-KD</div>
    <div v-if="!!lotNumber">{{ `LOT: ${lotNumber}` }}</div>
    <div v-else class="warning">LOT: PENDING</div>
    <br />
    <div>NO SPECIAL STORAGE REQUIREMENTS</div>
    <div>NO EXPIRATION DATE</div>
  </div>

  <div id="control-wrapper">
    <div id="input-wrapper">
      <input
        v-model="lotNumber"
        id="lot-number-input"
        type="text"
        placeholder="ENTER LOT NUMBER"
        class="py-1 px-2"
      />
    </div>
    <div v-if="!!lotNumber" id="button-wrapper">
      <button id="save-button" @click="saveLabel">SAVE LABEL</button>
    </div>
  </div>
</template>

<script>
import htmlToImage from "html-to-image";
import FileSaver from "file-saver";

import Barcode from "./components/Barcode.vue";

export default {
  name: "App",
  components: { Barcode },
  data() {
    return { lotNumber: null };
  },
  methods: {
    async saveLabel() {
      const element = this.$refs["label-mockup"];
      const blob = await htmlToImage.toBlob(element);
      FileSaver.saveAs(blob, `${this.lotNumber}.png`);
    },
  },
};
</script>

<style lang="postcss">
#label-mockup {
  @apply p-4;
  @apply w-max-content;
  @apply border border-solid border-black;
  @apply bg-white;
}

.warning {
  @apply text-red-600;
}

#control-wrapper {
  @apply mt-2;
  @apply flex flex-wrap;
}

#input-wrapper {
  @apply py-2 w-full max-w-lg;
}

#lot-number-input {
  @apply shadow appearance-none border rounded;
  @apply text-gray-700 leading-tight;
}

#lot-input:focus {
  @apply outline-none shadow-outline;
}

#button-wrapper {
  @apply py-2 w-full;
}

#save-button {
  @apply bg-blue-500;
  @apply rounded;
  @apply text-white font-bold;
  @apply py-2 px-4;
}

#save-button:hover {
  @apply bg-blue-700;
}

#save-button:focus {
  @apply outline-none;
}
</style>
