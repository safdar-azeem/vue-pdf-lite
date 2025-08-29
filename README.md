# vue-pdf-lite

A powerful Vue.js composable for generating high-quality PDFs from HTML content with intelligent page breaks, customizable styling, and responsive design support.

## Features

-   **Smart Page Breaks**: Intelligent pagination that respects content structure and prevents orphaned content
-   **Customizable PDF Options**: Control margins, font size, quality, and page orientation
-   **Vue 3 Composition API**: Clean, modern API using Vue 3 composables
-   **Content Grouping**: Automatic content sectioning for better page break handling
-   **Multiple Output Options**: Download PDFs or trigger print dialogs
-   **High Performance**: Optimized rendering with configurable quality settings
-   **TypeScript Support**: Full TypeScript support with comprehensive type definitions

## Installation

```bash
# npm
npm install vue-pdf-lite

# yarn
yarn add vue-pdf-lite

# pnpm
pnpm add vue-pdf-lite
```

## Basic Usage

```vue
<script setup lang="ts">
import { usePdfExport } from 'vue-pdf-lite'

const { handleExportPdf, loading, pdfSection } = usePdfExport()

const exportToPdf = async () => {
	await handleExportPdf({
		fileName: 'my-document',
		download: true,
		print: false, // Whether to open browser print dialog
		margins: [20, 20, 20, 20], // [top, right, bottom, left]
		fontSize: 16,
		quality: 'balanced', // 'high' | 'balanced' | 'compressed'
	})
}
</script>

<template>
	<div>
		<button
			@click="exportToPdf"
			:disabled="loading"
			class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
			{{ loading ? 'Generating...' : 'Export PDF' }}
		</button>

		<!-- Content to be exported -->
		<div
			ref="pdfSection"
			class="pdf-content">
			<h1>My Document</h1>
			<p>
				This content will be converted to PDF with intelligent page
				breaks.
			</p>

			<h2>Features</h2>
			<ul>
				<li>Smart pagination</li>
				<li>Custom styling</li>
				<li>Responsive design</li>
			</ul>

			<table>
				<thead>
					<tr>
						<th>Feature</th>
						<th>Status</th>
					</tr>
				</thead>
				<tbody>
					<tr>
						<td>PDF Export</td>
						<td>✅ Available</td>
					</tr>
					<tr>
						<td>Print Support</td>
						<td>✅ Available</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>
```

## Advanced Usage

### Custom Export Options

```vue
<script setup lang="ts">
import { usePdfExport } from 'vue-pdf-lite'

const { handleExportPdf, loading, pdfSection } = usePdfExport()

const exportWithCustomOptions = async () => {
	await handleExportPdf({
		fileName: 'custom-document',
		download: true,
		print: false,
		margins: [30, 25, 30, 25], // Custom margins
		fontSize: 14,
		quality: 'balanced', // 'high' | 'balanced' | 'compressed'
		pageWidth: 800, // Custom page width
		onSuccess: (blob) => {
			console.log('PDF generated successfully!', blob)
			// Handle the blob (e.g., upload to server)
		},
	})
}
</script>
```

### Print-Only Mode

```vue
<script setup lang="ts">
const printDocument = async () => {
	await handleExportPdf({
		fileName: 'document',
		download: false,
		print: true, // Opens print dialog
		margins: [0, 0, 0, 0], // No margins for printing
	})
}
</script>
```

### Using Custom PDF Section

```vue
<script setup lang="ts">
import { ref } from 'vue'
import { usePdfExport } from 'vue-pdf-lite'

const customPdfRef = ref<HTMLElement>()
const { handleExportPdf, loading } = usePdfExport()

const exportCustomSection = async () => {
	await handleExportPdf({
		fileName: 'custom-section',
		download: true,
		pdfSection: customPdfRef.value, // Use custom element
	})
}
</script>

<template>
	<div>
		<button @click="exportCustomSection">Export Custom Section</button>

		<div ref="customPdfRef">
			<!-- This specific content will be exported -->
			<h2>Custom Content</h2>
			<p>Only this section will be included in the PDF.</p>
		</div>

		<div>
			<!-- This content will NOT be exported -->
			<p>This content is outside the custom section.</p>
		</div>
	</div>
</template>
```

## Page Break Intelligence

The library includes sophisticated page break logic:

-   **Smart Heading Placement**: H1 elements avoid being placed in the bottom 35% of pages
-   **Widow/Orphan Prevention**: H2-H5 elements avoid the last 15% of pages
-   **Content Sectioning**: Related content is grouped and kept together when possible
-   **Table Handling**: Large tables are split intelligently or moved to new pages
-   **List Management**: Lists are broken appropriately while maintaining structure

## License

MIT License - see LICENSE file for details.

## Author

[safdar-azeem](https://github.com/safdar-azeem)
