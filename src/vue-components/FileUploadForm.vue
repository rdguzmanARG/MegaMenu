<script setup lang="ts">
import { ref } from 'vue';

const description = ref<string>('');
const selectedFiles = ref<File[]>([]);
const isSubmitting = ref<boolean>(false);
const statusMessage = ref<string>('');
const isError = ref<boolean>(false);

function handleFileChange(event: Event): void {
  const input = event.target as HTMLInputElement;
  if (input.files) {
    const incoming = Array.from(input.files);
    const existingNames = new Set(selectedFiles.value.map((f) => f.name));
    const newFiles = incoming.filter((f) => !existingNames.has(f.name));
    selectedFiles.value = [...selectedFiles.value, ...newFiles];
    input.value = '';
  }
}

function removeFile(name: string): void {
  selectedFiles.value = selectedFiles.value.filter((f) => f.name !== name);
}

async function submitForm(): Promise<void> {
  if (selectedFiles.value.length === 0) {
    statusMessage.value = 'Please select at least one file to upload.';
    isError.value = true;
    return;
  }

  isSubmitting.value = true;
  statusMessage.value = '';
  isError.value = false;

  const formData = new FormData();
  formData.append('description', description.value);
  selectedFiles.value.forEach((file, index) => {
    formData.append(`files`, file, file.name);
  });

  try {

    //const response = await fetch('https://httpbin.org/post', {
    const response = await fetch('/umbraco/api/contactform/send2', {
      method: 'POST',
      body: formData,
    });

    if (!response.ok) {
      throw new Error(`Server responded with status ${response.status}`);
    }

    statusMessage.value = 'Submitted successfully!';
    description.value = '';
    selectedFiles.value = [];
    const fileInput = document.getElementById('files') as HTMLInputElement;
    if (fileInput) fileInput.value = '';
  } catch (err) {
    isError.value = true;
    statusMessage.value = err instanceof Error ? err.message : 'Submission failed. Please try again.';
  } finally {
    isSubmitting.value = false;
  }
}
</script>

<template>
  <div class="mm-form-wrapper">
    <form class="mm-form" @submit.prevent="submitForm">
      <h2 class="mm-form__title">Submit a Request</h2>

      <div class="mm-form__field">
        <label class="mm-form__label" for="description">Description</label>
        <input id="description" class="mm-form__input" type="text" v-model="description"
          placeholder="Enter a description..." autocomplete="off" />
      </div>

      <div class="mm-form__field">
        <label class="mm-form__label" for="files">Attachments</label>
        <input id="files" class="mm-form__file-input" type="file" multiple @change="handleFileChange" />
        <ul v-if="selectedFiles.length" class="mm-form__file-list">
          <li v-for="file in selectedFiles" :key="file.name" class="mm-form__file-item">
            <span class="mm-form__file-icon">&#128206;</span>
            <span class="mm-form__file-name">{{ file.name }}</span>
            <span class="mm-form__file-size">({{ (file.size / 1024).toFixed(1) }} KB)</span>
            <button type="button" class="mm-form__file-remove" @click="removeFile(file.name)"
              title="Remove">&#x2715;</button>
          </li>
        </ul>
      </div>

      <button class="mm-form__submit" type="submit" :disabled="isSubmitting">
        <span v-if="isSubmitting" class="mm-form__spinner"></span>
        {{ isSubmitting ? 'Sending...' : 'Submit' }}
      </button>

      <p v-if="statusMessage"
        :class="['mm-form__status', isError ? 'mm-form__status--error' : 'mm-form__status--success']">
        {{ statusMessage }}
      </p>
    </form>
  </div>
</template>

<style scoped lang="scss">
.mm-form-wrapper {
  display: flex;
  justify-content: center;
  padding: 2rem 1rem;
  background-color: #f5f5f5;
  min-height: 300px;
}

.mm-form {
  background: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  width: 100%;
  max-width: 560px;

  &__title {
    font-size: 1.25rem;
    font-weight: 600;
    color: #333;
    margin: 0 0 1.5rem 0;
  }

  &__field {
    margin-bottom: 1.25rem;
  }

  &__label {
    display: block;
    font-size: 0.875rem;
    font-weight: 500;
    color: #555;
    margin-bottom: 0.4rem;
  }

  &__input {
    width: 100%;
    padding: 0.6rem 0.8rem;
    font-size: 0.95rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    outline: none;
    box-sizing: border-box;
    transition: border-color 0.2s;

    &:focus {
      border-color: #189922;
      box-shadow: 0 0 0 2px rgba(24, 153, 34, 0.15);
    }
  }

  &__file-input {
    display: block;
    font-size: 0.9rem;
    color: #444;
    cursor: pointer;
  }

  &__file-list {
    list-style: none;
    margin: 0.6rem 0 0 0;
    padding: 0.5rem 0.75rem;
    background: #f9f9f9;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
  }

  &__file-item {
    display: flex;
    align-items: center;
    gap: 0.3rem;
    font-size: 0.85rem;
    color: #444;
    padding: 0.25rem 0;
  }

  &__file-name {
    flex: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  &__file-remove {
    flex-shrink: 0;
    background: none;
    border: none;
    color: #999;
    cursor: pointer;
    font-size: 0.8rem;
    padding: 0 0.2rem;
    line-height: 1;
    transition: color 0.15s;

    &:hover {
      color: #c62828;
    }
  }

  &__file-icon {
    margin-right: 0.3rem;
  }

  &__file-size {
    color: #999;
    font-size: 0.8rem;
    margin-left: 0.3rem;
  }

  &__submit {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background-color: #189922;
    color: #fff;
    border: none;
    border-radius: 4px;
    padding: 0.65rem 1.5rem;
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.2s;

    &:hover:not(:disabled) {
      background-color: #147a1b;
    }

    &:disabled {
      opacity: 0.6;
      cursor: not-allowed;
    }
  }

  &__spinner {
    display: inline-block;
    width: 14px;
    height: 14px;
    border: 2px solid rgba(255, 255, 255, 0.4);
    border-top-color: #fff;
    border-radius: 50%;
    animation: mm-spin 0.7s linear infinite;
  }

  &__status {
    margin: 1rem 0 0 0;
    padding: 0.6rem 0.8rem;
    border-radius: 4px;
    font-size: 0.9rem;

    &--success {
      background: #e8f5e9;
      color: #2e7d32;
      border: 1px solid #a5d6a7;
    }

    &--error {
      background: #ffebee;
      color: #c62828;
      border: 1px solid #ef9a9a;
    }
  }
}

@keyframes mm-spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
