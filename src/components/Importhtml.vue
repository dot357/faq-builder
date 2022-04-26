<template>
  <div class="modal">
    <div class="close-modal" @click="this.$emit('close-modal', false)">X</div>
    <div class="content">
      <div class="hidden-html" v-html="html"></div>
      <textarea placeholder="Please enter the html" v-model="html"></textarea>
      <button @click="importer">Import</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      html: null,
      returnData : null
    };
  },
  methods: {
    importer() {
       
        this.$emit('event-import', this.returnData)
    },
    htmlCollectiontoArray(item) {
      const data = Array.prototype.slice.call(item);
      return data;
    },
  },
  watch: {
    html(newValue) {
      let pushableData = [];
      const data = this.$el.querySelectorAll(".hidden-html > details");
      data.forEach((element) => {
        let push;
        let additional = [];

        console.log(element.firstChild.innerHTML);
        push = {
          title: element.firstChild.innerHTML,
          content: element.lastChild.firstChild.innerHTML,
          additional: null,
          // additional => Array
          //content : text
        };
        // console.log(element.lastChild.lastChild)
        //turn html collection to an array
        const innerDetails = this.htmlCollectiontoArray(
          element.lastChild.lastChild.children
        );

        innerDetails.forEach((innerItem) => {
          additional.push({
            title: innerItem.firstChild.innerHTML,
            content: innerItem.lastChild.innerHTML,
          });
        });
        push.additional = additional;

        pushableData.push(push);
        
         this.returnData = pushableData
      });
      
      
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  width: 100%;
  height: 100%;

  z-index: 99;
}
.modal::before {
  content: "";
  display: block;

  backdrop-filter: blur(16px) saturate(180%);
  -webkit-backdrop-filter: blur(16px) saturate(180%);
  background-color: rgba(17, 25, 40, 0.75);
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  z-index: -3;
}

.modal .content {
  background: white;
  height: 90%;
  z-index: 98;
  border-radius: 4px;
  box-shadow: 0px 0px 25px 0px rgba(0, 0, 0, 0.247);
  padding: 0 2rem;
  overflow-x: hidden;
  overflow-y: overlay;

  width: min(1200px, 90%);
  position: absolute;
  top: 50%;
  right: 50%;
  transform: translate(50%, -50%);

  padding: 2rem;
}

.close-modal {
  background: rgb(255, 90, 90);
  cursor: pointer;
  height: 35px;
  width: 35px;
  border-radius: 50%;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 20px;

  right: 155px;
  z-index: 99;
}

button {
  margin-top: 15px;
  width: 100%;
  padding: 6px 12px;
  outline: none;
  border: none;
  cursor: pointer;
  border: 1px solid black;
  background: transparent;
  border-radius: 4px;
}
button:hover {
  transition: all 0.15s cubic-bezier(0.165, 0.84, 0.44, 1);
  background: black;
  color: white;
}
textarea {
  resize: none;
  width: 100%;
  height: 90%;
  border: 1px solid black;
  border-radius: 4px;
  outline: none;
  padding: 1rem;
}

.hidden-html{
  position: absolute;
  z-index: -999;
  pointer-events: none;
  opacity: 0;
}
</style>
