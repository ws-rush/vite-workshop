<script lang="ts">
	import Section from './outline-section.svelte';
	import Link from './outline-link.svelte';
</script>

<nav class="flex flex-col gap-8">
	<Section title="Getting Started">
		<Link href="/introduction">Introduction</Link>
		<Link href="/why-vite">Why Vite?</Link>
		<Link href="/core-features">Core Features</Link>
		<Link href="/basic-setup">Basic Setup</Link>
	</Section>
	<Section title="Styling with Vite">
		<Link href="/css">Working with CSS</Link>
		<Link href="/postcss">PostCSS</Link>
		<Link href="/sass">Using Sass with Vite</Link>
	</Section>
	<Section title="Assets">
		<Link href="/static-assets">Working with Static Assets</Link>
		<Link href="/assets-include">assetsInclude</Link>
		<Link href="/image-tools">Vite Image Tools</Link>
	</Section>
	<Section title="Importing">
		<Link href="/import-meta">import.meta</Link>
		<Link href="/json-named-exports">JSON Named Exports</Link>
		<Link href="/dynamic-imports">Dynamic and Conditional Imports</Link>
		<Link href="/glob-import">Glob Import</Link>
		<Link href="/web-workers">Web Workers</Link>
	</Section>
	<Section title="Environments and Build Modes">
		<Link href="/environments-and-modes">Build Modes</Link>
		<Link href="/environment-variables">Environment Variables</Link>
	</Section>
	<Section title="TypeScript and Frameworks">
		<Link href="/typescript">Vite with TypeScript</Link>
		<Link href="/frameworks">Vite with Frameworks</Link>
		<Link href="/templates">Project Templates</Link>
		<Link href="/mpa">Multi Page App</Link>
	</Section>
	<Section title="Building Libraries">
		<Link href="/library-mode">Library Mode</Link>
		<Link href="/library-mode-components">Library Mode with Components</Link>
		<Link href="/multiple-entries-library">Multiple Entries Library</Link>
		<Link href="/module-federation">Module Federation</Link>
	</Section>
	<Section title="Server Integration">
		<Link href="/dev">Development Server</Link>
		<Link href="/preview">Preview Server</Link>
		<Link href="/proxying">Proxying API Requests</Link>
		<Link href="/create-vite-server">createViteServer</Link>
		<Link href="/middleware">Types of Middleware Mode</Link>
		<Link href="/ssr">Server-Side Rendering</Link>
	</Section>
	<Section title="Plugins and Templates">
		<Link href="/plugins">Plugins</Link>
		<Link href="/plugins-virtual-modules">Virtual Modules</Link>
		<Link href="/plugin-inline-css">Inline CSS Plugin</Link>
		<Link href="/plugin-markdown">Markdown Plugin</Link>
		<Link href="/hot-module-replacement">Hot Module Replacement</Link>
		<Link href="/unplugin">Unplugin</Link>
	</Section>
	<Section title="Appendix">
		<Link href="/css-and-typescript">CSS and TypeScript</Link>
		<Link href="/dependency-prebundling">Dependency Pre-Bundling</Link>
		<Link href="/link-and-force-optimization">Link and Force Optimization</Link>
		<Link href="/optimize-deps">Optimize Dependencies</Link>
		<Link href="/esbuild-options">ESBuild Options</Link>
		<Link href="/preserve-modules">preserveModules</Link>
		<Link href="/configuration">Vite Configuration</Link>
	</Section>
</nav>
