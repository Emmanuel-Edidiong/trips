<template>
  <main>
    <section>
      <div class="container-fluid bg-image">
        <div class="overlay px-6 py-6">
          <div class="flex justify-center items-center mt-20 mb-6">
            <div>
              <h1 class="header-text text-center text-white mb-4">Find your next getaway trip!</h1>
              <p class="text-lg text-center text-white pb-6">
                Add multiple destinations, see the world around you in a trip
              </p>
            </div>
          </div>
          <div class="flex mt-6 justify-center items-center">
            <div class="flex items-center justify-center relative">
              <div
                class="max-w-5xl p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 custom-width"
                style="z-index: 99"
              >
                <div class="lg:flex md:flex sm:inline justify-center gap-2">
                  <div class="mt-2">
                    <label>Leaving From</label>
                    <svg
                      class="w-6 h-6 absolute text-gray-900 mt-4 ml-3"
                      aria-hidden="true"
                      xmlns="http://www.w3.org/2000/svg"
                      width="24"
                      height="24"
                      fill="currentColor"
                      viewBox="0 0 24 24"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M11.906 1.994a8.002 8.002 0 0 1 8.09 8.421 7.996 7.996 0 0 1-1.297 3.957.996.996 0 0 1-.133.204l-.108.129c-.178.243-.37.477-.573.699l-5.112 6.224a1 1 0 0 1-1.545 0L5.982 15.26l-.002-.002a18.146 18.146 0 0 1-.309-.38l-.133-.163a.999.999 0 0 1-.13-.202 7.995 7.995 0 0 1 6.498-12.518ZM15 9.997a3 3 0 1 1-5.999 0 3 3 0 0 1 5.999 0Z"
                        clip-rule="evenodd"
                      />
                    </svg>
                    <input
                      type="text"
                      placeholder="Abuja, Nigeria"
                      id="departure"
                      v-model="departure"
                      @input="locationSearchStatus = true; debouncedFetchLocation()"
                      class="bg-gray-50 mt-1 w-300 cus-input border border-gray-300 text-gray-900 text-lg rounded-lg focus:ring-blue-500 focus:border-blue-500 block px-8 py-4 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                      required
                    />
                  </div>
                  <div class="mt-2">
                    <label>Heading To</label>
                    <div v-for="(destination, index) in destinations" :key="index" class="mb-2 items-center gap-2">
                      <svg
                        class="w-6 h-6 absolute text-gray-900 mt-4 ml-3"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                      >
                        <path
                          fill-rule="evenodd"
                          d="M11.906 1.994a8.002 8.002 0 0 1 8.09 8.421 7.996 7.996 0 0 1-1.297 3.957.996.996 0 0 1-.133.204l-.108.129c-.178.243-.37.477-.573.699l-5.112 6.224a1 1 0 0 1-1.545 0L5.982 15.26l-.002-.002a18.146 18.146 0 0 1-.309-.38l-.133-.163a.999.999 0 0 1-.13-.202 7.995 7.995 0 0 1 6.498-12.518ZM15 9.997a3 3 0 1 1-5.999 0 3 3 0 0 1 5.999 0Z"
                          clip-rule="evenodd"
                        />
                      </svg>
                      <input
                        type="text"
                        placeholder="Lagos, Nigeria"
                        id="destination"
                        v-model="destination.name"
                        @input="destinationSearchStatus = true; debouncedFetchDestination(index)"
                        class="bg-gray-50 w-300 mt-1 cus-input border border-gray-300 text-gray-900 text-lg rounded-lg focus:ring-blue-500 focus:border-blue-500 block px-8 py-4 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                        required
                      />
                      <button @click="removeDestination(index)" class="relative text-red-500" :class="index > 0 ? 'flex' : 'hidden'">
                        <svg class="w-6 h-6 lg:absolute sm:mt-2 text-red-500 cus-pad" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                          <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 7h14m-9 3v8m4-8v8M10 3h4a1 1 0 0 1 1 1v3H9V4a1 1 0 0 1 1-1ZM6 7h12v13a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1V7Z"/>
                        </svg>
                      </button>
                      <div :class="destinationIndex > 0 ? 'flex' : 'hidden'">
                        <button @click="moveUp(index)" class="ml-2 mt-2 text-blue-500" :disabled="index === 0" :class="index === 0 ? 'opacity-4' : 'opacity-1'">
                          <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m5 15 7-7 7 7"/>
                          </svg>
                        </button>
                        <button @click="moveDown(index)" class="ml-2 mt-2 text-blue-500" :disabled="index === destinations.length - 1" :class="index === destinations.length - 1 ? 'opacity-4' : 'opacity-1'">
                          <svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 9-7 7-7-7"/>
                          </svg>
                        </button>
                      </div>
                    </div>
                  </div>
                  <div class="mt-2">
                    <div
                      @click="addDestinationField"
                      class="px-6  mt-6 cursor-pointer flex items-center gap-2 justify-center py-4"
                      style="background: #8bc34a; border-radius: 8px; cursor: pointer"
                    >
                      <svg
                        class="w-4 h-4 text-white"
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
                          d="M5 12h14m-7 7V5"
                        />
                      </svg>
                      <span class="text-white fw-700">Add </span>
                    </div>
                  </div>
                  <div class="mt-2">
                    <div
                      @click="destinationIndex !== null ? searchRoutes() : null"
                      class="px-6 w-full mt-6 flex items-center justify-center py-4"
                      style="background: #f44336; border-radius: 8px; cursor: pointer"
                      :class="destinationIndex !== null ? 'opacity-1' : 'opacity-4'"
                    >
                    <div role="status" v-if="loading">
                      <svg
                        aria-hidden="true"
                        class="w-6 h-6 text-gray-200 animate-spin dark:text-gray-600 fill-white"
                        viewBox="0 0 100 101"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <path
                          d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                          fill="currentColor"
                        />
                        <path
                          d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                          fill="currentFill"
                        />
                      </svg>
                      <span class="sr-only">Loading...</span>
                    </div>
                    <div class="flex items-center justify-center" v-else>
                      <span class="text-white fw-700">Let's Go </span>
                      <svg
                        class="w-6 h-6 text-white dark:text-white"
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
                      </div>
                    </div>
                  </div>
                </div>
                <div class="ml-2"></div>
              </div>
            </div>
          </div>
          <div class="flex justify-center" v-if="locationSearchStatus || destinationSearchStatus">
            <div
              class="max-w-5xl -mt-1 p-6 bg-white border border-gray-200 rounded dark:bg-gray-800 dark:border-gray-700 custom-width"
              style="z-index: 99"
            >
              <div role="status" v-if="searchLoading">
                <svg
                  aria-hidden="true"
                  class="w-8 h-8 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
                  viewBox="0 0 100 101"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                    fill="currentColor"
                  />
                  <path
                    d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                    fill="currentFill"
                  />
                </svg>
                <span class="sr-only">Loading...</span>
              </div>
              <ul
                class="py-2 text-sm text-gray-700 dark:text-gray-200"
                aria-labelledby="states-button"
                v-else
              >
                <li v-for="location in locations" :key="location.city_code">
                  <button
                    type="button"
                    @click="locationSearchStatus == true ? addLocation(location.name, location.country_code, location.city_code) : destinationSearchStatus == true ? addDestination(location.name, location.country_code, location.city_code) : null"
                    class="flex items-center justify-between w-full rounded-lg px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-600 dark:hover:text-white"
                  >
                    <div class="flex items-center gap-2">
                      <svg
                        class="w-6 h-6 text-gray-900"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        width="24"
                        height="24"
                        fill="currentColor"
                        viewBox="0 0 24 24"
                      >
                        <path
                          fill-rule="evenodd"
                          d="M11.906 1.994a8.002 8.002 0 0 1 8.09 8.421 7.996 7.996 0 0 1-1.297 3.957.996.996 0 0 1-.133.204l-.108.129c-.178.243-.37.477-.573.699l-5.112 6.224a1 1 0 0 1-1.545 0L5.982 15.26l-.002-.002a18.146 18.146 0 0 1-.309-.38l-.133-.163a.999.999 0 0 1-.13-.202 7.995 7.995 0 0 1 6.498-12.518ZM15 9.997a3 3 0 1 1-5.999 0 3 3 0 0 1 5.999 0Z"
                          clip-rule="evenodd"
                        />
                      </svg>
                      <p class="text-lg">{{ location.name }}, {{ location.country_code }}</p>
                    </div>
                    <div class="flex items-center">
                      <p class="text-lg">{{ location.city_code }}</p>
                    </div>
                  </button>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </section>
    <div class="fh5co-loader" id="reloader" style="display: none">
      <div class="loader"></div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { ref, watch  } from 'vue'
