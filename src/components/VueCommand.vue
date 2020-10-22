<template>
  <vue-terminal
    ref="vueTerinal"
    :task-list="taskList"
    :command-list="commandList"
    :not-found="notFound"
    :showHelpMessage="false"
    @execute="executing"
    defaultTaskCommandd=""
    greeting="Welcome, Fes"
    title="小船出海"
    style="margin: 0 auto"
  />
</template>
 
<script>
import VueTerminal from "vue-terminal";

const commandList = {
  version: {
    description: "Return this project version",
    messages: [{ message: "1.0.0" }],
  },
  contact: {
    description: "How to contact author",
    messages: [
      { message: "平安银行小船出海" },
    ],
  },
  about: {
    description: "About author",
    messages: [
      {
        message:
          "My name is shmilyvidian.",
      },
    ],
  }
};

function generateTime() {
  const timeNow = new Date();
  const hours = timeNow.getHours();
  const minutes = timeNow.getMinutes();
  const seconds = timeNow.getSeconds();
  let timeString = "" + hours;
  timeString += (minutes < 10 ? ":0" : ":") + minutes;
  timeString += (seconds < 10 ? ":0" : ":") + seconds;
  return timeString;
}

const mockData = [
  {
    time: generateTime(),
    type: "system",
    label: "System",
    message:
      "Welcome to PageGenerator, this is an example to show you what this project can do.",
  },
  {
    time: generateTime(),
    type: "info",
    label: "Info",
    message: "Terminal Initializing ............",
  },
  {
    time: generateTime(),
    type: "info",
    label: "Info",
    message: "start scan and compile png",
  },
  {
    time: generateTime(),
     type: "info",
    label: "Info",
    message: "convert to model in database",
  },
  {
    time: generateTime(),
    type: "success",
    label: "Success",
    message: "success, you can preview your code",
  },
];

const taskList = {
  echo: {
    description: "Echoes input",
    echo(pushToList, input) {
      input = input.split(" ");
      input.splice(0, 1);
      const p = new Promise((resolve) => {
        pushToList({
          time: generateTime(),
          label: "Echo",
          type: "success",
          message: input.join(" "),
        });
        resolve({ type: "success", label: "", message: "" });
      });
      return p;
    },
  },
  defaultTask: {
    description: "###start compile###",
    defaultTask(pushToList) {
      let i = 0;
      const p = new Promise((resolve) => {
        const interval = setInterval(() => {
          mockData[i].time = generateTime();
          pushToList(mockData[i]);
          i++;
          if (!mockData[i]) {
            clearInterval(interval);
            resolve({
              type: "success",
              label: "Success",
              message: "Example Over!",
            });
          }
        }, 1000);
      });
      return p;
    },
  },
  open: {
    description: "Open a specified url in a new tab.",
    open(pushToList, input) {
      const p = new Promise((resolve, reject) => {
        let url = input.split(" ")[1];
        if (!url) {
          reject({
            type: "error",
            label: "Error",
            message: "a url is required!",
          });
          return;
        }
        pushToList({ type: "success", label: "Success", message: "Opening" });

        if (input.split(" ")[1].indexOf("http") === -1) {
          url = "http://" + input.split(" ")[1];
        }
        window.open(url, "_blank");
        resolve({ type: "success", label: "Done", message: "Page Opened!" });
      });
      return p;
    },
  },
};
export default {
  name: 'VueCommand',
  components: { VueTerminal },
  props: {
    status: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      taskList,
      commandList,
      notFound: '抱歉您输入的指令不存在,请重新输入.',
    };
  },
  watch: {
    status(val){
      if(val === 1){
        this.$refs.vueTerinal.handleRun("defaultTask")
        setTimeout(()=>{
          this.$emit('changStatus', 2)
        }, 5000)
      }
    }
  },
  methods: {
    prompt(value) {
      if (value == "node -v") {
        this.send_to_terminal = process.versions.node;
      }
    },
    executing(e,k) {
        console.log('executing')
        console.log(e)
        console.log(k)
      }
  },
};
</script>