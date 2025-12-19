<script>
	const hobbies = [
		{
			name: 'Fishing',
			description:
				'Passionate about the outdoors and fishing. Developed an AI-powered fishing guide to help others identify fish and find the best spots.',
			focus: ['Outdoors', 'Nature', 'Conservation']
		},
		{
			name: 'Outdoor Adventures',
			description:
				'Finding balance through physical challenges and nature. Whether scaling walls, carving snow, or camping under the stars.',
			focus: ['Wall Climbing', 'Snowboarding', 'Camping']
		}
	];
</script>

<div class="space-y-16">
	<!-- Header -->
	<div class="space-y-3">
		<h1 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
			Beyond Code
		</h1>
		<h2
			class="text-3xl leading-tight font-light tracking-tight md:text-4xl"
			style="color: #111827;"
		>
			Hobbies & Interests
		</h2>
	</div>

	<!-- Hobbies List -->
	<div class="space-y-12">
		{#each hobbies as hobby, i}
			<article class="group relative">
				<!-- Number -->
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
					{hobby.name}
				</h3>

				<!-- Description -->
				<p
					class="mb-6 max-w-2xl text-[15px] leading-relaxed font-light transition-colors duration-300"
					style="color: #4b5563;"
				>
					{hobby.description}
				</p>

				<!-- Focus Pills -->
				<div class="flex flex-wrap gap-2">
					{#each hobby.focus as item}
						<span
							class="rounded-full px-3 py-1.5 font-mono text-[11px] tracking-wide transition-all duration-300"
							style="border: 1px solid #e5e7eb; color: #6b7280;"
						>
							{item}
						</span>
					{/each}
				</div>
			</article>
		{/each}
	</div>
</div>

<style>
	article {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}
</style>
