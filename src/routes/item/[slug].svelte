<script context="module">
  export async function preload({ params, query }) {
    // the `slug` parameter is available because
    // this file is called [slug].html
    const res = await this.fetch(`item/${params.slug.toLowerCase()}.json`);
    const data = await res.json();

    return {
      post: data
    };
  }
</script>

<script>
  import { onMount } from "svelte";
  onMount(async () => {
    gtag("config", "UA-93549235-3", { page_path: window.location.pathname });
  });
  export let post;
</script>

<style>
  article.postFull {
    margin-bottom: 3rem;
  }

  a.backLink {
    display: inline-block;
    margin: 1.5rem 0;
  }
  div.subtitle {
    font-size: 15px;
    margin-bottom: 1.2rem;
    color: var(--subtitle);
  }

  div.imageWrapper {
    max-width: 600px;
    max-height: 400px;
    margin: 1.5rem auto 2rem;
    width: 100%;
    overflow: hidden;
    border: 1px solid var(--sidebarBorder);
    padding: 0;
    border-radius: 4px;
    overflow: hidden;
    box-shadow: 0px 2px 2px 0px rgba(20, 20, 20, 0.1);
  }

  div.imageWrapper img {
    max-width: 100%;
    max-height: 100%;
    padding: 0;
    margin: 0;
    display: block;
  }

  div.embedWrapper {
    max-width: 90%;
    overflow: hidden;
    margin: 0 auto;
    display: flex;
    justify-content: center;
  }
</style>

<svelte:head>
  <link rel="canonical" href="https://rosshill.ca/item/{post.slug}" />
</svelte:head>
<div class="contentWrapper mainContent">
  <a rel="prefetch" class="backLink" href="/#timeline-item-{post.slug}">
    &larr; Back to Home
  </a>
  <article class="postFull">
    <h1>{post.title}</h1>
    <div class="subtitle">
      Started {post.date}
      {#if post.repository}
        &ndash;
        <a
          on:click={() => gtag('event', 'outbound', {
              event_label: post.repository
            })}
          target="_blank"
          rel="noopener noreferrer"
          href={post.repository}>
          GitHub
        </a>
      {/if}
      {#if post.website}
        &ndash;
        <a
          on:click={() => gtag('event', 'outbound', {
              event_label: post.website
            })}
          target="_blank"
          rel="noopener noreferrer"
          href={post.website}>
          Website
        </a>
      {/if}
    </div>
    {#if post.gif}
      <img src="timeline/{post.gif}.gif" alt={post.title} />
    {/if}
    {#if post.image}
      <div class="imageWrapper">
        <picture>
          <source srcset="timeline/{post.image}.webp" type="image/webp" />
          <source
            srcset="timeline/{post.image}.{post.imageExt || 'png'}"
            type="image/{post.imageExt || 'png'}" />
          <img
            src="timeline/{post.image}.{post.imageExt || 'png'}"
            alt={post.title} />
        </picture>
      </div>
    {/if}
    {#if post.embed}
      <div class="embedWrapper">
        {@html post.embed}
      </div>
    {/if}

    <div class="content">
      {@html post.content}
    </div>
  </article>
</div>
