<script lang="ts">
  import Panels from "$lib/Panels.svelte"
  import { Button, Overlay } from "ftc-panels"
  import { global } from "$lib"
  import { goto } from "$app/navigation"
</script>

<nav>
  <a href="/">
    <Panels />
  </a>

  <p>
    {global.isConnected ? "Connected" : "Waiting"}
  </p>

  <Overlay>
    {#snippet trigger({ isOpen }: { isOpen: boolean })}
      Plugins
    {/snippet}
    {#snippet overlay({ close }: { close: () => void })}
      <div class="plugins-overlay">
        <Button
          onclick={() => {
            goto("/plugins")
            close()
          }}>Details</Button
        >
        <div class="grid">
          {#each global.plugins as plugin}
            <button
              class="plugin"
              onclick={() => {
                goto(`/plugins/${plugin.details.id}`)
                close()
              }}
            >
              {plugin.details.letterName}
            </button>
          {/each}
        </div>
        {#each global.skippedPlugins as details}
          <button class="plugin" disabled onclick={close}>
            {details.letterName}
          </button>
        {/each}
      </div>
    {/snippet}
  </Overlay>
</nav>

<style>
  .plugins-overlay {
    min-width: 200px;
    min-height: 100px;

    padding: calc(var(--padding) / 2);
  }
  .grid {
    margin-top: 0.5rem;

    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(32px, auto));
    gap: calc(var(--padding) / 2);
  }

  button.plugin {
    background-color: transparent;
    color: inherit;
    margin: 0;
    border: 1px solid var(--bgLight);
    padding: calc(var(--padding) / 2);
    border-radius: 69vw;
    width: 32px;
    height: 32px;
    display: grid;
    place-content: center;
    cursor: pointer;
  }
  nav {
    background-color: var(--bgMedium);
    padding: 0 var(--padding);

    display: flex;
    justify-content: space-between;
    align-items: center;

    border-radius: 1rem;
    margin: 0.5rem;
    margin-bottom: 0;
  }
</style>
