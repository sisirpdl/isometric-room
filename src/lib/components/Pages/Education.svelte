<script>
	const education = [
		{
			degree: 'BS in Computer Science — AI Specialization',
			institution: 'Dakota State University',
			period: 'Dec 2026',
			gpa: '3.84',
			focus: [
				'Advanced Data Structures & Algorithms',
				'Database Management',
				'Mathematical Foundations of AI',
				'Artificial Intelligence',
				'Software Engineering',
				'Parallel Computing',
				'Programming Languages'
			],
			awards: [
				'1st Place, FBLA Nationals - Computer Science (Jun 2025)',
				'Outstanding Undergraduate Research Award (Mar 2024)',
				'Math Olympiad Finalist, Nepal (Feb 2022)'
			]
		}
	];
</script>

<div class="space-y-16">
	<!-- Header -->
	<div class="space-y-3">
		<h1 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
			Academic Background
		</h1>
		<h2
			class="text-3xl leading-tight font-light tracking-tight md:text-4xl"
			style="color: #111827;"
		>
			Education
		</h2>
	</div>

	<!-- Degrees -->
	<div class="space-y-12">
		{#each education as edu, i}
			<article class="group relative">
				<!-- Number -->
				<div class="absolute top-1 -left-12 hidden md:block">
					<span class="font-mono text-[10px] tracking-wider" style="color: #d1d5db;">
						{String(i + 1).padStart(2, '0')}
					</span>
				</div>

				<div class="mb-4">
					<h3
						class="mb-1 text-xl font-light tracking-tight transition-colors duration-300 md:text-2xl"
						style="color: #111827;"
					>
						{edu.degree}
					</h3>
					<div class="flex items-center gap-3 text-[13px]" style="color: #6b7280;">
						<span>{edu.institution}</span>
						<span style="color: #d1d5db;">•</span>
						<span>{edu.period}</span>
						<span style="color: #d1d5db;">•</span>
						<span class="font-mono">{edu.gpa}/4.0</span>
					</div>
				</div>

				<div class="mb-6 flex flex-wrap gap-2">
					{#each edu.focus as item}
						<span
							class="rounded-full px-3 py-1.5 font-mono text-[11px] tracking-wide transition-all duration-300"
							style="border: 1px solid #e5e7eb; color: #6b7280;"
						>
							{item}
						</span>
					{/each}
				</div>

				{#if edu.awards}
					<div class="space-y-2">
						<h4 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
							Awards
						</h4>
						<ul class="space-y-1">
							{#each edu.awards as award}
								<li class="text-[13px]" style="color: #4b5563;">
									{award}
								</li>
							{/each}
						</ul>
					</div>
				{/if}
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
