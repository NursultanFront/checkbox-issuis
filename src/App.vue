<template>
  <div class="block">
    <h2>Мой вариант</h2>
    <ul class="block__discount discount">
      <li v-for="item in discountTypesMy" :key="item.value">
        <UiCheckbox
          :id="item.id"
          v-model="item.isChecked"
          name="discount"
          :label="item.labelText"
        />
      </li>
      <li class="discount__custom">
        <div class="discount__content">
          <UiCheckbox id="custom" v-model="isCustomCheck" name="discount" label="Свой вариант" />
        </div>
      </li>
    </ul>
  </div>
  <div class="block">
    <h2>Твой вариант</h2>
    <ul class="block__discount discount">
      <li v-for="item in discountTypesYours" :key="item.value">
        <YourCheckbox
          :id="item.id"
          v-model="item.isChecked"
          name="discount"
          :label="item.labelText"
        />
      </li>
      <li class="discount__custom">
        <div class="discount__content">
          <YourCheckbox
            id="customYours"
            v-model="isCustmomYours"
            name="discount"
            label="Свой вариант"
          />
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'
import UiCheckbox from '@/components/TheCheckbox.vue'
import YourCheckbox from '@/components/UiCheckbox.vue'

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

const discountTypesMy = ref([
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

watch(isCustomCheck, (newVal) => {
  if (newVal) {
    discountTypesMy.value.forEach((item) => {
      item.isChecked = false
    })
  }
})

watch(
  discountTypesMy,
  (newVal) => {
    for (const item of newVal) {
      if (item.isChecked) {
        isCustomCheck.value = false
        break
      }
    }
  },
  { deep: true }
)

const isCustmomYours = ref<boolean>(false)

const discountTypesYours = ref([
  {
    id: 'fiveYours',
    isChecked: false,
    value: 5,
    labelText: 'Скидка 5% первым трем гостям'
  },
  {
    id: 'tenYours',
    isChecked: false,
    value: 10,
    labelText: 'Скидка 10% от 7 дней бронирования'
  },
  {
    id: 'thirtyYours',
    isChecked: false,
    value: 30,
    labelText: 'Скидка 30% от 30 дней бронирования'
  }
])

watch(isCustmomYours, (newVal) => {
  if (newVal) {
    discountTypesYours.value.forEach((item) => {
      item.isChecked = false
    })
  }
})

watch(
  discountTypesYours,
  (newVal) => {
    for (const item of newVal) {
      if (item.isChecked) {
        isCustomCheck.value = false
        break
      }
    }
  },
  { deep: true }
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
