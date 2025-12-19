<script>
	const techStack = [
		{
			category: 'Languages',
			technologies: ['Python', 'C++', 'Go', 'JavaScript', 'Dart', 'Solidity']
		},
		{
			category: 'Frontend',
			technologies: ['React', 'Flutter']
		},
		{
			category: 'Backend',
			technologies: ['Django', 'Node.js']
		},
		{
			category: 'Data & ML',
			technologies: ['PyTorch', 'TensorFlow']
		},
		{
			category: 'Infrastructure & DevOps',
			technologies: [
				'Git',
				'Docker',
				'AWS EKS',
				'Terraform',
				'ArgoCD',
				'Istio',
				'Redis',
				'Kong',
				'PostGIS',
				'Linux',
				'Azure',
				'Firebase',
				'PostgreSQL'
			]
		}
	];
</script>

<div class="space-y-16">
	<!-- Header -->
	<div class="space-y-3">
		<h1 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
			Technical Expertise
		</h1>
		<h2
			class="text-3xl leading-tight font-light tracking-tight md:text-4xl"
			style="color: #111827;"
		>
			Tech Stack
		</h2>
	</div>

	<!-- Tech Categories -->
	<div class="grid grid-cols-1 gap-x-12 gap-y-12 md:grid-cols-2">
		{#each techStack as stack}
			<div class="group">
				<h3
					class="mb-4 text-[12px] font-light tracking-[0.15em] uppercase transition-colors duration-300"
					style="color: #6b7280;"
				>
					{stack.category}
				</h3>
				<div class="flex flex-wrap gap-2">
					{#each stack.technologies as tech}
						<span
							class="rounded-full px-4 py-2 font-mono text-[13px] tracking-wide transition-all duration-300"
							style="border: 1px solid #e5e7eb; color: #6b7280;"
						>
							{tech}
						</span>
					{/each}
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	div {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}
</style>
