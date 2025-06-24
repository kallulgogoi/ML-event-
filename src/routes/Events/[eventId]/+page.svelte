<script context="module" lang="ts">
	export interface LeaderboardEntry {
		team_id?: string;
		team_name?: string;
		participant_id?: string;
		name?: string;
		points: string | number;
	}

	export interface Event {
		event_id: string;
		event_name: string;
		event_description: string;
		event_start_datetime: string;
		event_end_datetime: string;
		venue: string;
		poster_image: string;
		leaderboard: LeaderboardEntry[];
		feedback_form: string;
		post_event_task: string;
	}
</script>

<script lang="ts">
	import { error } from '@sveltejs/kit';
	import { page } from '$app/stores';

	export let data: { events: Event[] };

	const eventId = $page.params.eventId;
	const event = data.events.find((e) => e.event_id === eventId);

	if (!event) throw error(404, 'Event not found');

	const now = new Date();
	const startDate = new Date(event.event_start_datetime);
	const endDate = new Date(event.event_end_datetime);

	const isUpcoming = now < startDate;
	const isOngoing = now >= startDate && now <= endDate;
	const isPast = now > endDate;
</script>

<section class="min-h-screen bg-[#121212] text-gray-100">
	<div class="container mx-auto max-w-7xl px-6 lg:px-8">
		<!-- Fixed Header Section -->
		<div class="lg:sticky lg:top-0 lg:z-50 lg:bg-[#121212] lg:pt-6">
			<!-- Header with back button -->
			<div class="flex flex-col items-center pt-12 lg:pt-0">
				<a
					href="/"
					class="mb-4 flex items-center self-start text-orange-400 transition-colors hover:text-orange-300"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						width="24"
						height="24"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						class="mr-1"
					>
						<line x1="19" y1="12" x2="5" y2="12"></line>
						<polyline points="12 19 5 12 12 5"></polyline>
					</svg>
					Back to Events
				</a>
				<h1
					class="bg-gradient-to-r from-orange-600 to-amber-500 bg-clip-text text-center text-4xl font-bold tracking-tight text-transparent sm:text-5xl lg:text-6xl"
				>
					{event.event_name}
				</h1>
			</div>

			<!-- Mobile Image -->
			<div class="mb-8 mt-5 overflow-hidden rounded-xl shadow-2xl lg:hidden">
				<img
					src={`/${event.poster_image}`}
					alt={event.event_name}
					class="h-80 w-full object-cover"
				/>
			</div>
		</div>

		<div class="relative pb-12 lg:flex">
			<!-- Left Fixed Image -->
			<div
				class="z-10 hidden lg:fixed lg:left-[9%] lg:top-[150px] lg:block lg:h-[calc(100vh-200px)] lg:w-[40%]"
			>
				<div class="mx-6 h-full overflow-hidden rounded-xl shadow-2xl">
					<img
						src={`/${event.poster_image}`}
						alt={event.event_name}
						class="h-full
      w-full object-cover"
					/>
				</div>
			</div>

			<!-- Right Content -->
			<div class="grid w-full gap-12 space-y-8 lg:ml-[50%] lg:items-start lg:pt-6">
				<div class="w-full space-y-8 lg:w-full lg:px-0">
					<!-- Event Description -->
					<div class="rounded-xl border border-gray-700 bg-gray-800/50 p-6 backdrop-blur-sm">
						<h2 class="mb-3 flex items-center text-xl font-semibold text-[#FF7700]">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="20"
								height="20"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="mr-2"
							>
								<path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path>
								<path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path>
							</svg>
							About This Event
						</h2>
						<div class="prose prose-invert max-w-none text-gray-300">
							<p class="text-lg leading-relaxed">{event.event_description}</p>
							{#if event.venue}
								<div class="mt-4 flex items-start text-amber-100 sm:items-center">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										width="18"
										height="18"
										viewBox="0 0 24 24"
										fill="none"
										stroke="currentColor"
										stroke-width="2"
										stroke-linecap="round"
										stroke-linejoin="round"
										class="mr-2 mt-1 flex-shrink-0 sm:mt-0"
									>
										<path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path>
										<circle cx="12" cy="10" r="3"></circle>
									</svg>
									<div class="flex flex-col sm:flex-row sm:items-center">
										<span class="font-medium">Location :</span>
										<span class="ml-1">{event.venue}</span>
									</div>
								</div>
							{/if}
						</div>
					</div>

					<!-- Event Details Card -->
					<div class="rounded-xl border border-gray-700 bg-gray-800/50 p-6 backdrop-blur-sm">
						<h2 class="mb-4 text-xl font-semibold text-[#FF7700]">Event Details</h2>

						<!-- Timeline -->
						<div class="mb-6 flex items-start">
							<div class="flex-shrink-0 rounded-lg bg-orange-400/10 p-3">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									width="24"
									height="24"
									viewBox="0 0 24 24"
									fill="none"
									stroke="currentColor"
									stroke-width="2"
									stroke-linecap="round"
									stroke-linejoin="round"
									class="text-orange-400"
								>
									<rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
									<line x1="16" y1="2" x2="16" y2="6"></line>
									<line x1="8" y1="2" x2="8" y2="6"></line>
									<line x1="3" y1="10" x2="21" y2="10"></line>
								</svg>
							</div>

							<div class="ml-4">
								<h3 class="text-lg font-medium text-gray-100">Timeline</h3>
								<div class="mt-2 space-y-1 text-gray-300">
									<p class="flex items-center">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="16"
											height="16"
											viewBox="0 0 24 24"
											fill="none"
											stroke="currentColor"
											stroke-width="2"
											stroke-linecap="round"
											stroke-linejoin="round"
											class="mr-2 text-amber-400"
										>
											<circle cx="12" cy="12" r="10"></circle>
											<polyline points="12 6 12 12 16 14"></polyline>
										</svg>
										<span class="font-medium">Starts :</span>
										{#if isOngoing}
											<span class="ml-1 text-white"
												>{new Date(event.event_end_datetime).toLocaleString('en-IN')}</span
											>
										{:else if isUpcoming}
											<span class="ml-1 text-white"> Upcoming</span>
										{:else}
											<span class="ml-1"
												>{new Date(event.event_end_datetime).toLocaleString('en-IN')}</span
											>
										{/if}
									</p>
									<p class="flex items-center">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="16"
											height="16"
											viewBox="0 0 24 24"
											fill="none"
											stroke="currentColor"
											stroke-width="2"
											stroke-linecap="round"
											stroke-linejoin="round"
											class="mr-2 text-amber-400"
										>
											<circle cx="12" cy="12" r="10"></circle>
											<polyline points="12 6 12 12 16 14"></polyline>
										</svg>

										<span class="font-medium">Ends :</span>
										{#if isOngoing}
											<span class="ml-1 text-white"> Ongoing</span>
										{:else if isUpcoming}
											<span class="ml-1 text-white"> Upcoming</span>
										{:else}
											<span class="ml-1"
												>{new Date(event.event_end_datetime).toLocaleString('en-IN')}</span
											>
										{/if}
									</p>
									<p class="flex items-center text-orange-400">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="16"
											height="16"
											viewBox="0 0 24 24"
											fill="none"
											stroke="currentColor"
											stroke-width="2"
											stroke-linecap="round"
											stroke-linejoin="round"
											class="mr-2"
										>
											<path d="M22 12h-4l-3 9L9 3l-3 9H2"></path>
										</svg>
										<span class="font-medium">Status :</span>
										{#if isUpcoming}
											<span class="ml-1">Not Started Yet</span>
										{:else if isOngoing}
											<span class="ml-1">Happening Now</span>
										{:else}
											<span class="ml-1">Completed</span>
										{/if}
									</p>
								</div>
							</div>
						</div>

						{#if isOngoing}{/if}

						<!-- Only show these sections for past events -->
						{#if isPast}
							<!-- Leaderboard -->
							{#if event.leaderboard?.length > 0}
								<div class="mb-6 flex items-start">
									<div class="flex-shrink-0 rounded-lg bg-purple-400/10 p-3">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="24"
											height="24"
											viewBox="0 0 24 24"
											fill="none"
											stroke="currentColor"
											stroke-width="2"
											stroke-linecap="round"
											stroke-linejoin="round"
											class="text-purple-400"
										>
											<path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"></path>
											<path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"></path>
											<path d="M4 22h16"></path>
											<path d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"></path>
											<path d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"></path>
											<path d="M18 2H6v7a6 6 0 0 0 12 0V2Z"></path>
										</svg>
									</div>
									<div class="ml-4">
										<h3 class="text-lg font-medium text-gray-100">Leaderboard</h3>
										<div class="mt-2">
											<ol
												class="list-inside list-decimal space-y-2 text-gray-300 marker:text-orange-400"
											>
												{#each event.leaderboard as team}
													<li class="pl-2">
														<span class="font-medium text-amber-200">{team.team_name}</span>
														<span class="ml-2 text-purple-300">{team.points} points</span>
													</li>
												{/each}
											</ol>
										</div>
									</div>
								</div>
							{/if}

							<!-- Task Details -->
							{#if event.post_event_task}
								<div class="mb-6">
									<h2 class="text-lg font-medium text-[#FF7700]">Task Details</h2>
									<p class="leading-relaxed text-amber-100">{event.post_event_task}</p>
								</div>
							{/if}

							<!-- Feedback Form -->
							{#if event.feedback_form}
								<div class="mt-4 flex flex-col gap-4 sm:flex-row">
									<a
										href={event.feedback_form}
										target="_blank"
										rel="noopener"
										class="flex flex-1 items-center justify-center rounded-lg bg-[#FF7700] px-6 py-3 font-medium text-white transition-colors hover:bg-orange-600"
									>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="20"
											height="20"
											viewBox="0 0 24 24"
											fill="none"
											stroke="currentColor"
											stroke-width="2"
											stroke-linecap="round"
											stroke-linejoin="round"
											class="mr-2"
										>
											<path d="M21.5 12a6.5 6.5 0 1 1-13 0 6.5 6.5 0 0 1 13 0Z"></path>
											<path d="M12 16v-4"></path>
											<path d="M12 8h.01"></path>
										</svg>
										Submit Feedback
									</a>
								</div>
							{/if}
						{/if}
					</div>
				</div>
			</div>
		</div>
	</div>
</section>
