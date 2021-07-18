<template>
  <div class="container">
    <div class="my-3">
      <button
        type="button"
        class="btn btn-outline-primary"
        @click="setResolution"
      >
        切換大頭貼解析度
      </button>
      現在圖片解析度：
      <span>{{ resolution.text[num] }}</span>
    </div>
    <div class="row">
      <div class="col-md-4 mb-3" v-for="data in dataSource" :key="data.login.uuid">
        <div class="card h-100">
          <div class="position-absolute" style="top: 20px; right: 20px">
            <button
              type="button"
              class="btn btn-outline-danger rounded-pill"
            >
              <i class="bi bi-heart fs-5"></i>
            </button>
          </div>
          <img
            :src="data.picture[resolution.status[num]]"
            class="card-img-top"
          />
          <div class="card-body">
            <h5 class="card-title">
              {{ data.name.first}} {{ data.name.last }}( <span>{{ data.gender === 'male' ? '♂️' : '♀️'}}</span> )
            </h5>
            <p class="card-text">
              Email：<a :href="`mailto:${data.email}`"
                >{{ data.email }}</a
              >
            </p>
            <p class="card-text">
              Phone: <a :href="`tel:${data.Phone}`">{{ data.Phone }}</a>
            </p>
            <p class="card-text">
              Cell: <a :href="`tel:${data.cell}`">{{ data.cell }}</a>
            </p>
            <p class="card-text">City: {{ data.location.city }}</p>
            <p class="card-text">State: {{ data.location.state }}</p>
            <p class="card-text">Country: {{ data.location.country }}</p>
            <p class="card-text">Postcode: {{ data.location.postcode }}</p>
            <p>
              Address：<a
                :href="`https://www.google.com.tw/maps/search/${data.location.city+data.location.state}`"
                target="_blank"
                >{{ data.location.city }} {{ data.location.state }} {{ data.location.country }} {{ data.location.postcode }}</a
              >
            </p>
            <p class="card-text">
              description: {{ data.location.timezone.description }}
            </p>
            <p class="card-text">
              <small class="text-muted"
                >dob：{{ data.dob.date }}</small
              >
            </p>
            <p class="card-text">
              <small class="text-muted"
                >registered：{{ data.registered.date }}</small
              >
            </p>
            <iframe
              width="100%"
              height="300"
              frameborder="0"
              scrolling="no"
              marginheight="0"
              marginwidth="0"
              :src="`https://maps.google.com.tw/maps?f=q&amp;hl=en&amp;geocode=&amp;q=${ data.location.coordinates.latitude },${ data.location.coordinates.longitude }&amp;z=10&amp;output=embed`"
            ></iframe>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      url: 'https://randomuser.me/api/',
      dataSource: {},
      num: 0,
      resolution: {
        status: {
          0: 'thumbnail',
          1: 'medium',
          2: 'large',
        },
        text: {
          0: '模糊',
          1: '尚可',
          2: '清晰',
        },
      },
    };
  },
  methods: {
    setResolution() {
      this.num += 1;
      this.num %= 3;
    },
  },
  mounted() {
    this.axios.get(this.url, {
      params: {
        results: 10,
      },
    }).then((res) => {
      this.dataSource = res.data.results;
      console.log(this.dataSource);
    });
  },
};
</script>
