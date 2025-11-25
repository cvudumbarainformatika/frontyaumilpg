<template>
  <div ref="printContainer" v-show="isOpen"
    class="fixed inset-0 bg-white text-black z-50 w-full print:w-full  print-area">
    <div ref="printAreaRef" class="p-1 w-full print:w-full" style="padding-right: 20px !important;">
      <div class="text-center mb-[5px]">
        <div class="font-bold center bold" style="font-size: 10pt !important">{{ bio?.item?.name || 'NAMA TOKO' }}</div>
        <div class="center">{{ bio?.item?.address || 'ALAMAT TOKO' }}</div>
        <div class="center">Telp: {{ bio?.item?.phone || '-' }}</div>
        <hr class="border-dashed border-gray-400">
      </div>

      <div class="mb-[8px]">
        <div class="flex justify-between">
          <div>{{ data?.unique_code }}</div>
          <div>{{ data?.cashier?.name || data?.cashier_name }}</div>
          <!-- <div>{{ formatDateIndo(data?.created_at) }}</div> -->
        </div>
        <div class="flex justify-between text-[8px]">
          <div> {{ data?.customer?.category || '-' }}</div>
          <div>{{ data?.customer_name }}</div>
        </div>
        <hr class="border-dashed border-gray-400">
      </div>
      <div v-for="(item, index) in data?.items" :key="index" class="">
        <div class="flex-1">
          <div >{{ item?.product?.name }}</div>
        </div>
        <div class="bold flex justify-between">
          <div style="margin-left:20px;">  {{ item?.qty }} X {{ formatRupiah(item?.price || 0) }}</div>
          <div class="">{{ formatRupiah(item?.subtotal || 0) }}</div>
        </div>
      </div>


      <!-- <hr class="border-dashed border-gray-400"> -->
      <!-- <div class="flex">
        item: {{ data?.items?.length }}
      </div> -->
      <hr class="border-dashed border-gray-400">
      <div class="border-t border-dashed border-gray-400 pt-2 text-[8px]"
        style="margin-left:25px;"
      >
        <div class="flex justify-between">
          <div class="">TOTAL HARGA</div>
          <div class="">{{ formatRupiah(data?.total) }}</div>
        </div>
        <div class="flex justify-between">
          <div>Diskon</div>
          <div>{{ formatRupiah(data?.discount) }}</div>
        </div>
        <div class="flex justify-between">
          <div>Pajak</div>
          <div>{{ formatRupiah(data?.tax) }}</div>
        </div>
        <div class="flex justify-between">
          <div>Total</div>
          <div>{{ formatRupiah(data?.total) }}</div>
        </div>
        <div class="flex justify-between mt-[8px]">
          <div>BAYAR</div>
          <div>{{ formatRupiah(data?.bayar) }}</div>
        </div>
        <div class="flex justify-between">
          <div>KEMBALI</div>
          <div>{{ formatRupiah(data?.kembali) }}</div>
        </div>
      </div>
      <hr class="border-dashed border-gray-400">
       <div class="flex justify-between">
        <div>{{ data?.items?.length }} Item</div>
        <div>{{ formatDateIndo(data?.created_at) }} {{ formatTimeIndo(data?.created_at) }}</div>
      </div>
      <hr class="border-dashed border-gray-400">
      <div class="text-center text-[8px] mt-[20px]">
        <div class=" center">~ Terima Kasih ~</div>
        <div class="center">Barang yang sudah dibeli</div>
        <div class="center">tidak dapat dikembalikan</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watchEffect, nextTick, onMounted } from 'vue'
import { formatDateIndo, formatTimeIndo } from '@/utils/dateHelper'
import { useCompanyStore } from '@/stores/settings/company'
import { formatRupiah } from '@/utils/uangHelper'
import { printHtmlElement } from '@/utils/printing';

const bio = useCompanyStore()
// const format = (value) => {
//   return formatRupiah(value)
// }

const props = defineProps({
  data: {
    type: Object,
    default: null
  },
  modelValue: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:modelValue'])

const isOpen = computed({
  get: () => props.modelValue,
  set: (value) => {
    emit('update:modelValue', value)
  }
})

const printAreaRef = ref(null)

defineExpose({ printAreaRef })

onMounted(() => {
  console.log('🟢 Komponen Struk ter-mount', props?.data)
})

</script>


<style scoped>
body * {
  visibility: hidden;
}

.print-area,
.print-area * {
  visibility: visible;
}
</style>

<style></style>
