<template>
  <div class="container mt-5">
    <h1 class="mb-4">ร้านกาแฟ</h1>

    <!-- แสดงรายชื่อร้านกาแฟ -->
    <div class="row">
      <div class="col-md-6">
        <h2>รายชื่อร้านกาแฟ</h2>
        <div class="card-group">
          <div
            v-for="(shop, index) in coffeeShops"
            :key="index"
            class="card mb-3"
          >
            <img
              :src="shop.imageUrl"
              class="card-img-top"
              alt="รูปร้านกาแฟ"
            >
            <div class="card-body">
              <h5 class="card-title">{{ shop.name }}</h5>
              <p class="card-text">{{ shop.description }}</p>
            </div>
            
          </div>
        </div>
      </div>

      <!-- แสดงข้อมูลการจอง -->
      <div class="col-md-6">
        <h2>ข้อมูลการจอง</h2>
        <ul class="list-group">
          <li
            v-for="(reservation, index) in reservations"
            :key="index"
            class="list-group-item"
          >
            <b>ชื่อร้าน:</b> {{ reservation.coffeeShopName }}<br>
            <b>ชื่อผู้จอง:</b> {{ reservation.customerName }}<br>
            <b>เบอร์โทร:</b> {{ reservation.phoneNumber }}<br>
            <b>วันที่:</b> {{ reservation.date }}<br>
            <b>เวลา:</b> {{ reservation.time }}<br>
            <b>จำนวนโต๊ะ:</b> {{ reservation.tableCount }}<br>
          </li>
        </ul>
      </div>
    </div>

    <!-- แบบฟอร์มจองโต๊ะ -->
    <div class="mt-4">
      <h2>จองโต๊ะ</h2>
      <form @submit.prevent="saveReservation">
        <div class="mb-3">
          <label for="coffeeShopSelect" class="form-label">เลือกร้านกาแฟ</label>
          <select v-model="currentCoffeeShopIndex" id="coffeeShopSelect" class="form-select">
            <option value="" disabled>กรุณาเลือกร้านกาแฟ</option>
            <option
              v-for="(shop, index) in coffeeShops"
              :key="index"
              :value="index"
            >
              {{ shop.name }}
            </option>
          </select>
        </div>
        <div class="mb-3">
          <label for="customerName" class="form-label">ชื่อผู้จอง</label>
          <input v-model="customerName" type="text" class="form-control" id="customerName">
        </div>
        <div class="mb-3">
          <label for="phoneNumber" class="form-label">เบอร์โทร</label>
          <input v-model="phoneNumber" type="text" pattern="[0-9]{10}" required class="form-control" id="phoneNumber">
        </div>
        <div class="mb-3">
          <label for="date" class="form-label">วันที่</label>
          <input v-model="date" type="date" class="form-control" id="date">
        </div>
        <div class="mb-3">
          <label for="time" class="form-label">เวลา</label>
          <input v-model="time" type="time" class="form-control" id="time">
        </div>
        <div class="mb-3">
          <label for="tableCount" class="form-label">จำนวนโต๊ะ</label>
          <input v-model="tableCount" type="number" class="form-control" id="tableCount" min="1">
        </div>
        <button type="submit" class="btn btn-primary">บันทึก</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      coffeeShops: [
        { name: 'warm up', id: 1, imageUrl: 'https://media2.thrillophilia.com/images/photos/000/167/725/original/1625640085_1565338296_28872692_1653610264676619_1204406798002421760_o.jpg.jpg?gravity=center&width=1280&height=642&crop=fill&quality=auto&fetch_format=auto&flags=strip_profile&format=jpg&sign_url=true', description: '40 Nimmanhaemin Road, Chiang Mai, Thailand, Chiang Mai' },
        { name: 'TAMMA CAFE ', id: 2, imageUrl: 'https://scontent.fcnx2-1.fna.fbcdn.net/v/t39.30808-6/361305041_779199773993037_7111305937619716444_n.jpg?_nc_cat=110&ccb=1-7&_nc_sid=a2f6c7&_nc_eui2=AeFxvmteBvy2K79UsDn0RYOZKhovrXyAa8YqGi-tfIBrxpKuFLPrFlvZOGn0EPhZHElf6G2dL5UlEl78fU6V5jXx&_nc_ohc=uHWrPKKsU5IAX-bQClg&_nc_ht=scontent.fcnx2-1.fna&oh=00_AfCldRePkm9xyR75WX2AT0E6PCwTgLjqStyApa5lrnLYyQ&oe=6508E387', description: '648 หมู่ 5 ตำบลหนองหาร อำเภอสันทราย, Amphoe San Sai, Thailand, Chiang Mai' },
        { name: 'Overnight Bar Maejo', id: 3, imageUrl: 'https://scontent.fcnx2-1.fna.fbcdn.net/v/t39.30808-6/314943429_107842845474701_641691726405226769_n.jpg?stp=cp6_dst-jpg&_nc_cat=107&ccb=1-7&_nc_sid=a2f6c7&_nc_eui2=AeHl0FldXmTD61v_BgJwf6UpEYxzLzXtdwsRjHMvNe13C4JNOu4gKCYkItxS5mN6FvOvDtMWXSVXq5T0d79ZOxPu&_nc_ohc=9XTY6Iz78FAAX82ESoc&_nc_ht=scontent.fcnx2-1.fna&oh=00_AfD5P87LB9TPLnaMZpuQguBT29BRXBzGjH4pe525yzXEcw&oe=6509B431', description: '287 หมู่5 ต.หนองหาร, Amphoe San Sai, Thailand, Chiang Mai' },
      ],
      reservations: [],
      currentCoffeeShopIndex: null,
      customerName: '',
      phoneNumber: '',
      date: '',
      time: '',
      tableCount: 1,
    };
  },
  methods: {
    reserveTable(coffeeShopIndex) {
      this.currentCoffeeShopIndex = coffeeShopIndex;
    },
    saveReservation() {
      if (this.currentCoffeeShopIndex !== null) {
        this.reservations.push({
          coffeeShopName: this.coffeeShops[this.currentCoffeeShopIndex].name,
          customerName: this.customerName,
          phoneNumber: this.phoneNumber,
          date: this.date,
          time: this.time,
          tableCount: this.tableCount,
        });

        // Reset form data
        this.customerName = '';
        this.phoneNumber = '';
        this.date = '';
        this.time = '';
        this.tableCount = 1;
        this.currentCoffeeShopIndex = null;
      }
    },
  },
};
</script>

<style>
.coffee-image {
  max-width: 80px;
  border: 2px solid #FF5733;
  border-radius: 10px;
  padding: 5px;
}
</style>
