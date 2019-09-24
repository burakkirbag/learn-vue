<template>
  <div id="app" class="container mt-4">
    <div class="row">
      <div class="col-md-3">
        <div class="card shadow mb-3">
          <div class="card-header">User</div>
          <div class="card-body">
            <div class="input-group mb-3">
              <input
                type="text"
                class="form-control"
                name="message"
                id="message"
                placeholder="Name"
                v-model="user.name"
              />
            </div>
            <div class="input-group">
              <input
                type="text"
                class="form-control"
                name="message"
                id="message"
                placeholder="Username"
                v-model="user.username"
              />
            </div>
          </div>
          <div class="card-footer"></div>
        </div>
        <div class="card shadow">
          <div class="card-header">Users</div>
          <div class="list-group list-group-flush">
            <a
              v-for="u in users"
              :key="'user-'+u.id"
              href="#"
              class="list-group-item list-group-item-action"
            >
              @{{u.username}}
              <br />
              <small class="font-italic text-primary">{{u.name}}</small>
            </a>
          </div>
          <div class="card-footer"></div>
        </div>
      </div>
      <div class="col-md-9">
        <div class="card shadow">
          <div class="card-header">@burakkirbag messages</div>
          <div class="card-body">
            <div v-for="m in messages" :key="'message-'+m.id" class="media">
              <img :src="m.user.avatar" class="mr-3 rounded-circle shadow-sm" alt />
              <div class="media-body">
                <h5 class="mt-0">
                  {{m.user.name}}
                  <small class="ml-2 font-italic">@{{m.user.username}}</small>
                </h5>
                <p>{{m.message}}</p>
              </div>
            </div>
          </div>
          <div class="card-footer">
            <div class="input-group">
              <div class="input-group-prepend">
                <span class="input-group-text" id="basic-addon1">@burakkirbag</span>
              </div>
              <input
                v-model="usermessage"
                type="text"
                class="form-control"
                name="message"
                id="message"
                placeholder="Message"
                aria-label="Message"
                aria-describedby="button-addon2"
                @keydown.enter="send"
              />
              <div class="input-group-append">
                <button
                  class="btn btn-outline-secondary"
                  type="button"
                  id="button-addon2"
                  @click="send"
                >Send</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  mounted() {
    this.listen();
  },
  methods: {
    listen() {
      this.ws = new WebSocket("ws://80989dae.ngrok.io/ws");
      this.ws.onmessage = this.parsemessage;
      this.ws.onopen = msg => {
        console.log("opn", msg);
      };
      this.ws.onclose = msg => {
        console.log("cls", msg);
      };
      this.ws.onerror = msg => {
        console.log("err", msg);
      };
    },
    parsemessage(msg) {
      let data = JSON.parse(msg.data);
      this.messages.push(data);
    },
    send() {
      let msg = {
        user: this.user,
        id: this.messages.length,
        message: this.usermessage
      };

      this.usermessage = "";

      this.ws.send(JSON.stringify(msg));
    }
  },
  data() {
    return {
      ws: null,
      messages: [],
      usermessage: "",
      user: {
        name: "Burak",
        username: "burakkirbag",
        id: 5,
        avatar: "https://i.pravatar.cc/48"
      },
      users: [
        {
          name: "Burak",
          username: "burakkirbag",
          id: 5
        },
        {
          name: "Elif",
          username: "elif",
          id: 6
        },
        {
          name: "Lokum",
          username: "lokum",
          id: 7
        }
      ]
    };
  }
};
</script>

<style>
</style>
