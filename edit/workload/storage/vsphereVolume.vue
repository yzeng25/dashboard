<script>
import LabeledInput from '@/components/form/LabeledInput';
import { mapGetters } from 'vuex';

export default {
  components: { LabeledInput },

  props:      {
    value: {
      type:    Object,
      default: () => {
        return {};
      }
    },
    mode: {
      type:    String,
      default: 'create'
    }
  },

  computed: { ...mapGetters({ t: 'i18n/t' }) },

};
</script>

<template>
  <div>
    <div class="row mb-10">
      <div class="col span-6">
        <LabeledInput v-model="value.vsphereVolume.storagePolicyID" :mode="mode" :label="t('workload.storage.csi.storagePolicyID')" />
      </div>
      <div class="col span-6">
        <LabeledInput v-model.number="value.vsphereVolume.storagePolicyName" :mode="mode" :label="t('workload.storage.csi.storagePolicyName')" />
      </div>
    </div>
    <div class="row">
      <div class="col span-6">
        <LabeledInput v-model="value.vsphereVolume.volumePath" :mode="mode" :label="t('workload.storage.csi.volumePath')" />
      </div>
    </div>
  </div>
</template>