import moment from 'moment'
import { useRouter } from 'vue-router'
import debounce from 'lodash/debounce';

const router = useRouter();
const departure = ref('')
const destinations =  ref([{ name: '' }]);
const arrCode = ref('')
const cityCodes = ref([])
const loading = ref(false)
const searchLoading = ref(false)
const locations = ref([])
const locationSearchStatus = ref(false)
const destinationSearchStatus = ref(false)
const destinationIndex = ref(null)

const fetchLocation = async () => {
  try {

    if (departure.value.trim() === '') {
      locations.value = [];
      locationSearchStatus.value = false
      return;
    }

    searchLoading.value = true
    locationSearchStatus.value = true

    const response = await fetch(
      `${import.meta.env.VITE_APP_MAP_URL}=${departure.value}&api_key=${import.meta.env.VITE_APP_MAPS_API_KEY}`
    )
    const json = await response.json()

    locations.value = json?.response?.cities || []

    searchLoading.value = false
  } catch (error) {
    console.error('Error fetching location data:', error)

    searchLoading.value = false
  }
}

const fetchDestination = async (index) => {
  try {

    if (destinations.value[index].name.trim() === '') {
      locations.value = [];
      destinationIndex.value = null
      destinationSearchStatus.value = false;
      return;
    }

    searchLoading.value = true
    destinationSearchStatus.value = true

    destinationIndex.value = index

    const response = await fetch(
      `${import.meta.env.VITE_APP_MAP_URL}=${destinations.value[index].name}&api_key=${import.meta.env.VITE_APP_MAPS_API_KEY}`
    )
    const json = await response.json()

    locations.value = json?.response?.cities || []

    searchLoading.value = false
  } catch (error) {
    console.error('Error fetching location data:', error)

    searchLoading.value = false
  }
}

