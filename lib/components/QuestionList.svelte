<script>
  import QuestionItem from './QuestionItem.svelte';
  export let questions = [];
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
</script>

{#if questions.length === 0}
  <p>No questions yet.</p>
{:else}
  {#each questions as q (q.id)}
    <QuestionItem
      question={q}
      on:upvoted={(e) => dispatch('upvoted', e.detail)}
      on:deleted={async (e) => {
        const success = await e.detail;
        if (success) {
          dispatch('deleted', q);
        }
      }} />
  {/each}
{/if}
