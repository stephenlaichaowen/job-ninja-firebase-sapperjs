<script context="module">
  export async function preload(page, session) {
    const res = await this.fetch("https://sapper-tutorial.firebaseio.com/jobs.json")
    const jobs = await res.json();
    const jobsArray = [];
    for (const key in jobs) {
      jobsArray.push({ ...jobs[key], id: key });
    }
    return { jobsArray };
  }
</script>

<script>
  export let jobsArray;
</script>

<style>
  .container {
    max-width: 650px;
    margin: auto;
    padding: 0 1rem;
  }
  ul {
    margin: 0;
    padding: 0;
  }
  li {
    list-style: none;
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #f2f2f2;
  }
  li a {
    display: inline-block;
    padding: 15px;
    border-radius: 8px;
    text-decoration: none;
  }
  li a:hover {
    background: #fbfbfb;
  }
  .btn-wrapper {
    text-align: center;
    margin-top: 2rem;
  }
</style>

<svelte:head>
  <title>Job Lists</title>
</svelte:head>

<div>
  <div class="container animate__animated animate__fadeIn">
    <h1>All Current Jobs</h1>
    <ul>
      <!-- {#each jobsArray as item} -->
      {#each jobsArray as item}
        <li>
          <a rel="prefetch" href={`jobs/${item.id}`}>{item.title}</a>
        </li>
      {/each}
    </ul>
    <div class="btn-wrapper">
      <a href="jobs/create" class="btn">Add a New Job</a>
    </div>
  </div>
</div>
