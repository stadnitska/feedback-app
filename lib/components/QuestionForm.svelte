<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
  let title = '';
  let text = '';

  async function submitForm() {
    if (!title.trim() || !text.trim()) return;
    
    try {
      const res = await fetch('/courses/1/questions', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ title, text })
      });
      
      if (res.ok) {
        const newQuestion = await res.json();
        dispatch('added', newQuestion);
        title = '';
        text = '';
      }
    } catch (e) {
      console.error('Failed to add question', e);
    }
  }
</script>

<form on:submit|preventDefault={submitForm}>
  <input name="title" bind:value={title} placeholder="Title" />
  <textarea name="text" bind:value={text} placeholder="Text"></textarea>
  <button type="submit">Add question</button>
</form>
