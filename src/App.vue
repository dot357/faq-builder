<template>
  <section class="main">
    <div class="floating-menu">
      <button class="button" @click="saveAsTemplate">Save as template</button>
      <button
        @click="getDataFromLocalStorage"
        class="button"
        :disabled="!show.load"
        :class="{ active: show.load }"
      >
        Load from template
      </button>
    </div>
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <div class="left-side">
      <button class="add" @click="addMore">Add</button>
      <details v-for="(item, index) in faq" :key="index">
        <summary>
          <input type="text" v-model="item.title" />
          <button class="remove" @click="removeItem(index)">X</button>
        </summary>
        <div class="content">
          <textarea type="text" v-model="item.content"></textarea>
          <button class="addInner" @click="addInner(index)">Add Inner</button>

          <div class="inner" v-if="item.additional.length > 0">
            <details
              v-for="(innerItem, innerIndex) in item.additional"
              :key="innerIndex"
            >
              <summary>
                <input type="text" v-model="innerItem.title" /><button
                  class="remove"
                  @click="removeInner(index, innerIndex)"
                >
                  X
                </button>
              </summary>
              <div class="content">
                <textarea type="text" v-model="innerItem.content"></textarea>
              </div>
            </details>
          </div>
        </div>
      </details>
    </div>
    <div class="right-side">
      <details v-for="(item, index) in faq" :key="index">
        <summary>{{ item.title }}</summary>
        <div class="content">
          {{ item.content }}

          <div class="inner" v-if="item.additional.length > 0">
            <details
              v-for="(innerItem, innerIndex) in item.additional"
              :key="innerIndex"
            >
              <summary>{{ innerItem.title }} <button>Remove</button></summary>
              <div class="content">{{ innerItem.content }}</div>
            </details>
          </div>
        </div>
      </details>
    </div>
  </section>
</template>

<script>
import { useToast } from "vue-toastification";
export default {
  data() {
    return {
      show: {
        load: false,
      },
      faq: [],
    };
  },
  mounted() {
    this.getDataFromLocalStorage();
  },
  methods: {
    getDataFromLocalStorage() {
      const data = JSON.parse(localStorage.getItem("template"));
      if (data !== null && data !== undefined && data !== []) {
        this.faq = data;
        this.show.load = true;

        const toast = useToast();
        toast.success("Just loaded", {
          timeout: 2000,
        });
      }
    },
    addMore() {
      const simpleObj = {
        title: "Added item TITLE",
        content: "Added item CONTENT",
        additional: [
          {
            title: "ADDED Some inner title",
            content: "ADDED Some inner content",
          },
        ],
      };

      this.faq.push(simpleObj);
    },
    addInner(id) {
      console.log(id);
      const addItem = {
        title: "ADDED Some inner title",
        content: "ADDED Some inner content",
      };
      this.faq[id].additional.push(addItem);
    },
    removeItem(id) {
      this.faq = this.faq.filter((e, index) => index !== id);
    },
    removeInner(id, innerId) {
      this.faq[id].additional = this.faq[id].additional.filter(
        (e, innerIndex) => innerIndex !== innerId
      );
    },
    exportToHTML() {
      let HTML = `
      
      `;
    },
    saveAsTemplate() {
      localStorage.setItem("template", JSON.stringify(this.faq));
      this.show.load = true;
      const toast = useToast();
      toast.success("Save successful", {
        timeout: 2000,
      });
    },
  },
};
</script>

<style scoped>
.main {
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: row;
}

.main > div {
  padding: 2rem;
}
.left-side {
  width: 40%;
  border-right: 1px solid black;
}
.right-side {
  width: 100%;
}
details {
  margin-bottom: 15px;
}
details summary {
  background: black;

  color: white;
  padding: 1rem;
  position: relative;
}

details .content {
  background: white;
  box-shadow: 0px 0px 25px 0px rgba(0, 0, 0, 0.151);
  padding: 1rem;
  width: 100%;
}

input,
textarea {
  width: 100%;
  padding: 6px 12px;
  background: transparent;
  border: none;
  outline: none;
  color: white;
  border: 4px;
  border: 1px solid rgba(255, 255, 255, 0.232);
}
details button {
  position: absolute;
  right: 10px;
  top: 10px;
  outline: none;
  border: none;
  background: transparent;
  cursor: pointer;
}
details > summary {
  list-style: none;
}
details > summary::-webkit-details-marker {
  display: none;
}

details .content {
  position: relative;
}

details button:hover {
  transition: all 0.3s cubic-bezier(0.165, 0.84, 0.44, 1);
  opacity: 0.9;
  transform: scale(0.9);
}

textarea {
  color: black;
  margin-bottom: 25px;
  resize: none;
  border: 1px solid rgba(0, 0, 0, 0.307);
  border-radius: 4px;
  min-height: 150px;
  height: auto;
}

.remove {
  background: #f56942;
  height: 20px;
  width: 20px;
  border-radius: 50%;
  font-weight: bold;
}
.addInner,
.add {
  background: white;
  border: 1px solid black;
  height: 30px;
  width: 80px;
  border-radius: 4px;
  font-weight: bold;
  color: black;
}
.add {
  width: 100%;
  margin-bottom: 15px;
}

.addInner:hover,
.add:hover {
  background: black;
  color: white;
}

.floating-menu {
  position: absolute;
  width: 250px;

  padding: 1rem !important;
  display: flex;
  flex-direction: column;
  gap: 13px;
  bottom: 2rem;
  right: 2rem;
}
.floating-menu .button {
  background: white;
  outline: none;
  border: 1px solid black;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}
.floating-menu .button:hover {
  transition: all 0.15s ease-in-out;
  background: black;
  color: white;
}
.floating-menu .button:disabled {
  border-color: rgba(0, 0, 0, 0.367);
  cursor: not-allowed;
}
.floating-menu .button:disabled:hover {
  background: white;
  color: rgba(0, 0, 0, 0.432);
}

.floating-menu .active {
  background: black;
  color: white;
}
</style>
