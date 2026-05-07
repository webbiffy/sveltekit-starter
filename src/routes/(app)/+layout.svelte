<script lang="ts">
	import { page } from '$app/state';
	import * as Collapsible from '$lib/components/ui/collapsible';
	import * as DropdownMenu from '$lib/components/ui/dropdown-menu';
	import * as Sidebar from '$lib/components/ui/sidebar';
	import {
		BadgeCheck,
		Bell,
		BookOpen,
		Bot,
		ChevronRight,
		ChevronsUpDown,
		CreditCard,
		Frame,
		GalleryVerticalEnd,
		LogOut,
		Map,
		PieChart,
		Plus,
		Settings2,
		Sparkles,
		SquareTerminal
	} from '@lucide/svelte';

	interface Props {
		children: import('svelte').Snippet;
	}

	let { children }: Props = $props();

	const brand = { name: 'Webbiffy Inc', logo: GalleryVerticalEnd, plan: 'Enterprise' };

	const navMain = [
		{
			title: 'Playground',
			href: '/dashboard',
			icon: SquareTerminal,
			isActive: true,
			items: [
				{ title: 'History', href: '/dashboard/history' },
				{ title: 'Starred', href: '/dashboard/starred' },
				{ title: 'Settings', href: '/dashboard/settings' }
			]
		},
		{
			title: 'Models',
			href: '/models',
			icon: Bot,
			items: [
				{ title: 'Genesis', href: '/models/genesis' },
				{ title: 'Explorer', href: '/models/explorer' },
				{ title: 'Quantum', href: '/models/quantum' }
			]
		},
		{
			title: 'Documentation',
			href: '/docs',
			icon: BookOpen,
			items: [
				{ title: 'Introduction', href: '/docs/intro' },
				{ title: 'Get Started', href: '/docs/get-started' },
				{ title: 'Tutorials', href: '/docs/tutorials' },
				{ title: 'Changelog', href: '/docs/changelog' }
			]
		},
		{
			title: 'Settings',
			href: '/settings',
			icon: Settings2,
			items: [
				{ title: 'General', href: '/settings/general' },
				{ title: 'Team', href: '/settings/team' },
				{ title: 'Billing', href: '/settings/billing' },
				{ title: 'Limits', href: '/settings/limits' }
			]
		}
	];

	const navProjects = [
		{ name: 'Design Engineering', href: '/projects/design', icon: Frame },
		{ name: 'Sales & Marketing', href: '/projects/sales', icon: PieChart },
		{ name: 'Travel', href: '/projects/travel', icon: Map }
	];
</script>

