<script lang="ts">
  import {
    Row,
    Button,
    ToastNotification,
    Column,
  } from "carbon-components-svelte";
  import Add16 from "carbon-icons-svelte/lib/Add16";
  import { cardMappingStore } from "../../stores/settingsStore";
  import type { ICardMapping } from "../../stores/settingsStore";
  import MappingCreator from "./MappingCreator.svelte";
  import MappingAddModal from "./MappingAddModal.svelte";

  let cardMappings: ICardMapping[];
  cardMappingStore.subscribe((value) => (cardMappings = value));

  let modalOpen: boolean = false;
</script>

{#if cardMappings?.length > 0}
  <Row>
    {#each cardMappings as mapping}
      <Column md={6}>
        <MappingCreator mappingName={mapping.mappingName} />
      </Column>
    {/each}
  </Row>
{:else}
  <Row>
    <ToastNotification
      lowContrast
      kind="info-square"
      hideCloseButton
      title="No Mappings Defined"
      caption="Click Add New to create a model mapping."
    />
  </Row>
{/if}

<Row style="margin-top: 15px">
  <Button
    hasIconOnly
    iconDescription={"Add New"}
    tooltipPosition="top"
    icon={Add16}
    on:click={() => {
      modalOpen = true;
    }}
  />
</Row>

<MappingAddModal
  open={modalOpen}
  onSubmitted={(mappingName) => {
    modalOpen = false;
    if (mappingName !== undefined) {
      cardMappingStore.update((value) => [
        ...value,
        {
          deckName: "",
          mappingName: mappingName,
          modelName: "",
          modelFieldMappings: {},
        },
      ]);
    }
  }}
/>
