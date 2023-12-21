<template>
  <div>
    <button type="button" @click="zoomIn">Zoom In</button>
  </div>
  <div>
    <button type="button" @click="zoomOut">Zoom Out</button>
  </div>

  <g-gantt-chart
    :chart-start="chartStart"
    :chart-end="chartEnd"
    :precision="computedPrecision"
    :row-height="30"
    grid
    width="100%"
    bar-start="beginDate"
    bar-end="endDate"
    :date-format="format"
    color-scheme="reckitt"
    @click-bar="onClickBar($event.bar, $event.e, $event.datetime)"
    @mousedown-bar="onMousedownBar($event.bar, $event.e, $event.datetime)"
    @dblclick-bar="onMouseupBar($event.bar, $event.e, $event.datetime)"
    @mouseenter-bar="onMouseenterBar($event.bar, $event.e)"
    @mouseleave-bar="onMouseleaveBar($event.bar, $event.e)"
    @dragstart-bar="onDragstartBar($event.bar, $event.e)"
    @drag-bar="onDragBar($event.bar, $event.e)"
    @dragend-bar="onDragendBar($event.bar, $event.e, $event.movedBars)"
    @contextmenu-bar="onContextmenuBar($event.bar, $event.e, $event.datetime)"
  >
    <div v-for="row in rows">
      <g-gantt-row label="My row 1" :bars="row.bars" highlight-on-hover />
    </div>
    <template v-slot:upper-timeunit="slotProps">
      <!-- {{ dayjs(slotProps.date).format("MMMM") }} -->
    </template>
    <template v-slot:timeunit="slotProps">
      <div>W{{ slotProps.value }}</div>

      <div>{{ dayjs(slotProps.date).format("DD") }}</div>

      <!-- <div>{{ slotProps  }}</div> -->
    </template>
  </g-gantt-chart>

  <!-- <button type="button" @click="addBar()">Add bar</button> -->
  <!-- <button type="button" @click="deleteBar()">Delete bar</button> -->
</template>

<script setup lang="ts">
import dayjs from "dayjs"
import { ref, computed } from "vue"
import type { GanttBarObject } from "./types.js"

const chartStart = ref("01.01.2023")
const chartEnd = ref("31.12.2023")
const format = ref("DD.MM.YYYY")
const zoomLevel = ref(1) // Initial zoom level

const zoomIn = () => {
  zoomLevel.value = 1.1 // You can adjust the zoom increment as needed
  updateChartZoom()
}

const zoomOut = () => {
  zoomLevel.value = 0.9 // You can adjust the zoom decrement as needed
  updateChartZoom()
}

const updateChartZoom = () => {
  console.log("zoomLevel", zoomLevel.value)
  const initialChartStart = dayjs(chartStart.value, format.value)
  const initialChartEnd = dayjs(chartEnd.value, format.value)

  // Calculate the new duration based on the zoom level
  const newDuration = initialChartEnd.diff(initialChartStart)
  const zoomedDuration = newDuration / zoomLevel.value

  // Calculate the new chartEnd based on the initial chartStart and zoomed duration
  const newChartEnd = initialChartStart.add(zoomedDuration)

  // Update chartStart and chartEnd with the formatted dates
  chartStart.value = initialChartStart.format(format.value)
  chartEnd.value = newChartEnd.format(format.value)
}

