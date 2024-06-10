<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <main>
    <section>
      <div class="container-fluid bg-image1">
        <div class="overlay px-6 py-6">
          <div class="flex justify-center items-center mt-20 mb-6">
            <div>
              <h1 class="header-text text-center text-white mb-4">See your current trips!</h1>
              <p class="text-lg text-center text-white pb-6">
                Check your trip duration for each destination and locate your interests
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- trips -->
    <section>
      <div class="container-fluid mx-auto">
        <div class="flex w-full justify-center grid grid-cols-2 sm:grid-cols-1 gap-4">
          <div
            class="p-6 rounded-lg dark:bg-gray-800 dark:border-gray-700"
            style="background: #fef5ff; position: relative"
            v-if="trips.length > 0"
          >
            <div v-for="(trip, index) in trips" :key="index">
              <div v-if="trip !== null">
                <div class="flex items-center justify-center">
                  <div
                    class="max-w-5xl mt-4 p-6 bg-white border-0 rounded-lg dark:bg-gray-800 dark:border-gray-700"
                    style="z-index: 99"
                  >
                    <div class="flex items-center gap-2 justify-between">
                      <h5
                        class="mb-2 text-xl font-bold tracking-tight text-gray-900 dark:text-white"
                      >
                        {{ additionalData[index].dep_city }},
                        {{ additionalData[index]?.dep_country }}
                      </h5>
                      <div class="dotted-line"></div>
                      <svg
                        class="w-6 h-6 text-color"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        fill="none"
                        viewBox="0 0 24 24"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M19 12H5m14 0-4 4m4-4-4-4"
                        />
                      </svg>
                      <h5
                        class="mb-2 text-xl font-bold tracking-tight text-gray-900 dark:text-white"
                      >
                        {{ additionalData[index].arr_city }},
                        {{ additionalData[index]?.arr_country }}
                      </h5>
                    </div>
                    <div class="flex items-center justify-between">
                      <p>Time of Arrival:</p>
                      <p>{{ flightTime(additionalData[index]?.duration) }}</p>
                    </div>
                    <div class="flex items-center justify-between">
                      <p>Date of Departure:</p>
                      <p>{{ formattedDate(additionalData[index]?.dep_time.split(' ')[0]) }}</p>
                    </div>
                    <div class="flex items-center justify-between">
                      <p>Airport of Departure:</p>
                      <p>{{ additionalData[index]?.dep_name }}</p>
                    </div>
                    <div class="flex items-center justify-between">
                      <p>Airline:</p>
                      <p>
                        {{ additionalData[index]?.airline_name }} ({{
                          additionalData[index]?.flight_iata
                        }})
                      </p>
                    </div>
                  </div>
                </div>
                <div v-if="index + 1 < trips.length">
                  <div class="flex items-center py-4 justify-center">
                    <div class="dotted-vertical-line"></div>
                  </div>
                  <div class="flex items-center justify-center">
                    <svg
                      class="w-6 h-6 text-color"
                      aria-hidden="true"
                      xmlns="http://www.w3.org/2000/svg"
                      width="24"
                      height="24"
                      fill="none"
                      viewBox="0 0 24 24"
                    >
                      <path
                        stroke="currentColor"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M12 19V5m0 14-4-4m4 4 4-4"
                      />
                    </svg>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="flex items-center justify-center h-200" v-else>
            <h5 class="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">
              No Trip data found
            </h5>
          </div>
          <div
            class="p-6 rounded-lg dark:bg-gray-800 dark:border-gray-700"
            style="background: #fff6ef; position: relative"
            v-if="trips.length > 0"
          >
            <div class="">
              <div>
                <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">
                  Places of interest
                </h5>
              </div>
              <div class="mt-6 grid grid-cols-4 sm:grid-cols-1" style="gap: 10px">
                <div class="mb-4">
                  <img
                    src="/src/assets/hotel-1.jpg"
                    style="width: 100%; height: 150px; border-radius: 10px; object-fit: cover"
                  />
                </div>
                <div class="mb-4">
                  <img
                    src="/src/assets/hotel-2.jpg"
                    style="width: 100%; height: 150px; border-radius: 10px; object-fit: cover"
                  />
                </div>
                <div class="mb-4">
                  <img
                    src="/src/assets/hotel-3.jpeg"
                    style="width: 100%; height: 150px; border-radius: 10px; object-fit: cover"
                  />
                </div>
                <div class="mb-4">
                  <img
                    src="/src/assets/hotel-4.jpg"
                    style="width: 100%; height: 150px; border-radius: 10px; object-fit: cover"
                  />
                </div>
              </div>
              <div>
                <p class="mt-2 text-gray-900 dark:text-white">Hotels in {{ city }} you can visit</p>
              </div>
            </div>
          </div>
          <div class="flex items-center justify-center h-200" v-else>
            <h5 class="mb-2 text-lg font-bold tracking-tight text-gray-900 dark:text-white">
              No Trip data found
            </h5>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import moment from 'moment'
import { useRoute } from 'vue-router'

const route = useRoute()
const trips = ref([])
const status = ref(false)
const additionalData = ref<any>([])
const city = ref('')

const fetchTrips = async () => {
  const storedTrips = localStorage.getItem('trips')
  if (storedTrips) {
    const parsedTrips = JSON.parse(storedTrips)

    trips.value = parsedTrips.filter((trip) => trip !== null)

    for (let i = 0; i < trips.value.length; i++) {
      await fetchAdditionalData(trips.value[i], i)
    }
  }
}

const fetchAdditionalData = async (trip: any, index: number) => {
  try {
    const response = await fetch(
      `${import.meta.env.VITE_APP_INFO_URL}=${trip.flight_iata}&api_key=${import.meta.env.VITE_APP_MAPS_API_KEY}`
    )
    const json = await response.json()
    additionalData.value[index] = json.response
    city.value = additionalData.value[index].arr_city
  } catch (error) {
    console.error('Error fetching additional data:', error)
  }
}

const formattedDate = (date: moment.MomentInput) => {
  return moment(date).format('MMM DD, YYYY')
}

const flightTime = (time: any) => {
  const totalMinutes = time

  const hours = Math.floor(totalMinutes / 60)
  const minutes = totalMinutes % 60

  const formattedTime = `${hours}hr ${minutes}mins`

  return formattedTime
}

onMounted(() => {
  fetchTrips()
})
</script>
