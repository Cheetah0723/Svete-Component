<svelte:options tag="formrenderer-component" />

<script lang="ts">
	/**
	 * Svelte Video Gallery WebComponent
	 * =====================
	 *
	 * @contributors: Dario Caruso <dev@dariocaruso.info> (https://dariocaruso.info)
	 *
	 * @license: MIT License
	 *
	 */
	import { get_current_component } from "svelte/internal";
	import { createEventDispatcher } from "svelte";

	import type { FormSchema } from "@app/types/webcomponent.type";
	import FormSchemaRenderer from "./FormSchemaRenderer.svelte";

	export let schema: FormSchema;
	$: schema = typeof schema === "string" ? JSON.parse(schema) : schema;

	const component = get_current_component();

	const link = document.createElement("link");
	link.rel = "stylesheet";
	link.href = "https://cdn.jsdelivr.net/npm/bootstrap@5.1.1/dist/css/bootstrap.min.css";
	component.shadowRoot.prepend(link);

	const svelteDispatch = createEventDispatcher();

	let values: Record<string, string | number | boolean>;
	let isInvalid = true;

	const onSubmit = () => {
		svelteDispatch("submit", values);
		component.dispatchEvent?.(new CustomEvent("submit", { detail: values }));
	};
</script>

{#if schema}
	<FormSchemaRenderer {schema} bind:values bind:isInvalid />
	<button type="button" class="btn btn-primary" disabled={isInvalid} on:click|preventDefault={onSubmit}><slot name="submit-label">Submit</slot></button>
{/if}

<style lang="scss">
	@import "../styles/webcomponent.scss";
</style>