const searchRoutes = async () => {

  document.getElementById('reloader').style.display = 'block';

  try {

    const newTrips = [];

    loading.value = true
    localStorage.setItem('arrCode', arrCode.value);
    localStorage.setItem('cityCodes', JSON.stringify(cityCodes.value));

    const response = await fetch(
      `${import.meta.env.VITE_APP_DEP_URL}=${arrCode.value}&api_key=${import.meta.env.VITE_APP_MAPS_API_KEY}`
    );
    const routes = await response.json();

    const filteredRoutes = await routes.response.filter((route: { arr_iata: string | never[]; status: string | string[]; }) => {
      return route.arr_iata.includes(cityCodes.value[0])
    });

    newTrips.push(filteredRoutes[0])

    console.log(cityCodes.value.length)
    console.log(cityCodes.value)

    if (cityCodes.value.length > 1) {

      for (let i = 0; i < cityCodes.value.length; i++) {

        console.log([cityCodes.value[i], cityCodes.value[i + 1]])

        const newResponse = await fetch(
          `${import.meta.env.VITE_APP_DEP_URL}=${cityCodes.value[i]}&api_key=${import.meta.env.VITE_APP_MAPS_API_KEY}`
        );
        const newRoutes = await newResponse.json();

        const newFilteredRoutes = await newRoutes.response.filter((route: { arr_iata: string | never[]; status: string | string[]; }) => {
          return route.arr_iata.includes(cityCodes.value[i + 1])
        });

        newTrips.push(newFilteredRoutes[0])
      }

    }

    console.log(newTrips)

    localStorage.setItem('trips', JSON.stringify(newTrips));

    router.push('/trips');


  } catch (err) {

    loading.value = false

    console.error('Error fetching destination routes:', err)
  }
}

const addLocation = (city: any, countryCode: any, cityCode: string) => {
  departure.value = `${city}, ${countryCode}`;
  locationSearchStatus.value = false;

  arrCode.value = cityCode
}

const addDestination = (city: any, countryCode: any, cityCode: any) => {

  destinations.value[destinationIndex.value].name = `${city}, ${countryCode}`;

  destinationSearchStatus.value = false;

  cityCodes.value.push(cityCode)

};

const addDestinationField = () => {
  destinations.value.push({ name: '' });
};

const removeDestination = (index: number) => {
  destinations.value.splice(index, 1);
};

const moveUp = (index: number) => {
  if (index > 0) {
    const temp = destinations.value[index - 1];
    destinations.value[index - 1] = destinations.value[index];
    destinations.value[index] = temp;
  }
};

const moveDown = (index: number) => {
  if (index < destinations.value.length - 1) {
    const temp = destinations.value[index + 1];
    destinations.value[index + 1] = destinations.value[index];
    destinations.value[index] = temp;
  }
};

const debouncedFetchLocation = debounce(fetchLocation, 300);
const debouncedFetchDestination = debounce(fetchDestination, 300);

watch(departure, (newValue, oldValue) => {
  debouncedFetchLocation();
});

watch(destinations, () => {
  destinations.value.forEach((_, index) => {
    debouncedFetchDestination(index);
  });
}, { deep: true });

</script>: number(: { arr_iata: string | never[]; status: string | string[]; })(: { arr_iata: string | never[]; status: string | string[]; }): any: any: string: any: any: any
