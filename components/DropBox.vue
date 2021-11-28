<template>
  <div class="container">
    <select v-model="province">
      <option v-for="province in provincesItem" :key="province.id" :value="province.id">{{ province.name }}</option>
    </select>
    <select v-model="city">
      <option v-for="city in citiesItem" :key="city.id" :value="city.id">{{ city.name }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "DropBox",
  data() {
    return {
      province: '',
      city: '',
      gps: '',
      provinceArray: [],
      citiesArray: []
    }
  },
  computed: {
    name() {
      return this.data
    },
    provincesItem() {
      if (this.gps) {
        this.province = this.provinceArray.find(pro => pro.nameEN === this.gps).id
      }
      return this.provinceArray
    },
    citiesItem() {
      return this.citiesArray.filter(city => city.province_id === this.province)
    }
  },
  mounted() {
    this.provinceArray = require('@/static/provinces.json')
    this.citiesArray = require('@/static/cities.json')
    navigator.geolocation.getCurrentPosition(this.success, this.gpsError, options);
    const options = {
      enableHighAccuracy: true,
      timeout: 5000,
      maximumAge: 0
    };
  },
  methods: {
    success(pos) {
      const token = 'pk.f4fabe5ca406ac6668c2a26520b328e7'
      const lat = pos.coords.latitude.toString();
      const lng = pos.coords.longitude.toString();
      // const lat = '35.7029075622558';
      // const lng = '51.3497581481933';
      const url = `https://us1.locationiq.com/v1/reverse.php?format=json&key=${token}&lat=${lat}&lon=${lng}`
      fetch(url)
        .then((resp) => {
          if (!resp.ok) throw new Error(resp.statusText);
          return resp.json();
        })
        .then((data) => {
          console.log(data.address.state?.replace('Province', ' ').trim() || data.address.province?.replace('Province', ' ').trim())
          this.gps = data.address.state?.replace('Province', ' ').trim() || data.address.province?.replace('Province', ' ').trim()
        })
        .catch((err) => {
          console.error(err);
        });
    },
    gpsError(err) {
      console.error(`ERROR(${err.code}): ${err.message}`);
    }
  },
}
</script>

<style scoped>
.container {
  max-width: 500px;
  width: 100%;
  margin: 5rem auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  direction: rtl;
}

select {
  width: 200px;
  height: 40px;
}
</style>
