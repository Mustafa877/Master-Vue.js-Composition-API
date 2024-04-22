<script setup>
import { ref } from "vue";

const items = ref([
  {
    title: "What is HTML?",
    content:
      "The HyperText Markup Language or HTML is the standard markup language for documents designed to be displayed in a web browser. It defines the content and structure of web content. It is often assisted by technologies such as Cascading Style Sheets and scripting languages such as JavaScript.",
    open: false,
  },
  {
    title: "What is CSS?",
    content:
      "Cascading Style Sheets is a style sheet language used for specifying the presentation and styling of a document written in a markup language such as HTML or XML. CSS is a cornerstone technology of the World Wide Web, alongside HTML and JavaScript.",
    open: false,
  },
  {
    title: "What is JS?",
    content:
      "JavaScript, often abbreviated as JS, is a programming language and core technology of the World Wide Web, alongside HTML and CSS. As of 2023, 98.7% of websites use JavaScript on the client side for webpage behavior, often incorporating third-party libraries.",
    open: false,
  },
]);

const toggleAccordion = (index) => {
  items.value = items.value.map((item, i) => ({
    ...item,
    open: i === index ? !item.open : false,
  }));
};
</script>

<template>
  <div class="accordion-container">
    <div v-for="(item, index) in items" :key="index" class="accordion" :class="{ 'open': item.open }">
      <div class="accordion-header" @click="toggleAccordion(index)">
        <span class="title">{{ item.title }}</span>
        <span class="arrow-icon">{{ item.open ? "▼" : "▶" }}</span>
      </div>
      <div v-show="item.open" class="accordion-content">
        {{ item.content }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.accordion-container {
  max-width: 600px;
  margin: 50px auto;
  background: linear-gradient(to bottom, #ffffff, #f2f2f2);
  padding: 20px;
  border-radius: 12px;
}

.accordion {
  border: 1px solid #ddd;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.accordion:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.accordion.open .accordion-header {
  background-color: #6c757d;
}

.accordion-header {
  background-color: #495057;
  color: #fff;
  padding: 15px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.accordion-header:hover {
  background-color: #343a40;
}

.title {
  flex: 1;
  font-family: 'Roboto', sans-serif;
  font-size: 18px;
  line-height: 1.5;
}

.arrow-icon {
  font-size: 20px;
  transition: transform 0.3s ease;
}

.accordion.open .arrow-icon {
  transform: rotate(90deg);
}

.accordion-content {
  padding: 15px;
  display: none;
  border-top: 1px solid #ddd;
  background-color: #f9f9f9;
  color: #333;
  transition: max-height 0.3s ease;
  overflow: hidden;
}

.accordion.open .accordion-content {
  display: block;
  max-height: 500px;  
}
</style>