const rows = ref([
  {
    label: "Promotion 1",
    bars: [
      {
        beginDate: "01.02.2023",
        endDate: "25.05.2023",
        ganttBarConfig: {
          id: "8621987320",
          label: "Promotion 1",
          hasHandles: true,
          className: "promotion-1",
          style: {
            borderRadius: "5px",
            color: "blue",
            fontSize: "10px"
          }
        }
      }
    ]
  },
  {
    label: "Promotion 2",
    bars: [
      {
        beginDate: "10.02.2023",
        endDate: "01.04.2023",
        ganttBarConfig: {
          id: "8621987321",
          label: "Promotion 2",
          hasHandles: true,
          style: {
            borderRadius: "5px",
            color: "blue",
            fontSize: "10px"
          }
        }
      }
    ]
  },
  {
    label: "Promotion 3",
    bars: [
      {
        beginDate: "20.02.2023",
        endDate: "20.05.2023",
        ganttBarConfig: {
          id: "8621987322",
          label: "Promotion 3",
          hasHandles: true,
          style: {
            borderRadius: "5px",
            color: "blue",
            fontSize: "10px"
          }
        }
      }
    ]
  },
  {
    label: "Promotion 4",
    bars: [
      {
        beginDate: "20.02.2023",
        endDate: "25.05.2023",
        ganttBarConfig: {
          id: "8621987323",
          label: "Promotion 4",
          hasHandles: true,
          style: {
            borderRadius: "5px",
            color: "blue",
            fontSize: "10px"
          }
        }
      }
    ]
  },
  {
    label: "Promotion 5",
    bars: [
      {
        beginDate: "01.04.2023",
        endDate: "25.08.2023",
        ganttBarConfig: {
          id: "8621987324",
          label: "Promotion 5",
          hasHandles: true,
          style: {
            borderRadius: "5px",
            color: "blue",
            fontSize: "10px"
          }
        }
      }
    ]
  },
  {
    label: "Promotion 6",
    bars: [
      {
        beginDate: "24.04.2023",
        endDate: "25.10.2023",
        ganttBarConfig: {
          id: "8621987325",
          label: "Promotion 6",
          hasHandles: true,
          style: {
            borderRadius: "5px",
            color: "blue",
            fontSize: "10px"
          }
        }
      }
    ]
  }
])

function addSelectedById(barId) {
  for (const promotion of rows) {
    for (const bar of promotion.bars) {
      if (bar.ganttBarConfig.id === barId) {
        bar.selected = true
      }
    }
  }
  return ganttData
}

const addBar = () => {
  if (bars1.value.some((bar) => bar.ganttBarConfig.id === "test1")) {
    return
  }
  const bar = {
    beginDate: "26.02.2023",
    endDate: "26.03.2023",
    ganttBarConfig: {
      id: "test1",
      hasHandles: true,
      label: "Hello!",
      style: {
        background: "#5484b7",
        borderRadius: "20px"
      }
    }
  }
  bars1.value.push(bar)
}

const deleteBar = () => {
  const idx = bars1.value.findIndex((b) => b.ganttBarConfig.id === "test1")
  if (idx !== -1) {
    bars1.value.splice(idx, 1)
  }
}

const onClickBar = (bar: GanttBarObject, e: MouseEvent, datetime?: string) => {
  console.log("click-bar", bar, e, datetime)
}

const onMousedownBar = (bar: GanttBarObject, e: MouseEvent, datetime?: string) => {
  console.log("mousedown-bar", bar, e, datetime)
}

const onMouseupBar = (bar: GanttBarObject, e: MouseEvent, datetime?: string) => {
  console.log("mouseup-bar", bar, e, datetime)
}

const onMouseenterBar = (bar: GanttBarObject, e: MouseEvent) => {
  console.log("mouseenter-bar", bar, e)
}

const onMouseleaveBar = (bar: GanttBarObject, e: MouseEvent) => {
  console.log("mouseleave-bar", bar, e)
}

const onDragstartBar = (bar: GanttBarObject, e: MouseEvent) => {
  console.log("dragstart-bar", bar, e)
}

const onDragBar = (bar: GanttBarObject, e: MouseEvent) => {
  console.log("drag-bar", bar, e)
}

const onDragendBar = (
  bar: GanttBarObject,
  e: MouseEvent,
  movedBars?: Map<GanttBarObject, { oldStart: string; oldEnd: string }>
) => {
  console.log("dragend-bar", bar, e, movedBars)
  // convert oldStartTimestamp to Date
  // convert oldEndTimestamp to Date
  // bar.beginDate = oldStart
}

const computedPrecision = computed(() => {
  return "week"
  const intervalDays = dayjs(chartEnd.value, format.value).diff(
    dayjs(chartStart.value, format.value),
    "days"
  )
  return intervalDays <= 60 ? "day" : "month"
})

const onContextmenuBar = (bar: GanttBarObject, e: MouseEvent, datetime?: string) => {
  console.log("contextmenu-bar", bar, e, datetime)
}
</script>
