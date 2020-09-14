<script context="module">
  export async function preload(page, session) {
    const { id } = page.params;
    const res = await this.fetch(
      `https://sapper-tutorial.firebaseio.com/jobs/${id}.json`
    );
    const job = await res.json();
    return { job, id };
  }
</script>

<script>
  import { goto } from "@sapper/app";

  export let job
  export let id

  let isEditMode = false;
  let btnText = "";

  const handleDelete = async () => {
    const res = await fetch(
      `https://sapper-tutorial.firebaseio.com/jobs/${id}.json`,
      {
        method: "DELETE"
      }
    );
    // const result = await res.json();
    // console.log(result);

    goto("jobs");
  };

  const handleEdit = () => {
    isEditMode = true;
  };

  const handleSave = async () => {
    let title = job.title;
    let salary = job.salary;
    let details = job.details;
    if (title && salary && details) {
      const res = await fetch(
        `https://sapper-tutorial.firebaseio.com/jobs/${id}.json`,
        {
          method: "PATCH",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ title, salary, details })
        }
      );
      const updatedJobs = await res.json();
      // console.log(updatedJobs);
      goto("jobs");
    }
  };

  const handleBack = () => {
    goto("jobs");
  };

  const handleCancel = () => {
    goto("jobs");
  };
</script>

<style>
  .container {
    display: flex;
    justify-content: center;
  }

  .btn-group {
    margin-top: 2rem;
  }

  .btn {
    margin-right: 1rem;
    outline: none;
  }

  input {
    display: block;
    width: 100%;
    padding: 10px;
    font-family: arial;
    margin: 10px auto;
    border: none;
    border-bottom: 1px solid #eee;
    outline: none;
  }
</style>

<svelte:head>
  <title>Job Details</title>
</svelte:head>

<div>
  <div class="container animate__animated animate__fadeIn">
    {#if isEditMode}
      <div class="job">
        <label for="title">Title</label>
        <input id="title" class="title" type="text" bind:value={job.title} />
        <label for="salary">Salary</label>
        <input
          id="salary"
          class="salary"
          type="number"
          bind:value={job.salary} />
        <label for="details">Details</label>
        <input
          id="details"
          class="details"
          type="text"
          bind:value={job.details} />
        <div class="btn-group">
          <button class="btn" on:click={handleSave}>Save</button>
          <button class="btn" on:click={handleCancel}>Cancel</button>
        </div>
      </div>
    {:else}
      <div class="job">
        <h2>{job.title}</h2>
        <p>Salary of ${job.salary}</p>
        <p>{job.details}</p>
        <div class="btn-group">
          <button class="btn" on:click={handleEdit}>Edit</button>
          <button class="btn" on:click={handleDelete}>Delete</button>
          <button class="btn" on:click={handleBack}>Back</button>
        </div>
      </div>
    {/if}
  </div>
</div>
