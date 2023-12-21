<template>
  <div class="g-timeaxis">
    <!-- <div class="g-timeunits-container">
      <div
        v-for="({ label, value, date, width }, index) in timeaxisUnits.upperUnits"
        :key="label"
        class="g-upper-timeunit"
        :style="{
          background: index % 2 === 0 ? colors.primary : colors.secondary,
          color: colors.text,
          width
        }"
      >
        <slot name="upper-timeunit" :label="label" :value="value" :date="date">
          {{ label }}
        </slot>
      </div>
    </div> -->

    <div class="g-timeunits-container">
      <div
        v-for="({ label, value, date, width }, index) in timeaxisUnits.lowerUnits"
        :key="label"
        class="g-timeunit"
        :style="{
          background: index % 2 === 0 ? colors.ternary : colors.quartenary,
          color: colors.text,
          flexDirection: precision === 'hour' ? 'column' : 'row',
          alignItems: precision === 'hour' ? '' : 'center',
          width
        }"
      >
        <slot name="timeunit" :label="label" :value="value" :date="date">
          {{ label }}
        </slot>
        <div
          v-if="precision === 'hour'"
          class="g-timeaxis-hour-pin"
          :style="{ background: colors.text }"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import provideConfig from "../provider/provideConfig.js"
import useTimeaxisUnits from "../composables/useTimeaxisUnits.js"

const { precision, colors } = provideConfig()
const { timeaxisUnits } = useTimeaxisUnits()
</script>

<style>
.g-timeaxis {
  position: sticky;
  top: 0;
  width: 100%;
  height: 45px;
  min-height: 45px;
  background: white;
  z-index: 4;
  /* box-shadow: 0px 1px 3px 2px rgba(50, 50, 50, 0.5); */
  display: flex;
  flex-direction: column;
  font-family: Energy;
  font-size: 12px;
  font-weight: 400;
  line-height: 16px;
  letter-spacing: 0em;
  text-align: center;

  color: #000000de !important;
}

.g-timeunits-container {
  display: flex;
  width: 100%;
  height: 100%;
}

.g-timeunit {
  height: 100%;
  font-size: 65%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-left: 1px solid #ebebef;
  font-family: Energy;
  font-size: 10px;
  font-weight: 400;
  line-height: 13px;
  letter-spacing: 0em;
  text-align: center;
  color: #4c6172;
}

.g-upper-timeunit {
  display: flex;
  height: 100%;
  justify-content: center;
  align-items: center;
  border-left: 1px solid #ebebef;
  font-family: Energy;
  font-size: 12px;
  font-weight: 400;
  line-height: 16px;
  letter-spacing: 0em;
  text-align: center;
  color: #4c6172;
}

.g-timeaxis-hour-pin {
  width: 1px;
  height: 10px;
}
</style>
