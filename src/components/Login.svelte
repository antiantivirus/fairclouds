<script>
  import { createDirectus, authentication, rest, login } from "@directus/sdk";

  export let lang;

  let email = "";
  let password = "";
  let error = "";
  let isLoggedIn = false;

  const client = createDirectus("https://cms.fairclouds.life").with(
    authentication("session", { credentials: "include" }),
  );

  const handleLogin = async () => {
    try {
      await client.login({ email, password });
      isLoggedIn = true;
      error = "";
    } catch (err) {
      error = err.message || "Invalid login credentials";
      isLoggedIn = false;
    }
  };
</script>

{#if isLoggedIn}
  <p>You are logged in!</p>
  <a href="/en/dashboard">Go to dashboard</a>
{:else}
  <form on:submit|preventDefault={handleLogin}>
    <input type="email" bind:value={email} placeholder="Email" required />
    <input
      type="password"
      bind:value={password}
      placeholder="Password"
      required
    />
    {#if error}
      <p>{error}</p>
    {/if}
    <!-- to do: use proper translation function -->
    <button type="submit">{lang == "en" ? "Login" : "Acceso"}</button>
  </form>
{/if}
