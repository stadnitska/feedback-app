<script>
  import { createEventDispatcher } from 'svelte';
  export let question;
  const dispatch = createEventDispatcher();

  async function upvote() {
    try {
      const res = await fetch(`/courses/1/questions/${question.id}/upvote`, { 
        method: 'POST',
        headers: { 'Content-Type': 'application/json' }
      });
      if (res.ok) {
        const updated = await res.json();
        dispatch('upvoted', updated);
      }
    } catch (e) {
      console.error('Upvote failed', e);
    }
  }

  async function remove() {
    if (!confirm('Are you sure you want to delete this question?')) return;
    
    try {
      const res = await fetch(`/courses/1/questions/${question.id}`, { 
        method: 'DELETE',
        headers: { 'Content-Type': 'application/json' }
      });
      
      if (res.ok) {
        const deleted = await res.json();
        dispatch('deleted', deleted);
        return true;
      }
      return false;
    } catch (e) {
      console.error('Delete failed', e);
      return false;
    }
  }
</script>

<div class="question">
  <h3>{question.title}</h3>
  <p>{question.text}</p>
  <p>Upvotes: {question.upvotes ?? 0}</p>
  <button on:click={upvote}>Upvote</button>
  <button on:click={remove}>Delete</button>
</div>
