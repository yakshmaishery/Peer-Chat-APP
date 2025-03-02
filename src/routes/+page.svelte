<script lang="ts">
   import Header from "$lib/Components/Header.svelte";
   import ShareScreenWindow from "$lib/Components/ShareScreenWindow.svelte";
   import ChatWindow from "$lib/Components/ChatWindow.svelte";
   import {Peer} from 'peerjs'
   import {nanoid} from 'nanoid'
   import Swal from 'sweetalert2';
   let CurrentUser = ""
   let AnotherUser = ""
   let IsConnected = false
   let videodata:HTMLVideoElement
   let conn:any
   let Window = ""
   let UserMessage = ""
   let LogMessages:{type:string;message:string}[] = []

   const shortdummyID = nanoid(4).toLowerCase() // Generate Random User ID

   var peer = new Peer(shortdummyID,{config: {iceServers: [{ urls: 'stun:stun.l.google.com:19302' }]}}) // Create Peer

   peer.on("open",(id) => { // Connect Peer if Success set the ID
      CurrentUser = id
   })

   peer.on('error', (err) => { // IF Peer connection fails
      Swal.fire({icon:"error",title:"PEER CONNECTION:- "+err.type,confirmButtonColor: "green"})
   });

   // Connect with another person
   function ConnectwithUserFirst(){
      debugger
      conn = peer.connect(AnotherUser)
      conn.on("open",function(){
         conn.send("jhzxkdvbuyizxv")
         IsConnected = true
         Swal.fire({icon:"success",title:"Connected successfully",confirmButtonColor: "green"})
      })
      conn.on('error', (err:any) => {
         Swal.fire({icon:"error",title:err.type,confirmButtonColor: "green"})
      });
   }

   // Start Share Screen
   async function ShareScreen() {
      let screenStream = await navigator.mediaDevices.getDisplayMedia({
      audio: true,
      video:{
             width:{ideal:4096},
             height:{ideal:2160}
      }
      }).catch((e) => {
             if(e.name == "NotAllowedError"){
                    Swal.fire({icon:"warning",title:"Recording was cancelled",confirmButtonColor: "green"})
             }
             else{
                    Swal.fire({icon:"error",title:"Something went wrong!",confirmButtonColor: "green"})
             }
      })
      if(screenStream){
         // @ts-ignore
         peer.call(AnotherUser,screenStream)
         conn.send("SharedScreen")
         const mediarecorder = new MediaRecorder(screenStream)
         mediarecorder.start()
         mediarecorder.addEventListener("stop",()=>{
            // LeaveConnection()
         })
         Swal.fire({icon:"success",title:"Screen is shared",confirmButtonColor: "green"})
      }
   }

   // Fetch the Stream of video
   peer.on('call', function(call) {
      call.answer()
      call.on("stream",function(remoteStream:any) {
         if(videodata){
            videodata.srcObject = remoteStream
            videodata.play()
         }
      })
   })

   // Get other Data
   peer.on('connection', function(inconn) {
      inconn.on('data', function(data:any){
         if(data == "CHATLEAVECODE"){
            // If leave
            videodata.srcObject = null
            conn.close()
            location.reload()
         }
         else if(data == "SharedScreen"){
            // If share screen
            document.getElementById("btnscreenwin")?.click()
         }
         else if(data == "jhzxkdvbuyizxv"){
            console.warn(inconn.peer)
            AnotherUser = inconn.peer
            conn = peer.connect(AnotherUser)
            conn.on("open",function(){
               IsConnected = true
               Swal.fire({icon:"success",title:`Connection has been established with ${AnotherUser}`,confirmButtonColor: "green"})
            })
         }
         else{
            LogMessages.push({type:"Receiver",message:data})
            LogMessages = LogMessages
            scrolldownmessages()
         }
      });
   });
   const LeaveConnection = () => {
      conn.send("CHATLEAVECODE")
      conn.close()
      location.reload()
   }
   const SendMessage = () => {
      conn.send(UserMessage)  
      LogMessages.push({type:"Sender",message:UserMessage})
      UserMessage = ""
      LogMessages = LogMessages
      scrolldownmessages()
   }
   const scrolldownmessages = () => {
      setTimeout(() => {
         const div = document.getElementById("chatwindow");
         if(div){
            div.scrollTo({
               top: div.scrollHeight+2000,
               behavior: "smooth"
            });
         }
      }, 500);
   }
</script>
<div id=MainWindow>
   <Header bind:Window={Window} {CurrentUser} bind:AnotherUser={AnotherUser} bind:IsConnected={IsConnected}
   on:ConnectwithUserFirst={ConnectwithUserFirst} on:LeaveConnection={LeaveConnection} />
   <!-- <div style="width: 100%;"> -->
   <div hidden={!(Window == "screen")}>
      <ShareScreenWindow bind:videodata={videodata} on:ShareScreen={ShareScreen}/>
   </div>
   <div hidden={!(Window == "chat")}>
      <ChatWindow bind:UserMessage={UserMessage} on:SendMessage={SendMessage} bind:LogMessages={LogMessages}/>
   </div>
   <div hidden={!(Window == "")}>
     
   </div>
   <!-- </div> -->
</div>
<style>
   #MainWindow{
      display: grid;
      grid-template-rows: max-content 90vh;
      height: 100vh;
   }
</style>