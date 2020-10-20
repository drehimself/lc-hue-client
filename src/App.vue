<template>
  <div class="container px-4 my-8 mx-auto text-gray-800">
    <div>
      <h3 class="text-lg leading-6 font-medium text-gray-900">
        Dre's Lights
      </h3>
      <p class="mt-1 text-sm leading-5 text-gray-500">
        Use these buttons to control Dre's lights!
      </p>
    </div>
    <div class="mt-6 grid grid-cols-1 gap-y-6 gap-x-4 sm:grid-cols-6">
      <div class="sm:col-span-4">
        <div class="block text-sm font-medium leading-5 text-gray-700">
          Turn On/Off
        </div>
        <div class="flex mt-2">
          <span
            @click="toggleLights"
            role="checkbox"
            tabindex="0"
            aria-checked="false"
            :class="{ 'bg-blue-400': areLightsOn, 'bg-gray-200' : !areLightsOn }"
            class="relative inline-flex flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:shadow-outline"
          >
            <span
              aria-hidden="true"
              :class="{ 'translate-x-5': areLightsOn, 'translate-x-0' : !areLightsOn }"
              class="translate-x-0 relative inline-block h-5 w-5 rounded-full bg-white shadow transform transition ease-in-out duration-200"
            >
              <span
                :class="{ 'opacity-0 ease-out duration-100': areLightsOn, 'opacity-100 ease-in duration-200' : !areLightsOn }"
                class="absolute inset-0 h-full w-full flex items-center justify-center transition-opacity"
              >
                <svg class="h-3 w-3 text-gray-400" fill="none" viewBox="0 0 12 12">
                  <path d="M4 8l2-2m0 0l2-2M6 6L4 4m2 2l2 2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
              </span>
              <span
                :class="{ 'opacity-100 ease-in duration-200': areLightsOn, 'opacity-0 ease-out duration-100' : !areLightsOn }"
                class="absolute inset-0 h-full w-full flex items-center justify-center transition-opacity"
              >
                <svg class="h-3 w-3 text-blue-400" fill="currentColor" viewBox="0 0 12 12">
                  <path d="M3.707 5.293a1 1 0 00-1.414 1.414l1.414-1.414zM5 8l-.707.707a1 1 0 001.414 0L5 8zm4.707-3.293a1 1 0 00-1.414-1.414l1.414 1.414zm-7.414 2l2 2 1.414-1.414-2-2-1.414 1.414zm3.414 2l4-4-1.414-1.414-4 4 1.414 1.414z" />
                </svg>
              </span>
            </span>
          </span>

          <span class="text-gray-500 ml-6">
            <span v-if="areLightsOn">Lights on!</span>
            <span v-else>Lights off!</span>
          </span>

        </div>
      </div>

      <div class="sm:col-span-6">
        <div class="block text-sm font-medium leading-5 text-gray-700">
          Change Light Colors
        </div>
        <div class="flex space-x-4 mt-4">
          <button @click="changeColor('white')" type="button" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-gray-900 bg-white border-gray-400 hover:bg-gray-200 focus:outline-none focus:border-gray-700 focus:shadow-outline-gray active:bg-gray-200 transition duration-150 ease-in-out">
            White
          </button>
          <button @click="changeColor('red')" type="button" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-red-600 hover:bg-red-500 focus:outline-none focus:border-red-700 focus:shadow-outline-red active:bg-red-700 transition duration-150 ease-in-out">
            Red
          </button>
          <button @click="changeColor('green')" type="button" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-green-600 hover:bg-green-500 focus:outline-none focus:border-green-700 focus:shadow-outline-green active:bg-green-700 transition duration-150 ease-in-out">
            Green
          </button>
          <button @click="changeColor('blue')" type="button" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-blue-600 hover:bg-blue-500 focus:outline-none focus:border-blue-700 focus:shadow-outline-green active:bg-blue-700 transition duration-150 ease-in-out">
            Blue
          </button>
        </div>

        <div v-show="selectedColor" class="text-gray-500 mt-6">
          Color changed to {{ selectedColor }}
        </div>

      </div>

      <div class="sm:col-span-6">
        <div class="block text-sm font-medium leading-5 text-gray-700">
          Blink Lights
        </div>

        <div class="mt-4">
          <button @click="blinkLights" type="button" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-gray-900 bg-white border-gray-400 hover:bg-gray-200 focus:outline-none focus:border-gray-700 focus:shadow-outline-gray active:bg-gray-200 transition duration-150 ease-in-out">
            Blink Lights
          </button>

          <div v-show="areLightsBlinking" class="text-gray-500 mt-6">
            Lights are blinking!
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      areLightsOn: false,
      selectedColor: null,
      areLightsBlinking: false,
      colors: {
        red: {
          h: 65535,
          s: 255,
          b: 20
        },
        white: {
          h: 0,
          s: 0,
          b: 20
        },
        green: {
          h: 26870,
          s: 204,
          b: 20
        },
        blue: {
          h: 44000,
          s: 255,
          b: 20
        }
      }
    }
  },
  methods: {
    toggleLights() {
      this.areLightsOn = !this.areLightsOn

      axios.put('http://192.168.2.23/api/7lA86r1P9LnQKPLhyFiU2sJGIgX54Nn1U0CuIlc3/lights/3/state', {
        on: this.areLightsOn
      }).then(response => {
        //
      }).catch(error => {
        console.log(error.response)
      })
    },
    changeColor(color) {
      this.selectedColor = color
      this.areLightsOn = true

      axios.put('http://192.168.2.23/api/7lA86r1P9LnQKPLhyFiU2sJGIgX54Nn1U0CuIlc3/lights/3/state', {
        on: true,
        hue: this.colors[color].h,
        sat: this.colors[color].s,
        bri: this.colors[color].b,
      }).then(response => {
        //
      }).catch(error => {
        console.log(error.response)
      })
    },
    blinkLights() {
      this.areLightsBlinking = true

      let timesRun = 0
      this.toggleLights()

      let interval = setInterval(() => {
        timesRun++
        if (timesRun === 6) {
          clearInterval(interval)
        }

        this.toggleLights()
      }, 1000);
    }
  }
}
</script>
