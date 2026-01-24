<script>
	const experiences = [
		{
			title: 'AI Security Developer',
			company: 'Cyberclinics, DSU',
			period: 'Aug 2024 - Present',
			bullets: [
				'Developed and deployed modular Python agents, scanning IPs, URLs, and files against VirusTotal and auto-posting results to Microsoft Defender tickets, resulting in a 40% reduction in false positive investigation',
				'Built automated playbooks for email phishing analysis, parsing headers and bodies and validating attachments, standardizing Tier-1 alert response workflow and achieving a 55% reduction in response time for phishing incidents',
				'Fine-tuned small language models (Phi-3) to translate natural language into executable KQL, enabling analysts to query complex log streams and saving approximately 8 hours weekly',
				'Managed student threat detection team on developing custom yara rules, identifying malicious patterns in network traffic, and integrating with SIEM for real-time alerting'
			],
			technologies: ['Python', 'Phi-3', 'KQL', 'VirusTotal', 'Microsoft Defender', 'YARA', 'SIEM']
		},
		{
			title: 'Software Developer',
			company: 'SD Elementary Schools Partnership',
			period: 'Oct 2025 - Apr 2026',
			bullets: [
				'Led a 6-person Agile team to deploy a web app that automates scoring for literacy assessments, replacing static spreadsheets with a system that tracks historical progress and groups students for immediate academic support',
				'Structured a data model using Django ORM to track student history, automatically grouping students by skill level to help teachers formulate targeted intervention strategies daily',
				'Coded React forms that auto-generate based on database configuration, allowing teachers to create new assessment formats without requiring developer intervention'
			],
			technologies: ['Django', 'React', 'PostgreSQL', 'Agile']
		},
		{
			title: 'Computer Science Researcher',
			company: 'Dakota State University',
			period: 'Aug 2023 - Feb 2024',
			bullets: [
				'Programmed a simulation environment to benchmark 10+ consensus protocols, rigorously quantifying transaction throughput and network fault tolerance to evaluate viability for high-stakes electronic voting'
			],
			technologies: ['Blockchain', 'Consensus Protocols', 'Python']
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
