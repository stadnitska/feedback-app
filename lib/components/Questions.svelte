<script>
  import { onMount } from 'svelte';
  import QuestionForm from './QuestionForm.svelte';
  import QuestionList from './QuestionList.svelte';

  let questions = [];

  async function fetchQuestions() {
    try {
      const res = await fetch('/courses/1/questions');
      if (res.ok) {
        questions = await res.json();
      } else {
        questions = [];
      }
    } catch (e) {
      console.error('Initial fetch failed', e);
      questions = [];
    }
  }

  onMount(fetchQuestions);

  async function addQuestion(question) {
    try {
      const res = await fetch('/courses/1/questions', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(question)
      });
      if (res.ok) {
        await fetchQuestions();
      }
    } catch (e) {
      console.error('Failed to add question', e);
    }
  }

  async function upvoteQuestion(updated) {
    try {
      await fetch(`/courses/1/questions/${updated.id}/upvote`, {
        method: 'POST'
      });
      await fetchQuestions();
    } catch (e) {
      console.error('Failed to upvote question', e);
    }
  }

  async function deleteQuestion(question) {
    try {
      const response = await fetch(`/courses/1/questions/${question.id}`, {
        method: 'DELETE',
        headers: { 'Content-Type': 'application/json' }
      });
      
      if (response.ok) {
        await response.json();
        await fetchQuestions();
        return true;
      }
      return false;
    } catch (e) {
      console.error('Failed to delete question', e);
      return false;
    }
  }
</script>

<section>
  <h2>Questions</h2>
  <QuestionForm on:added={addQuestion} />
  <QuestionList {questions}
    on:upvoted={upvoteQuestion}
    on:deleted={deleteQuestion} />
</section>
