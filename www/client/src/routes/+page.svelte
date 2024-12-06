<script>
   let msg = $state(""); // ny
   
   const ws = new WebSocket("ws://localhost/api/");
   ws.binaryType = "blob";

   // Log socket opening and closing
   ws.addEventListener("open", (event) => {
      console.log("Websocket connection opened");
   });
   ws.addEventListener("close", (event) => {
      console.log("Websocket connection closed");
   });

   ws.onmessage = function (message) {
      const msgDiv = document.createElement("div");
      msgDiv.classList.add("msgCtn");
      if (message.data instanceof Blob) {
         const reader = new FileReader();
         reader.onload = () => {
            msgDiv.innerHTML = reader.result;
            document.getElementById("messages").appendChild(msgDiv);
         };
         reader.readAsText(message.data);
      } else {
         console.log("Result2: " + message.data);
         msgDiv.innerHTML = message.data;
         document.getElementById("messages").appendChild(msgDiv);
      }
   };

   async function sendMsg(event) { // Ändrat
      event.preventDefault();
      ws.send(msg); // Se överst state
      msg = "";
   }
</script>

<h1>Welcome to SvelteKit</h1>
<div class="container">
   <p class="msg">Messages:</p>
   <div id="messages" class="messages"></div>
   <form id="msgForm" class="msgForm" onsubmit={sendMsg}>
      <input
         type="text"
         placeholder="Send message"
         class="input"
         id="inputBox"
         bind:value = {msg}
      />
      <input type="submit" class="btn" value="Send" />
   </form>
   <p>{msg}</p>
</div>
