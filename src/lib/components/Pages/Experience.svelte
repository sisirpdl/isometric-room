<script>
	const experiences = [
		{
			title: 'Security Software Engineering Intern',
			company: 'Google Consortium of Cyber Clinics',
			period: 'Aug 2024 - Present',
			bullets: [
				'Fine-tuning Microsoft Phi-3 to engineer Modular Agents that enhances adaptability towards natural language commands, enabling automated threat hunting across Sentinel log streams, reducing investigation time by 40%',
				'Developing Python pipelines to automate and validate the remediation of Tier-1 alerts, reducing manual effort by 80% for high-volume events and recovering over 15 hours of analyst time per week',
				'Leveraging real-time threat intelligence from VirusTotal and Microsoft Graph to efficiently inject context into alerts, resolving 15+ recurring cases weekly by transforming signatures into auto-remediation rules'
			],
			technologies: ['Microsoft Phi-3', 'Python', 'Sentinel', 'VirusTotal', 'Microsoft Graph']
		},
		{
			title: 'Computer Science Researcher',
			company: 'Dakota State University',
			period: 'Aug 2023 - Feb 2024',
			bullets: [
				'Awarded "Best Undergraduate Research" at the South Dakota State Capitol for excellence in blockchain analysis',
				'Critically evaluated blockchain architectures for secure electronic voting by benchmarking 10+ consensus protocols against data integrity models, synthesizing technical trade-offs into actionable policy recommendations'
			],
			technologies: ['Blockchain', 'Consensus Protocols', 'Data Integrity Models']
		}
	];
</script>

<div class="space-y-16">
	<!-- Header -->
	<div class="space-y-3">
		<h1 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
			Professional Journey
		</h1>
		<h2
			class="text-3xl leading-tight font-light tracking-tight md:text-4xl"
			style="color: #111827;"
		>
			Experience
		</h2>
	</div>

	<!-- Experience Timeline -->
	<div class="space-y-12">
		{#each experiences as exp, i}
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
						{exp.title}
					</h3>
					<div class="flex items-center gap-3 text-[13px]" style="color: #6b7280;">
						<span>{exp.company}</span>
						<span style="color: #d1d5db;">•</span>
						<span>{exp.period}</span>
					</div>
				</div>

				<ul class="mb-6 max-w-2xl space-y-2">
					{#each exp.bullets as bullet}
						<li
							class="flex gap-3 text-[15px] leading-relaxed font-light transition-colors duration-300"
							style="color: #4b5563;"
						>
							<span class="mt-2 h-1 w-1 shrink-0 rounded-full" style="background: #9ca3af;"></span>
							<span>{bullet}</span>
						</li>
					{/each}
				</ul>

				<div class="flex flex-wrap gap-2">
					{#each exp.technologies as tech}
						<span
							class="rounded-full px-3 py-1.5 font-mono text-[11px] tracking-wide transition-all duration-300"
							style="border: 1px solid #e5e7eb; color: #6b7280;"
						>
							{tech}
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
