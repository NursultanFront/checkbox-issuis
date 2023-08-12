<template>
  <div class="block">
    <h3 class="block__title">И привлеките больше гостей 🎁</h3>
    <p class="block__text">Предложите скидки своим гостям.Так бронировать будут чаще и быстрее</p>
    <ul class="block__discount discount">
      <li v-for="item in discountTypes" :key="item.value">
        <UiCheckbox
          :id="item.id"
          v-model="item.isChecked"
          name="discount"
          :label="item.labelText"
        />
      </li>
      <li class="discount__custom">
        <div class="discount__content">
          <UiCheckbox id="custom" v-model="isCustomCheck" name="discount" />
          <span class="discount__text">Свой вариант:</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'
import UiCheckbox from '@/components/TheCheckbox.vue'

export type Price = {
  price: number
  monthly_sale: number
  weekly_sale: number
  custom_sale: number
  custom_days: number
  custom_percent: number
}

enum DiscountTypeId {
  TEN = 'ten',
  THIRTY = 'thirty'
}

const isCustomCheck = ref(false)

const discountTypes = ref([
  {
    id: 'five',
    isChecked: false,
    value: 5,
    labelText: 'Скидка 5% первым трем гостям'
  },
  {
    id: DiscountTypeId.TEN,
    isChecked: false,
    value: 10,
    labelText: 'Скидка 10% от 7 дней бронирования'
  },
  {
    id: DiscountTypeId.THIRTY,
    isChecked: false,
    value: 30,
    labelText: 'Скидка 30% от 30 дней бронирования'
  }
])

const resetDiscount = () => {
  for (const item of discountTypes.value) {
    item.isChecked = false
  }
}

const resetCustom = () => {
  isCustomCheck.value = false
}

watch(
  () => discountTypes.value,
  (newVal) => {
    newVal.forEach((_) => {
      resetCustom()
    })
  },
  { deep: true }
)

watch(
  () => isCustomCheck.value,
  () => {
    if (isCustomCheck.value === true) {
      resetDiscount()
    }
  }
)
</script>

<style scoped lang="scss">
.block {
  &__title {
    margin-bottom: 8px;

    color: black;

    line-height: 140%;

    &--mobile {
      display: none;
      color: black;
    }
  }

  &__text {
    color: black;

    line-height: 140%;
  }
}

.discount {
  display: flex;
  flex-direction: column;
  gap: 16px;

  &__item {
    display: flex;
    align-items: center;
  }

  &__content {
    display: flex;
    align-items: center;
  }

  &__custom {
    display: flex;
    gap: 10px;
  }

  &__text {
    color: black;
    line-height: 150%;
  }

  &__percent {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  &__select {
    text-wrap: nowrap;
  }

  &__input {
    max-width: 100px;
    text-align: center;
  }
}
</style>
