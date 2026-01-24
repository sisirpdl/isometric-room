<script>
	const projects = [
		{
			title: 'Enterprise Kanban Board',
			bullets: [
				'Created a real-time Kanban board application using .NET 8 and Clean Architecture principles, achieving 100% business logic isolation to facilitate comprehensive unit and integration testing',
				'Enabled real-time synchronization using SignalR to resolve race conditions during concurrent drag-and-drop updates, maintaining sub-42 ms latency for 300 simulated users',
				'Implemented the Request-Endpoint-Response (REPR) pattern to structure modular API workflows, delivering a functional Trello-style UI with instant activity logging'
			],
			link: 'https://github.com/sisirpdl',
			tags: ['.NET 8', 'SignalR', 'C#', 'Clean Architecture']
		},
		{
			title: 'Literacy Assessment Platform for Elementary Teachers',
			bullets: [
				'Led a 6-person Agile team to deploy a web app that automates scoring for literacy assessments, replacing static spreadsheets with a system that tracks historical progress and groups students for immediate academic support',
				'Structured a data model using Django ORM to track student history, automatically grouping students by skill level to help teachers formulate targeted intervention strategies daily',
				'Coded React forms that auto-generate based on database configuration, allowing teachers to create new assessment formats without requiring developer intervention'
			],
			link: 'https://github.com/sisirpdl',
			tags: ['Django', 'React', 'PostgreSQL']
		},
		{
			title: 'Sticky Notes App (Meta Hackathon)',
			bullets: [
				'Leveraged cutting-edge IWSDK tools and immersive AR/VR technologies to engineer a spatial sticky notes application for the Meta Quest ecosystem',
				'Implemented high-fidelity hand grab interactions and precise ray casting algorithms to enable intuitive spatial interaction and object placement in 3D space'
			],
			link: '',
			tags: ['IWSDK', 'AR/VR', 'Meta Quest', 'Ray Casting']
		},
		{
			title: 'LLM-powered Conversational Avatar',
			bullets: [
				'Collaborated within a 5-person Agile team to build an interactive virtual avatar that uses LLM-generated speech to drive accurate lip-sync and expressive facial movements, completing 28 user stories',
				'Reduced latency by 82% under 1000 ms by assembling a concurrent inference pipeline, merging LLaMA2, Wav2Lip, and Text-To-Speech in a docker environment',
				'Optimized system performance and Wav2Lip inference by implementing concurrent processing techniques (intelligent audio buffer), maximizing throughput to achieve 30 frames per second (FPS) processing speed'
			],
			link: 'https://github.com/sisirpdl/LlamaAnimate',
			tags: ['LLaMA 2', 'Python', 'Docker', 'Wav2Lip']
		},
		{
			title: 'A-Level Notes App',
			bullets: [
				'Launched a Flutter app on the Play Store that syncs notes and past papers via Firebase, serving over 10,000 users',
				'Designed a responsive UI with instant search capabilities, allowing students to find Math and Physics topics immediately and contributing to a 4.8-star average rating'
			],
			link: 'https://github.com/sisirpdl/alevelnotes',
			tags: ['Flutter', 'Firebase', 'Google Play']
		},
		{
			title: 'Advanced Text Analyzer',
			bullets: [
				'Increased throughput 24% on 1 GB files by engineering a text analyzer using Tries (prefix search), Hash Tables (O(1) lookups), and a Binomial Heap (top-k extraction)'
			],
			link: 'https://github.com/sisirpdl/advance-dsa-sp25/tree/main/100_final_project',
			tags: ['C++', 'Tries', 'Hash Tables', 'Binomial Heap']
		}
	];
</script>

<div class="space-y-16">
	<!-- Header -->
	<div class="space-y-3">
		<h1 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
			Selected Work
		</h1>
		<h2
			class="text-3xl leading-tight font-light tracking-tight md:text-4xl"
			style="color: #111827;"
		>
			Projects
		</h2>
	</div>

	<!-- Projects List -->
	<div class="space-y-12">
		{#each projects as project, i}
			<article class="group relative">
				<!-- Project Number -->
				<div class="absolute top-1 -left-12 hidden md:block">
					<span class="font-mono text-[10px] tracking-wider" style="color: #d1d5db;">
						{String(i + 1).padStart(2, '0')}
					</span>
				</div>

				<!-- Title -->
				<h3
					class="mb-3 text-xl font-light tracking-tight transition-colors duration-300 md:text-2xl"
					style="color: #111827;"
				>
					{project.title}
				</h3>

				<!-- Description -->
				<ul class="mb-6 max-w-2xl space-y-2">
					{#each project.bullets as bullet}
						<li
							class="flex gap-3 text-[15px] leading-relaxed font-light transition-colors duration-300"
							style="color: #4b5563;"
						>
							<span class="mt-2 h-1 w-1 shrink-0 rounded-full" style="background: #9ca3af;"></span>
							<span>{bullet}</span>
						</li>
					{/each}
				</ul>

				<!-- Tech Stack Pills -->
				<div class="mb-6 flex flex-wrap gap-2">
					{#each project.tags as tag}
						<span
							class="rounded-full px-3 py-1.5 font-mono text-[11px] tracking-wide transition-all duration-300"
							style="border: 1px solid #e5e7eb; color: #6b7280;"
						>
							{tag}
						</span>
					{/each}
				</div>

				<!-- Link -->
				{#if project.link}
					<a
						href={project.link}
						target="_blank"
						rel="noopener noreferrer"
						class="project-link inline-flex items-center gap-2 text-[12px] tracking-[0.15em] uppercase transition-colors duration-300"
					>
						View Project
						<svg
							width="10"
							height="10"
							viewBox="0 0 12 12"
							fill="none"
							stroke="currentColor"
							stroke-width="1"
							class="translate-y-[-1px] transition-transform duration-300"
						>
							<path d="M1 11L11 1M11 1H1M11 1V11" />
						</svg>
					</a>
				{/if}
			</article>
		{/each}
	</div>
</div>

<style>
	/* Ensure ultra-clean typography */
	article {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}

	/* Link styling */
	.project-link {
		color: #9ca3af;
	}

	/* Hover effects for link */
	.project-link:hover {
		color: #2563eb;
	}

	/* Arrow animation on link hover */
	.project-link:hover svg {
		transform: translate(2px, -2px);
	}
</style>
