<template>
  <div id="nav">
    <!-- <button @click="connect">연결</button>
    <input type="number" v-model="number" />
    <button @click="numberRequester">전송</button>
    <ul>
      <li v-for="items in arrays" :key="items.id">
        {{ items }}
      </li>
    </ul> -->
    <button @click="action">시작</button>
  </div>
</template>

<script>
// import {
//   RSocketClient,
//   JsonSerializer,
//   IdentitySerializer,
// } from "rsocket-core";
// import {
// BufferEncoders,
// JsonSerializer,
// RSocketClient,
// APPLICATION_JSON,
// MESSAGE_RSOCKET_COMPOSITE_METADATA,
// encodeRoute,
// MESSAGE_RSOCKET_ROUTING,
// encodeCompositeMetadata,
// IdentitySerializer,
// } from "rsocket-core";
// import RSocketWebSocketClient from "rsocket-websocket-client";

// export default {
//   data() {
//     return {
//       client: new RSocketClient({
//         serializers: {
//           data: JsonSerializer,
//           metadata: IdentitySerializer,
//         },
//         setup: {
//           keepAlive: 60000,
//           lifetime: 180000,
//           dataMimeType: "application/json",
//           metadataMimeType: "message/x.rsocket.routing.v0",
//         },
//         transport: new RSocketWebSocketClient({
//           url: "ws://localhost:6565/rsocket",
//         }),
//       }),
//       number: "",
//       socket: "",
//       arrays: [],
//     };
//   },
//   created() {
//     this.client.connect().then((socket) => {
//       console.log("socket :: ", socket);
//       this.socket = socket;
//       // document
//       //   .getElementById("n")
//       //   .addEventListener("change", ({ srcElement }) => {
//       //     this.numberRequester(socket, parseInt(srcElement.value));
//       //   });
//     }, this.errorHanlder);
//   },
//   methods: {
//     connect() {
//       new RSocketClient({
//         serializers: {
//           data: JsonSerializer,
//           metadata: IdentitySerializer,
//         },
//         setup: {
//           keepAlive: 999999,
//           lifetime: 999999,
//           dataMimeType: APPLICATION_JSON.string,
//           metadataMimeType: MESSAGE_RSOCKET_COMPOSITE_METADATA.string,
//           payload: {
//             metadata: String.fromCharCode("setup".length) + "setup",
//           },
//         },
//         transport: new RSocketWebSocketClient(
//           {
//             debug: true,
//             url: "ws://localhost:6565/rsocket",
//           },
//           BufferEncoders
//         ),
//       });
//     },
//     numberRequester() {
//       console.log("this.socket", this.socket);
//       this.socket
//         .requestStream({
//           data: 5,
//           metadata:
//             String.fromCharCode("number.stream".length) + "number.stream",
//         })
//         .subscribe({
//           onError: this.errorHanlder,
//           onNext: this.responseHanlder,
//           onSubscribe: (subscription) => {
//             console.log("subscription : ", subscription);
//             subscription.request(100); // set it to some max value
//           },
//         });
//     },
//     errorHanlder(e) {
//       console.log("rsocket error : ", e);
//     },
//     responseHanlder(payload) {
//       console.log("roscoket response : ", payload);
//       this.arrays.push(payload);
//       // const li = document.createElement("li");
//       // li.innerText = payload.data;
//       // li.classList.add("list-group-item", "small");
//       // document.getElementById("result").appendChild(li);
//     },
//   },
// };
export default {
  data() {
    return {
      // client: new RSocketClient({
      //   serializers: {
      //     data: JsonSerializer,
      //     metadata: IdentitySerializer,
      //   },
      //   setup: {
      //     keepAlive: 60000,
      //     lifetime: 180000,
      //     dataMimeType: "application/json",
      //     metadataMimeType: "message/x.rsocket.routing.v0",
      //   },
      //   transport: new RSocketWebSocketClient({
      //     url: "ws://localhost:6565/rsocket",
      //   }),
      // }),
    };
  },
  created() {},
  methods: {
    action() {
      this.client.connect().subscribe({
        onComplete: (socket) => {
          socket
            .requestStream({
              data: null,
              metadata:
                String.fromCharCode("my.time-updates.stream".length) +
                "my.time-updates.stream",
            })
            .subscribe({
              onComplete: () => console.log("complete"),
              onError: (error) => {
                console.log("Connection has been closed due to:: " + error);
              },
              onNext: (payload) => {
                console.log(payload);
              },
              onSubscribe: (subscription) => {
                console.log("subscription : ", subscription);
                subscription.request(1000000);
              },
            });
        },
      });
    },
  },
};
</script>
