<template>
  <div
    ref="printContainer"
    v-show="isOpen"
    class="fixed inset-0 bg-white text-black z-50 w-full print:w-full print-area"
  >
    <div
      ref="printAreaRef"
      class="p-1 w-full print:w-full"
      style="padding: 5px 0px !important; font-family: sans-serif"
    >
      <!-- Store Info (Full Width) -->
      <div class="text-center mb-[5px]">
        <div class="font-bold center bold" style="font-size: 12px !important">
          {{ bio?.item?.name || 'NAMA TOKO' }}
        </div>
        <div class="center" style="font-size: 12px !important">
          {{ bio?.item?.address || 'ALAMAT TOKO' }}
        </div>
        <div class="center" style="font-size: 12px !important">
          Telp: {{ bio?.item?.phone || '-' }}
        </div>
      </div>

      <hr class="border-dashed border-black my-1 mx-1" />

      <!-- Transaction Info (Left Padded) -->
      <div class="mb-[8px]" style="padding: 0 15px 0 35px !important; font-size: 12px !important">
        <div class="flex justify-between">
          <div>{{ data?.unique_code }}</div>
          <div>{{ data?.cashier?.name || data?.cashier_name }}</div>
        </div>
        <div class="flex justify-between">
          <div>{{ data?.customer?.category || '-' }}</div>
          <div>{{ data?.customer?.name || data?.customer_name }}</div>
        </div>
      </div>

      <hr class="border-dashed border-black my-1 mx-1" />

      <!-- New Header (Left Padded) -->
      <div
        class="flex justify-between font-bold mb-1"
        style="padding: 0 15px 0 35px !important; font-size: 12px !important"
      >
        <div>DETAILS</div>
        <div>HARGA</div>
      </div>
      <hr class="border-dashed border-black my-1 mx-1" />

      <!-- Items (Left Padded) -->
      <div style="padding: 0 15px 0 35px !important; font-size: 12px !important">
        <div v-for="(item, index) in data?.items" :key="index" class="">
          <div class="flex-1">
            <div>{{ item?.product?.name }}</div>
          </div>
          <div class="bold flex justify-between pl-4">
            <div>{{ item?.qty }} X {{ formatRupiah(item?.price || 0) }}</div>
            <div class="">{{ formatRupiah(item?.subtotal || 0) }}</div>
          </div>
        </div>
      </div>

      <hr class="border-dashed border-black my-1 mx-1" />

      <!-- Totals (Left Padded) -->
      <div class="pt-2" style="padding: 0 15px 0 35px !important; font-size: 12px !important">
        <div class="flex justify-between">
          <div class="">TOTAL HARGA</div>
          <div class="">{{ formatRupiah(data?.total) }}</div>
        </div>
        <div class="flex justify-between" v-if="data?.discount">
          <div>Diskon</div>
          <div>{{ formatRupiah(data?.discount) }}</div>
        </div>
        <div class="flex justify-between" v-if="data?.tax">
          <div>Pajak</div>
          <div>{{ formatRupiah(data?.tax) }}</div>
        </div>
        <div class="flex justify-between" v-if="data?.discount || data?.tax">
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

      <hr class="border-dashed border-black my-1 mx-1" />

      <!-- Item Count (Left Padded) -->
      <div
        class="flex justify-between"
        style="padding: 0 15px 0 35px !important; font-size: 12px !important"
      >
        <div>{{ data?.items?.length }} Item</div>
        <div>{{ formatDateIndo(data?.created_at) }} {{ formatTimeIndo(data?.created_at) }}</div>
      </div>

      <hr class="border-dashed border-black my-1 mx-1" />

      <!-- Footer (Left Padded) -->
      <div
        class="text-center mt-[10px]"
        style="padding: 0 15px 0 35px !important; font-size: 12px !important"
      >
        <div class="center">~ Terima Kasih ~</div>
        <div class="center">Barang yang sudah dibeli</div>
        <div class="center">tidak dapat dikembalikan lagi</div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref, computed, watchEffect, nextTick, onMounted } from 'vue'
  import { formatDateIndo, formatTimeIndo } from '@/utils/dateHelper'
  import { useCompanyStore } from '@/stores/settings/company'
  import { formatRupiah } from '@/utils/uangHelper'
  import { printHtmlElement } from '@/utils/printing'

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
