<template>
  <div class="relative h-10 m-1">
    <div style="border-top 1px solid #0000" class="grid grid-cols-6">
      <input
        type="text"
        v-model="message"
        @keyup.enter="sendMessage()"
        placeholder="Say something..."
        class="col-span-5 outline-none p-1"/>
      <button @click="sendMessage()">Send</button>
    </div>
  </div>
</template>

<script>
import Button from "../../Jetstream/Button";

export default {
  components: { Button },
  props: ["room"],
  data: function () {
    return {
      message: "",
    };
  },
  methods: {
    sendMessage() {
      if (this.message == " ") {
        return;
      }

      axios
        .post("/chat/room/" + this.room.id + "/message", {
          message: this.message,
        })
        .then((response) => {
          if (response.status == 201) {
            this.message = "";
            this.$emit("messageSent");
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>