<Sidebar.Provider>
	<Sidebar.Root variant="inset">
		<!-- Header: Brand -->
		<Sidebar.Header>
			<div class="flex items-center gap-2 px-2 py-2">
				<div
					class="flex aspect-square size-8 items-center justify-center rounded-lg bg-sidebar-primary text-sidebar-primary-foreground"
				>
					<brand.logo class="size-4" />
				</div>
				<div class="grid flex-1 text-left text-sm leading-tight">
					<span class="truncate font-semibold">{brand.name}</span>
					<span class="truncate text-xs">{brand.plan}</span>
				</div>
			</div>
		</Sidebar.Header>

		<!-- Content: Main nav -->
		<Sidebar.Content>
			<Sidebar.Group>
				<Sidebar.GroupLabel>Platform</Sidebar.GroupLabel>
				<Sidebar.Menu>
					{#each navMain as item}
						{@const isActive = page.url.pathname.startsWith(item.href)}
						<Collapsible.Root open={isActive} class="group/collapsible">
							<Sidebar.MenuItem>
								<Collapsible.Trigger>
									{#snippet child({ props })}
										<Sidebar.MenuButton {isActive} tooltipContent={item.title} {...props}>
											<item.icon />
											<span>{item.title}</span>
											<ChevronRight
												class="ml-auto transition-transform duration-200 group-data-[state=open]/collapsible:rotate-90"
											/>
										</Sidebar.MenuButton>
									{/snippet}
								</Collapsible.Trigger>
								<Collapsible.Content>
									<Sidebar.MenuSub>
										{#each item.items as subItem}
											<Sidebar.MenuSubItem>
												<Sidebar.MenuSubButton>
													{#snippet child({ props })}
														<a href={subItem.href} {...props}>{subItem.title}</a>
													{/snippet}
												</Sidebar.MenuSubButton>
											</Sidebar.MenuSubItem>
										{/each}
									</Sidebar.MenuSub>
								</Collapsible.Content>
							</Sidebar.MenuItem>
						</Collapsible.Root>
					{/each}
				</Sidebar.Menu>
			</Sidebar.Group>

			<!-- Projects -->
			<Sidebar.Group class="group-data-[collapsible=icon]:hidden">
				<Sidebar.GroupLabel>Projects</Sidebar.GroupLabel>
				<Sidebar.Menu>
					{#each navProjects as project}
						<Sidebar.MenuItem>
							<Sidebar.MenuButton>
								{#snippet child({ props })}
									<a href={project.href} {...props}>
										<project.icon />
										<span>{project.name}</span>
									</a>
								{/snippet}
							</Sidebar.MenuButton>
						</Sidebar.MenuItem>
					{/each}
					<Sidebar.MenuItem>
						<Sidebar.MenuButton>
							<Plus />
							<span>More</span>
						</Sidebar.MenuButton>
					</Sidebar.MenuItem>
				</Sidebar.Menu>
			</Sidebar.Group>
		</Sidebar.Content>

		<!-- Footer: User profile with dropdown -->
		<Sidebar.Footer>
			<Sidebar.Menu>
				<Sidebar.MenuItem>
					<DropdownMenu.Root>
						<DropdownMenu.Trigger>
							{#snippet child({ props })}
								<Sidebar.MenuButton size="lg" {...props}>
									<div
										class="flex size-8 items-center justify-center rounded-lg bg-muted text-sm font-semibold"
									>
										CN
									</div>
									<div class="grid flex-1 text-left text-sm leading-tight">
										<span class="truncate font-semibold">shadcn</span>
										<span class="truncate text-xs">m@example.com</span>
									</div>
									<ChevronsUpDown class="ml-auto size-4" />
								</Sidebar.MenuButton>
							{/snippet}
						</DropdownMenu.Trigger>
						<DropdownMenu.Content class="w-56" side="right" align="end" sideOffset={4}>
							<DropdownMenu.Label class="p-0 font-normal">
								<div class="flex items-center gap-2 px-1 py-1.5">
									<div
										class="flex size-8 items-center justify-center rounded-lg bg-muted text-sm font-semibold"
									>
										CN
									</div>
									<div class="grid flex-1 text-left text-sm leading-tight">
										<span class="truncate font-semibold">shadcn</span>
										<span class="truncate text-xs">m@example.com</span>
									</div>
								</div>
							</DropdownMenu.Label>
							<DropdownMenu.Separator />
							<DropdownMenu.Group>
								<DropdownMenu.Item>
									<Sparkles class="mr-2 size-4" />
									Upgrade to Pro
								</DropdownMenu.Item>
							</DropdownMenu.Group>
							<DropdownMenu.Separator />
							<DropdownMenu.Group>
								<DropdownMenu.Item>
									<BadgeCheck class="mr-2 size-4" />
									Account
								</DropdownMenu.Item>
								<DropdownMenu.Item>
									<CreditCard class="mr-2 size-4" />
									Billing
								</DropdownMenu.Item>
								<DropdownMenu.Item>
									<Bell class="mr-2 size-4" />
									Notifications
								</DropdownMenu.Item>
							</DropdownMenu.Group>
							<DropdownMenu.Separator />
							<DropdownMenu.Item>
								<LogOut class="mr-2 size-4" />
								Log out
							</DropdownMenu.Item>
						</DropdownMenu.Content>
					</DropdownMenu.Root>
				</Sidebar.MenuItem>
			</Sidebar.Menu>
		</Sidebar.Footer>

		<Sidebar.Rail />
	</Sidebar.Root>

	<Sidebar.Inset>
		<header
			class="flex h-16 shrink-0 items-center gap-2 transition-[width,height] ease-linear group-has-data-[collapsible=icon]/sidebar-wrapper:h-12"
		>
			<div class="flex items-center gap-2 px-4">
				<Sidebar.Trigger class="-ml-1" />
				<div class="h-4 w-px bg-border"></div>
				<nav class="flex items-center gap-1 text-sm text-muted-foreground">
					<span>Build Your Application</span>
					<ChevronRight class="size-4" />
					<span class="font-medium text-foreground">Data Fetching</span>
				</nav>
			</div>
		</header>
		<main class="flex flex-1 flex-col gap-4 p-4 pt-0">
			{@render children()}
		</main>
	</Sidebar.Inset>
</Sidebar.Provider>
