<template>
  <div class="upload-wrapper">
    <div class="left-container">
      <div class="title-with-icon">
        <div class="icon-circle">
          <div class="background-circle">
            <div class="inner-circle">1</div>
          </div>
        </div>
      </div>
      <p class="explanation-title">
        Enviar respondentes
      </p>
      <p class="explanation">
        Coloque aqui o seu arquivo com a lista de contatos que responderão esta pesquisa.
      </p>
      <button class="attach-button" @click="triggerFileInput">Anexar arquivo</button>
    </div>
    <div class="dashed-arrow">
      <img src="/Arrow 3.svg" class="arrow-icon" alt="Arrow" />
    </div>
    <div class="right-container">
      <div class="title-with-icon">
        <div class="icon-circle">2</div>
      </div>
      <p class="explanation-title">
        Relação de dados
      </p>
      <p class="explanation">
        Faça a associação das colunas da sua lista de contatos com os dados da plataforma. Não se preocupe. É bem simples!
      </p>
    </div>
    <input type="file" ref="fileInput" @change="handleFileSelect" style="display: none;" accept=".xlsx, .xls" />
  </div>
</template>

<script lang="ts">
import { ref, Ref } from 'vue';

export default {
  setup() {
    const fileInput: Ref<null | HTMLInputElement> = ref(null);
    const file = ref<File | null>(null);

    const triggerFileInput = () => {
      fileInput.value?.click();
    };

    const handleDrop = (event: DragEvent) => {
      event.preventDefault();
      if (event.dataTransfer?.files) {
        const files = event.dataTransfer.files;
        if (files.length > 0) {
          const uploadedFile = files[0];
          // Check if the file is an Excel file
          if (uploadedFile.name.endsWith('.xlsx') || uploadedFile.name.endsWith('.xls')) {
            file.value = uploadedFile;
          } else {
            alert("Please upload only .xlsx or .xls files.");
          }
        }
      }
    };
    const handleFileSelect = (event: Event) => {
      const input = event.target as HTMLInputElement;
      if (input.files) {
        const files = input.files;
        if (files.length > 0) {
          file.value = files[0];
        }
      }
    };

    const highlight = (event: DragEvent) => {
      event.preventDefault();
    };

    const unhighlight = (event: DragEvent) => {
      event.preventDefault();
    };

    return {
      fileInput,
      file,
      triggerFileInput,
      handleDrop,
      handleFileSelect,
      highlight,
      unhighlight,
    };
  },
};
</script>


<style scoped>
.upload-wrapper {
  display: flex;
  gap: 80px;
}

.left-container {
  width: 261px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.title-with-icon {
  width: 261px;
  height: 67px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
}

.icon-circle {
  width: 32px;
  height: 32px;
  border: 2px solid black;
  border-radius: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative; /* This is important to position the inner circle */
}

.background-circle {
  width: 25px;
  height: 24px;
  background-color: #000; /* Replace with the actual background color you want */
  border-radius: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute; /* This positions it within the icon-circle */
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /* Centers the circle */
}

.inner-circle {
  width: 100%; /* Makes the inner text circle take the full width of the background circle */
  height: 100%; /* Makes the inner text circle take the full height of the background circle */
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: Inter, sans-serif;
  font-size: 18px;
  font-weight: 700;
  line-height: 22px;
  color: #FFF; /* Replace with the actual text color you want */
  border-radius: 100px; /* Makes the text circle rounded */
}

.explanation {
  font-family: Inter, sans-serif;
  font-size: 14px;
  text-align: center;
}

.attach-button {
  border: none;
  background-color: none;
  color: #09A57C;
  padding: 10px 20px;
  font-family: Inter, sans-serif;
  font-size: 16px;
  cursor: pointer;
  border-radius: 4px;
}

.right-container {
  width: 261px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.dashed-arrow {
  padding-top: 24px;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  flex-grow: 1;
}

.arrow-icon {
  position: absolute;
  width: 339px;
}
</style>
