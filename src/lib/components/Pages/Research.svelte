<script lang="ts">
	interface Publication {
		title: string;
		authors: string;
		venue: string;
		abstract: string;
		link: string;
		tags: string[];
		image?: string;
	}

	interface Book {
		title: string;
		description: string;
		publisher: string;
		year: string;
		link: string;
		isbn: string;
		cover?: string;
	}

	const publications: Publication[] = [
		{
			title: 'Secure Electronic Voting via Blockchain Architectures',
			authors: 'Shishir Poudel',
			venue: 'South Dakota State Capitol (Best Undergraduate Research)',
			abstract:
				"Critically evaluated blockchain architectures for secure electronic voting by benchmarking 10+ consensus protocols against data integrity models. Awarded 'Best Undergraduate Research' for excellence in blockchain analysis.",
			link: '/research_poster.png',
			image: '/research_poster.png',
			tags: ['Blockchain', 'Security', 'Electronic Voting', 'Research Poster']
		}
	];

	const books: Book[] = [
		{
			title: 'PASHUPATINATH: A GUIDE TO SACRED JOURNEY',
			description:
				'Before this there was no book to highlight all dimensions of Pashupatinath, and everything it holds. This guide explores the sacred journey, history, and spiritual significance of the Pashupatinath Temple.',
			publisher: 'Reinstalling Hope',
			year: '2024',
			link: 'https://pashupatinath.reinstallinghope.org/',
			isbn: 'N/A',
			cover: '/pashupatinath.png'
		}
	];
</script>

<div class="space-y-16">
	<!-- Header -->
	<div class="space-y-3">
		<h1 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
			Academic Contributions
		</h1>
		<h2
			class="text-3xl leading-tight font-light tracking-tight md:text-4xl"
			style="color: #111827;"
		>
			Research & Publications
		</h2>
	</div>

	<!-- Books Section -->
	{#if books.length > 0}
		<div class="space-y-8">
			<h3 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
				Published Books
			</h3>
			{#each books as book}
				<article
					class="group relative py-4 pl-8 transition-colors duration-300"
					style="border-left: 1px solid #e5e7eb;"
				>
					<div class="flex flex-col gap-8 md:flex-row">
						<!-- Book Cover -->
						{#if book.cover}
							<div
								class="flex h-32 w-24 shrink-0 items-center justify-center overflow-hidden rounded shadow-sm"
							>
								<img src={book.cover} alt={book.title} class="h-full w-full object-cover" />
							</div>
						{:else}
							<!-- Book Cover Placeholder / Icon -->
							<div
								class="flex h-32 w-24 shrink-0 items-center justify-center rounded"
								style="background: rgba(0, 0, 0, 0.02);"
							>
								<svg
									class="h-8 w-8"
									style="color: #e5e7eb;"
									fill="none"
									stroke="currentColor"
									viewBox="0 0 24 24"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="1"
										d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"
									/>
								</svg>
							</div>
						{/if}

						<div class="flex-1">
							<h4
								class="mb-3 text-xl font-light tracking-tight transition-colors duration-300 md:text-2xl"
								style="color: #111827;"
							>
								{book.title}
							</h4>
							<p class="mb-4 text-[15px] leading-relaxed font-light" style="color: #4b5563;">
								{book.description}
							</p>
							<div
								class="mb-4 flex flex-wrap items-center gap-3 text-[12px]"
								style="color: #9ca3af;"
							>
								<span>{book.publisher}</span>
								<span style="color: #d1d5db;">•</span>
								<span>{book.year}</span>
								<span style="color: #d1d5db;">•</span>
								<span class="font-mono">{book.isbn}</span>
							</div>
							<a
								href={book.link}
								target="_blank"
								rel="noopener noreferrer"
								class="research-link inline-flex items-center gap-2 text-[12px] tracking-[0.15em] uppercase transition-colors duration-300"
							>
								View Book
								<svg
									width="12"
									height="12"
									viewBox="0 0 12 12"
									fill="none"
									stroke="currentColor"
									stroke-width="1"
									class="translate-y-[-1px]"
								>
									<path d="M1 11L11 1M11 1H1M11 1V11" />
								</svg>
							</a>
						</div>
					</div>
				</article>
			{/each}
		</div>
	{/if}

	<!-- Research Papers Section -->
	{#if publications.length > 0}
		<div class="space-y-12">
			<h3 class="text-[11px] font-light tracking-[0.2em] uppercase" style="color: #9ca3af;">
				Peer-Reviewed Publications
			</h3>
			{#each publications as paper, i}
				<article
					class="group relative py-4 pl-8 transition-colors duration-300"
					style="border-left: 1px solid #e5e7eb;"
				>
					<div class="flex flex-col gap-8 md:flex-row">
						{#if paper.image}
							<div class="shrink-0">
								<img
									src={paper.image}
									alt="Research Poster"
									class="h-32 w-24 rounded border border-gray-200 object-cover shadow-sm"
								/>
							</div>
						{/if}

						<div class="flex-1">
							<h4
								class="mb-2 text-xl font-light tracking-tight transition-colors duration-300 md:text-2xl"
								style="color: #111827;"
							>
								{paper.title}
							</h4>
							<div class="mb-1 text-[13px]" style="color: #6b7280;">
								{paper.authors}
							</div>
							<div class="mb-3 text-[12px] italic" style="color: #9ca3af;">
								{paper.venue}
							</div>

							<p
								class="mb-6 text-[15px] leading-relaxed font-light transition-colors duration-300"
								style="color: #4b5563;"
							>
								{paper.abstract}
							</p>

							<div class="mb-4 flex flex-wrap gap-2">
								{#each paper.tags as tag}
									<span
										class="rounded-full px-3 py-1.5 font-mono text-[11px] tracking-wide transition-all duration-300"
										style="border: 1px solid #e5e7eb; color: #6b7280;"
									>
										{tag}
									</span>
								{/each}
							</div>

							<div>
								<a
									href={paper.link}
									target="_blank"
									rel="noopener noreferrer"
									class="research-link inline-flex items-center gap-2 text-[12px] tracking-[0.15em] uppercase transition-colors duration-300"
								>
									{paper.tags.includes('Research Poster') ? 'Read Poster' : 'Read Paper'}
									<svg
										width="12"
										height="12"
										viewBox="0 0 12 12"
										fill="none"
										stroke="currentColor"
										stroke-width="1"
										class="translate-y-[-1px]"
									>
										<path d="M1 11L11 1M11 1H1M11 1V11" />
									</svg>
								</a>
							</div>
						</div>
					</div>
				</article>
			{/each}
		</div>
	{/if}
</div>

<style>
	article {
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}

	/* Link styling */
	.research-link {
		color: #9ca3af;
	}

	/* Hover effects for links */
	.research-link:hover {
		color: #2563eb;
	}

	/* Arrow animation on link hover */
	.research-link:hover svg {
		transform: translate(2px, -2px);
	}
</style>
