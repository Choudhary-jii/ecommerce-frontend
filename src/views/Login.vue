<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100 px-4">
    <div class="bg-white rounded-3xl shadow-2xl p-8 w-full max-w-md">
      <h2 class="text-2xl font-bold mb-6 text-center text-purple-700">
        Welcome Back
      </h2>

      <div v-if="step === 1">
        <label class="block text-sm font-semibold mb-2 text-gray-700">
          Mobile Number
        </label>
        <input
          v-model="mobile"
          type="text"
          placeholder="Enter your 10-digit mobile number"
          class="w-full px-4 py-2 rounded-md border border-gray-300 focus:outline-none focus:ring-2 focus:ring-purple-500"
        />
        <p v-if="mobileError" class="text-red-500 text-sm mt-1">
          {{ mobileError }}
        </p>

        <button
          @click="requestOtp"
          class="w-full mt-6 bg-purple-600 text-white py-2 rounded-full hover:bg-purple-700"
        >
          Send OTP
        </button>
      </div>

      <div v-else>
        <label class="block text-sm font-semibold mb-2 text-gray-700">
          Full Name
        </label>
        <input
          v-model="fullName"
          type="text"
          placeholder="Enter your full name"
          class="w-full px-4 py-2 mb-4 rounded-md border border-gray-300 focus:outline-none focus:ring-2 focus:ring-purple-500"
        />

        <label class="block text-sm font-semibold mb-2 text-gray-700">
          OTP
        </label>
        <input
          v-model="otp"
          type="text"
          placeholder="Enter OTP"
          class="w-full px-4 py-2 rounded-md border border-gray-300 focus:outline-none focus:ring-2 focus:ring-purple-500"
        />

        <p v-if="otpError" class="text-red-500 text-sm mt-1">{{ otpError }}</p>

        <button
          @click="verifyOtp"
          class="w-full mt-6 bg-purple-600 text-white py-2 rounded-full hover:bg-purple-700"
        >
          Verify & Login
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { jwtDecode } from "jwt-decode";

export default {
  name: "LoginPage",
  data() {
    return {
      step: 1,
      mobile: "",
      fullName: "",
      otp: "",
      mobileError: "",
      otpError: "",
    };
  },
  methods: {
    validateMobile(number) {
      const regex = /^[6-9]\d{9}$/;
      return regex.test(number);
    },
    async requestOtp() {
      this.mobileError = "";
      if (!this.validateMobile(this.mobile)) {
        this.mobileError =
          "Enter a valid 10-digit mobile number starting with 9, 8, 7, or 6.";
        return;
      }

      try {
        const res = await fetch(
          "https://aayushchoudhary.pythonanywhere.com/api/auth/request-otp/",
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ mobile: this.mobile }),
          }
        );

        const data = await res.json();
        if (res.ok) {
          this.step = 2;
        } else {
          this.mobileError = data.message || "Failed to send OTP.";
        }
      } catch (err) {
        this.mobileError = "Network error. Try again.";
      }
    },

    async verifyOtp() {
      this.otpError = "";
      if (!this.otp || !this.fullName) {
        this.otpError = "Please enter both full name and OTP.";
        return;
      }

      try {
        const res = await fetch(
          "https://aayushchoudhary.pythonanywhere.com/api/auth/verify-otp/",
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              mobile: this.mobile,
              otp: this.otp,
              full_name: this.fullName,
            }),
          }
        );

        const data = await res.json();
        console.log("OTP verification response:", data);

        if (res.ok) {
          console.log(data.token);
          localStorage.setItem("token", data.token);
          const decoded = jwtDecode(data.token);
          console.log(decoded);
          localStorage.setItem("user_id", decoded.user_id);
          localStorage.setItem("full_name", data.full_name);

          alert("Login successful! 🎉"); // <-- THIS WILL SHOW THAT LOGIN WORKED

          console.log("Login success, redirecting to /about...");
          // THEN redirect to homepage
          this.$router.push("/");
        } else {
          this.otpError = data.message || "OTP verification failed.";
        }
      } catch (err) {
        console.error("Error verifying OTP:", err);
        this.otpError = "Network error. Try again.";
      }
    },
  },
};
</script>

<style scoped>
body {
  font-family: "Poppins", sans-serif;
}
</style>
