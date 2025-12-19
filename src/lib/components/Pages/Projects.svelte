<script>
	const projects = [
		{
			title: 'Fishing Guide (AI-Powered Outdoor Companion)',
			bullets: [
				'Architecting a high-performance backend using Go and Kong Gateway to handle 100,000+ concurrent users with sub-50ms latency, utilizing Redis for caching and PostGIS for geospatial queries across 15,000+ water bodies',
				'Engineering a computer vision pipeline using PyTorch, EfficientNetV2, and Triton Inference Server to classify 250+ fish species with <500ms latency, integrating pgvector for semantic search of fishing techniques',
				'Deploying multi-region cloud infrastructure on AWS EKS using Terraform and ArgoCD for GitOps, implementing Istio service mesh to ensure 99.95% system availability and zero-downtime deployments'
			],
			link: '',
			tags: [
				'Go',
				'Flutter',
				'AWS EKS',
				'Kong Gateway',
				'Redis',
				'PostGIS',
				'PyTorch',
				'Terraform',
				'ArgoCD',
				'Istio'
			]
		},
		{
			title: 'Advanced Text Analyzer',
			bullets: [
				'Increased throughput 24% on 1 GB files by engineering a text analyzer using Tries (prefix search), Hash Tables (O(1) lookups), and a Binomial Heap (top-k extraction)'
			],
			link: 'https://github.com/sisirpdl/advance-dsa-sp25/tree/main/100_final_project',
			tags: ['C++', 'Tries', 'Hash Tables', 'Binomial Heap']
		},
		{
			title: 'Sticky Notes App (Meta Hackathon)',
			bullets: [
				'Leveraged cutting-edge IWSDK tools and immersive AR/VR technologies to engineer a spatial sticky notes application for the Meta Quest ecosystem',
				'Implemented high-fidelity hand grab interactions and precise ray casting algorithms to enable intuitive, physics-based object manipulation in 3D space'
			],
			link: '',
			tags: ['IWSDK', 'AR/VR', 'Meta Quest', 'Ray Casting']
		},
		{
			title: 'Literacy Assessment Platform for Elementary Teachers',
			bullets: [
				'Implemented a polymorphic assessment engine using Django Abstract Classes to support multiple testing standards (DIBELS, AIMSweb), facilitating seamless zero-downtime content updates and long-term maintainability',
				'Executed a DAG decision tree service to map deficits to interventions, decoupling business logic from the codebase to allow JSON-based editing which significantly accelerated iteration on diagnostic criteria',
				'Utilized React to develop a schema-driven UI based on API definitions that dynamically adapts to backend models, enhancing user experience and resulting in a 40% reduction in frontend boilerplate code'
			],
			link: 'https://github.com/sisirpdl',
			tags: ['Django', 'React', 'PostgreSQL']
		},
		{
			title: 'LlamaAnimate: Real-Time AI Lip-Sync',
			bullets: [
				'Orchestrated a concurrent inference pipeline integrating LLaMA 2 (via Ollama) and Piper TTS to parallelize text generation and audio synthesis, achieving sub-1000ms latency for seamless, real-time live interactions',
				'Optimized Wav2Lip model inference by embedding intelligent audio chunk buffering to maintain precise visual lip synchronization with dynamic audio streams, ensuring a fluid, cohesive user experience for interactive AI agents'
			],
			link: 'https://github.com/sisirpdl/LlamaAnimate',
			tags: ['LLaMA 2', 'Ollama', 'Docker', 'Python']
		},
		{
			title: 'A-Level Notes App',
			bullets: [
				'Deployed a scalable Flutter app integrated with Firebase Cloud Firestore to sync notes in real-time across devices, providing robust offline-first access and data persistence to over 10,000+ active users',
				'Embedded a local search index using efficient string matching algorithms to enable instant topic querying without network latency, significantly optimizing student study time and contributing to a 4.8-star average rating'
			],
			link: 'https://github.com/sisirpdl/alevelnotes',
			tags: ['Flutter', 'Firebase', 'Google Play']
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
