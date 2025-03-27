<script>
	import { onMount } from 'svelte';
	import { fade, fly, scale } from 'svelte/transition';
	import { spring } from 'svelte/motion';
	import AnimatedBackgrounHero from '$lib/components/AnimatedBackgrounHero.svelte';

	// Estado para controlar elementos visíveis
	let isVisible = false;
	let activeTab = 'hiperautomacao';
	let showMobileMenu = false;

	// Efeito de scroll para revelar elementos
	let sections = [];
	let windowHeight;
	let isScrolled = false;
	let scrollOpacity = 0;
	let mobileMenuOpen = false;

	// Efeito para o contador de números
	let stats = [
		{ value: 0, target: 95, suffix: '%', label: 'Redução de tarefas manuais' },
		{ value: 0, target: 3, suffix: 'x', label: 'Mais rápido' },
		{ value: 0, target: 85, suffix: '%', label: 'Precisão em extração de dados' }
	];

	// Serviços com n8n
	const servicos = [
		{
			id: 'hiperautomacao',
			titulo: 'Hiperautomação',
			descricao:
				'Conectamos múltiplos sistemas e processos para criar fluxos de trabalho inteligentes que se adaptam automaticamente.',
			icone: 'rocket'
		},
		{
			id: 'extracao',
			titulo: 'Extração Inteligente de Dados',
			descricao:
				'Automatize a captura de informações de diversos formatos de documentos com precisão e velocidade superiores.',
			icone: 'file-text'
		},
		{
			id: 'sentimentos',
			titulo: 'Análise de Sentimentos',
			descricao:
				'Entenda o que seus clientes realmente sentem através de análise automatizada em tempo real.',
			icone: 'heart-pulse'
		},
		{
			id: 'etl',
			titulo: 'Integração e ETL',
			descricao:
				'Soluções sob medida para extração, transformação e carregamento de dados entre sistemas diversos.',
			icone: 'database'
		},
		{
			id: 'decisoes',
			titulo: 'Automação de Decisões',
			descricao:
				'Sistemas que tomam decisões operacionais automaticamente com base em regras de negócio pré-definidas.',
			icone: 'cpu'
		},
		{
			id: 'monitoramento',
			titulo: 'Monitoramento Proativo',
			descricao:
				'Alertas automatizados e dashboards em tempo real para métricas críticas do seu negócio.',
			icone: 'activity'
		}
	];

	onMount(() => {
		// Iniciar animações quando a página carrega
		setTimeout(() => {
			isVisible = true;
			animateCounters();
		}, 300);
		const handleScroll = () => {
			// Calculate opacity based on scroll position (0 to 1)
			// Full opacity at 200px scroll, starts fading in at 10px
			const scrollPos = window.scrollY;
			scrollOpacity = Math.min(scrollPos / 200, 1);
		};

		// Initial check
		handleScroll();

		// Add scroll event listener - THIS WAS MISSING
		window.addEventListener('scroll', handleScroll);

		// Observer para elementos que aparecem no scroll
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						entry.target.classList.add('animate-in');
						observer.unobserve(entry.target);
					}
				});
			},
			{ threshold: 0.1 }
		);

		// Observar todas as seções
		document.querySelectorAll('.observe-section').forEach((section) => {
			observer.observe(section);
		});

		return () => {
			if (observer) {
				observer.disconnect();
			}
		};
	});

	const toggleMobileMenu = () => {
		mobileMenuOpen = !mobileMenuOpen;
	};

	const navItems = [
		{ href: '#inicio', text: 'Início' },
		{ href: '#servicos', text: 'Serviços' },
		{ href: '#sobre', text: 'Sobre Nós' },
		{ href: '#contato', text: 'Contato' }
	];

	function animateCounters() {
		const duration = 2000; // 2 segundos para animação
		const steps = 60; // Frames por segundo * duração em segundos
		const interval = duration / steps;

		let step = 0;

		const timer = setInterval(() => {
			step++;
			const progress = step / steps;

			stats = stats.map((stat) => {
				// Usar uma função de easing para desacelerar no final
				const easedProgress = 1 - Math.pow(1 - progress, 3);
				return {
					...stat,
					value: Math.round(stat.target * easedProgress)
				};
			});

			if (step >= steps) {
				clearInterval(timer);
			}
		}, interval);
	}
</script>

<svelte:head>
	<title>Autono | Potencializando seu negócio com Hiperautomação e IA</title>
	<link
		href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
		rel="stylesheet"
	/>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div class="min-h-screen bg-slate-900 text-slate-100 font-inter relative overflow-hidden">
	<!-- Elementos de fundo animados -->
	<div class="fixed top-0 left-0 w-full h-full overflow-hidden -z-10">
		<div
			class="absolute top-[10%] -left-[10%] w-[50%] h-[40%] bg-purple-900/20 rounded-full blur-[100px]"
		></div>
		<div
			class="absolute top-[60%] right-[5%] w-[40%] h-[50%] bg-blue-600/10 rounded-full blur-[100px]"
		></div>
		<div
			class="absolute top-[20%] right-[10%] w-[30%] h-[30%] bg-emerald-500/10 rounded-full blur-[100px]"
		></div>
	</div>

	<!-- Navbar -->
	<nav
		class="fixed top-0 left-0 right-0 z-50 transition-all duration-500"
		style="background-color: rgba(0,0,0,{scrollOpacity * 0.6}); 
		   backdrop-filter: blur({scrollOpacity * 12}px); 
		   box-shadow: 0 4px 30px rgba(0, 0, 0, {scrollOpacity * 0.1})"
	>
		<div class="max-w-6xl mx-auto px-4 sm:px-6">
			<div class="flex items-center justify-between h-20">
				<!-- Logo -->
				<div class="relative">
					<a
						href="/"
						class="text-2xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-purple-500 to-blue-500"
					>
						Autono
					</a>
					<div
						class="absolute -bottom-1 left-0 h-[2px] w-1/2 bg-gradient-to-r from-purple-500 to-blue-400"
					></div>
				</div>

				<!-- Desktop Navigation -->
				<div class="hidden md:flex items-center">
					<!-- Navigation Items -->
					<div class="ml-10 flex items-baseline space-x-8">
						{#each navItems as item}
							<a
								href={item.href}
								class="px-2 py-1 text-sm font-light tracking-wide text-white/70 hover:text-white border-b-2 border-transparent hover:border-purple-500/50 transition-all duration-300"
							>
								{item.text}
							</a>
						{/each}
					</div>

					<!-- Contact Button -->
					<div class="ml-8 pl-8 border-l border-white/10">
						<a
							href="#contato"
							class="group px-5 py-2 bg-gradient-to-r from-purple-600/10 to-blue-500/10 hover:from-purple-600/20 hover:to-blue-500/20 border border-purple-500/20 backdrop-blur-sm rounded-sm text-sm transition-all duration-300 flex items-center gap-2"
						>
							<span>Fale Conosco</span>
							<svg
								class="w-4 h-4 transform group-hover:translate-x-1 transition-transform"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								/>
							</svg>
						</a>
					</div>
				</div>

				<!-- Mobile menu button -->
				<div class="md:hidden flex items-center">
					<button class="p-2 text-white/70 hover:text-white" on:click={toggleMobileMenu}>
						<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M4 6h16M4 12h16m-7 6h7"
							/>
						</svg>
					</button>
				</div>
			</div>
		</div>

		<!-- Mobile menu -->
		{#if mobileMenuOpen}
			<div class="md:hidden bg-black/80 backdrop-blur-lg animate-fadeIn">
				<div class="px-4 pt-2 pb-4 space-y-1 border-t border-white/10">
					{#each navItems as item}
						<a
							href={item.href}
							class="block px-3 py-2 text-base font-light text-white/70 hover:text-white hover:bg-white/[0.05] rounded-sm transition-all duration-300"
							on:click={toggleMobileMenu}
						>
							{item.text}
						</a>
					{/each}
					<a
						href="#contato"
						class="mt-4 block w-full px-4 py-3 bg-gradient-to-r from-purple-600/20 to-blue-500/20 border border-purple-500/20 text-center rounded-sm transition-all duration-300"
						on:click={toggleMobileMenu}
					>
						Fale Conosco
					</a>
				</div>
			</div>
		{/if}
	</nav>

	<!-- Hero Section -->
	<AnimatedBackgrounHero {isVisible} />

	<!-- Stats Section -->
	<section
		class="observe-section py-16 border-y border-slate-800 backdrop-blur-sm bg-slate-900/50 relative"
	>
		<div class="container mx-auto px-4">
			<div class="grid grid-cols-1 md:grid-cols-3 gap-8">
				{#each stats as stat}
					<div
						class="backdrop-blur-sm bg-slate-800/50 border border-slate-700/50 rounded-sm p-8 shadow-lg hover:shadow-purple-900/20 transition-all duration-300 hover:translate-y-[-5px]"
					>
						<div class="flex items-end space-x-2 mb-4">
							<span
								class="text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
								>{stat.value}</span
							>
							<span class="text-3xl text-slate-300 font-semibold pb-1">{stat.suffix}</span>
						</div>
						<p class="text-slate-300 font-medium">{stat.label}</p>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<!-- Serviços Section -->
	<!-- Serviços Section -->
	<!-- Serviços Section -->
	<section id="servicos" class="observe-section py-24 relative">
		<div class="container mx-auto px-4">
			<div class="text-center mb-16">
				<h2
					class="text-3xl md:text-4xl font-bold mb-4 inline-block text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
				>
					Nossas Soluções de Automação
				</h2>
				<div class="w-20 h-1 bg-gradient-to-r from-purple-500 to-blue-500 mx-auto mb-6"></div>
				<p class="text-slate-300 max-w-2xl mx-auto">
					Desenvolvemos automações personalizadas com n8n para eliminar tarefas manuais e aumentar a
					produtividade do seu negócio
				</p>
			</div>

			<!-- Integração entre Sistemas - Left image, right content -->
			<div class="mb-24">
				<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
					<!-- Image on left -->
					<div class="relative group overflow-hidden rounded-sm">
						<div
							class="absolute inset-0 bg-gradient-to-br from-purple-600/20 to-blue-600/20 group-hover:from-purple-600/30 group-hover:to-blue-600/30 transition-all duration-500"
						></div>
						<div
							class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 h-72 lg:h-96 rounded-sm shadow-lg flex items-center justify-center relative"
						>
							<div
								class="w-24 h-24 bg-gradient-to-br from-purple-500 to-blue-600 rounded-sm flex items-center justify-center shadow-lg shadow-purple-500/20 transform transition-all duration-500 group-hover:scale-110"
							>
								<i class="fas fa-link text-white text-4xl"></i>
							</div>
						</div>
					</div>

					<!-- Content on right -->
					<div
						class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 rounded-sm p-8 shadow-lg"
					>
						<h3
							class="text-2xl md:text-3xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
						>
							Integração entre Sistemas
						</h3>
						<p class="text-lg text-slate-300 mb-6">
							Conectamos suas plataformas e aplicativos para que dados fluam automaticamente entre
							eles, eliminando retrabalho e inconsistências.
						</p>

						<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Sincronização entre CRM e ERP</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Integrações com APIs externas</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Conexão com ferramentas SaaS populares</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Transferência automática de dados</span>
							</div>
						</div>

						<a
							href="#contato"
							class="inline-flex items-center px-8 py-3 bg-gradient-to-r from-purple-600 to-blue-500 hover:from-purple-700 hover:to-blue-600 text-white font-medium rounded-sm shadow-lg shadow-purple-500/20 hover:shadow-purple-500/40 transition-all duration-300"
						>
							Solicitar demonstração
							<svg
								class="w-5 h-5 ml-2"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								xmlns="http://www.w3.org/2000/svg"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								></path>
							</svg>
						</a>
					</div>
				</div>
			</div>

			<!-- Automação de Marketing - Right image, left content -->
			<div class="mb-24">
				<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
					<!-- Content on left -->
					<div
						class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 rounded-sm p-8 shadow-lg"
					>
						<h3
							class="text-2xl md:text-3xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
						>
							Automação de Marketing e Vendas
						</h3>
						<p class="text-lg text-slate-300 mb-6">
							Automatizamos seus fluxos de marketing e vendas para capturar, nutrir e converter
							leads com maior eficiência e menor esforço manual.
						</p>

						<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Captura e qualificação automática de leads</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Campanhas de email personalizadas</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Alertas de oportunidades para equipe</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Acompanhamento automatizado de conversões</span>
							</div>
						</div>

						<a
							href="#contato"
							class="inline-flex items-center px-8 py-3 bg-gradient-to-r from-purple-600 to-blue-500 hover:from-purple-700 hover:to-blue-600 text-white font-medium rounded-sm shadow-lg shadow-purple-500/20 hover:shadow-purple-500/40 transition-all duration-300"
						>
							Solicitar demonstração
							<svg
								class="w-5 h-5 ml-2"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								xmlns="http://www.w3.org/2000/svg"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								></path>
							</svg>
						</a>
					</div>

					<!-- Image on right -->
					<div class="relative group overflow-hidden rounded-sm">
						<div
							class="absolute inset-0 bg-gradient-to-br from-purple-600/20 to-blue-600/20 group-hover:from-purple-600/30 group-hover:to-blue-600/30 transition-all duration-500"
						></div>
						<div
							class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 h-72 lg:h-96 rounded-sm shadow-lg flex items-center justify-center relative"
						>
							<div
								class="w-24 h-24 bg-gradient-to-br from-purple-500 to-blue-600 rounded-sm flex items-center justify-center shadow-lg shadow-purple-500/20 transform transition-all duration-500 group-hover:scale-110"
							>
								<i class="fas fa-bullhorn text-white text-4xl"></i>
							</div>
						</div>
					</div>
				</div>
			</div>

			<!-- Automação de Processos - Left image, right content -->
			<div class="mb-24">
				<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
					<!-- Image on left -->
					<div class="relative group overflow-hidden rounded-sm">
						<div
							class="absolute inset-0 bg-gradient-to-br from-purple-600/20 to-blue-600/20 group-hover:from-purple-600/30 group-hover:to-blue-600/30 transition-all duration-500"
						></div>
						<div
							class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 h-72 lg:h-96 rounded-sm shadow-lg flex items-center justify-center relative"
						>
							<div
								class="w-24 h-24 bg-gradient-to-br from-purple-500 to-blue-600 rounded-sm flex items-center justify-center shadow-lg shadow-purple-500/20 transform transition-all duration-500 group-hover:scale-110"
							>
								<i class="fas fa-cogs text-white text-4xl"></i>
							</div>
						</div>
					</div>

					<!-- Content on right -->
					<div
						class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 rounded-sm p-8 shadow-lg"
					>
						<h3
							class="text-2xl md:text-3xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
						>
							Automação de Processos Operacionais
						</h3>
						<p class="text-lg text-slate-300 mb-6">
							Eliminamos tarefas repetitivas do dia a dia da sua operação, liberando sua equipe para
							focar em atividades estratégicas.
						</p>

						<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Geração automática de relatórios</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Processamento e verificação de documentos</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Fluxos de aprovação automatizados</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Notificações e alertas inteligentes</span>
							</div>
						</div>

						<a
							href="#contato"
							class="inline-flex items-center px-8 py-3 bg-gradient-to-r from-purple-600 to-blue-500 hover:from-purple-700 hover:to-blue-600 text-white font-medium rounded-sm shadow-lg shadow-purple-500/20 hover:shadow-purple-500/40 transition-all duration-300"
						>
							Solicitar demonstração
							<svg
								class="w-5 h-5 ml-2"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								xmlns="http://www.w3.org/2000/svg"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								></path>
							</svg>
						</a>
					</div>
				</div>
			</div>

			<!-- Chatbots Inteligentes - Right image, left content -->
			<div class="mb-24">
				<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
					<!-- Content on left -->
					<div
						class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 rounded-sm p-8 shadow-lg"
					>
						<h3
							class="text-2xl md:text-3xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
						>
							Criação de Chatbots Inteligentes
						</h3>
						<p class="text-lg text-slate-300 mb-6">
							Desenvolvemos chatbots personalizados que automatizam o atendimento ao cliente,
							qualificam leads e aumentam as conversões em seus canais digitais.
						</p>

						<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Integração com WhatsApp, site e redes sociais</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Respostas automáticas personalizadas</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Encaminhamento inteligente para humanos</span>
							</div>
							<div class="flex items-start space-x-3">
								<div
									class="shrink-0 w-6 h-6 mt-1 rounded-sm bg-purple-500/20 flex items-center justify-center"
								>
									<i class="fas fa-check text-purple-500 text-sm"></i>
								</div>
								<span class="text-slate-300">Qualificação de leads e agendamento automático</span>
							</div>
						</div>

						<a
							href="#contato"
							class="inline-flex items-center px-8 py-3 bg-gradient-to-r from-purple-600 to-blue-500 hover:from-purple-700 hover:to-blue-600 text-white font-medium rounded-sm shadow-lg shadow-purple-500/20 hover:shadow-purple-500/40 transition-all duration-300"
						>
							Solicitar demonstração
							<svg
								class="w-5 h-5 ml-2"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								xmlns="http://www.w3.org/2000/svg"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								></path>
							</svg>
						</a>
					</div>

					<!-- Image on right -->
					<div class="relative group overflow-hidden rounded-sm">
						<div
							class="absolute inset-0 bg-gradient-to-br from-purple-600/20 to-blue-600/20 group-hover:from-purple-600/30 group-hover:to-blue-600/30 transition-all duration-500"
						></div>
						<div
							class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 h-72 lg:h-96 rounded-sm shadow-lg flex items-center justify-center relative"
						>
							<div
								class="w-24 h-24 bg-gradient-to-br from-purple-500 to-blue-600 rounded-sm flex items-center justify-center shadow-lg shadow-purple-500/20 transform transition-all duration-500 group-hover:scale-110"
							>
								<i class="fas fa-comments text-white text-4xl"></i>
							</div>
						</div>
					</div>
				</div>
			</div>

			<!-- Final CTA Section -->
			<div class="mt-20">
				<div
					class="backdrop-blur-sm bg-slate-800/40 border border-slate-700/50 rounded-sm p-10 shadow-lg relative overflow-hidden"
				>
					<!-- Decorative elements -->
					<div
						class="absolute -top-20 -right-20 w-64 h-64 bg-gradient-to-br from-purple-600/10 to-blue-600/10 rounded-full blur-3xl"
					></div>
					<div
						class="absolute -bottom-20 -left-20 w-64 h-64 bg-gradient-to-br from-blue-600/10 to-purple-600/10 rounded-full blur-3xl"
					></div>

					<div class="relative z-10 text-center">
						<h3
							class="text-2xl md:text-3xl font-bold mb-4 text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
						>
							Pronto para automatizar seus processos?
						</h3>
						<p class="text-lg text-slate-300 mb-8 max-w-3xl mx-auto">
							Nossa equipe de especialistas em n8n está pronta para desenvolver soluções de
							automação personalizadas que reduzam custos e aumentem a eficiência do seu negócio.
						</p>

						<a
							href="#contato"
							class="inline-flex items-center px-8 py-3 bg-gradient-to-r from-purple-600 to-blue-500 hover:from-purple-700 hover:to-blue-600 text-white font-medium rounded-sm shadow-lg shadow-purple-500/20 hover:shadow-purple-500/40 transition-all duration-300 group"
						>
							Fale com um especialista
							<svg
								class="w-5 h-5 ml-2 transform transition-transform duration-300 group-hover:translate-x-1"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
								xmlns="http://www.w3.org/2000/svg"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M14 5l7 7m0 0l-7 7m7-7H3"
								></path>
							</svg>
						</a>
					</div>
				</div>
			</div>
		</div>
	</section>
	<!-- Sobre Section -->
	<section
		id="sobre"
		class="observe-section py-24 relative backdrop-blur-md bg-slate-900/50 border-y border-slate-800"
	>
		<div class="container mx-auto px-4">
			<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
				<div class="relative h-96">
					<!-- Design glassmórfico para N8N -->
					<div
						class="absolute top-0 left-0 w-64 h-64 bg-gradient-to-br from-purple-700/30 to-purple-900/30 backdrop-blur-md rounded-sm border border-purple-700/30 shadow-lg transform rotate-6"
					></div>
					<div
						class="absolute top-12 left-12 w-64 h-64 bg-gradient-to-br from-blue-700/30 to-blue-900/30 backdrop-blur-md rounded-sm border border-blue-700/30 shadow-lg transform -rotate-3"
					></div>

					<div
						class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-24 h-24 bg-white rounded-sm shadow-xl flex items-center justify-center z-10"
					>
						<img src="https://n8n.io/favicon.ico" alt="n8n" class="w-16 h-16" />
					</div>

					<!-- Elementos de conexão -->
					<div
						class="absolute top-[25%] left-[55%] w-[20%] h-[2px] bg-purple-500/40 transform rotate-45"
					></div>
					<div
						class="absolute top-[65%] left-[55%] w-[20%] h-[2px] bg-blue-500/40 transform -rotate-45"
					></div>
				</div>

				<div>
					<h2
						class="text-3xl md:text-4xl font-bold mb-6 text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
					>
						Sobre a Autono
					</h2>

					<p class="text-lg text-slate-300 mb-6">
						Somos uma empresa especialista em automação e hiperautomação que transforma a maneira
						como as empresas operam seus processos.
					</p>
					<p class="text-lg text-slate-300 mb-8">
						Utilizamos o poderoso n8n como nossa principal ferramenta de automação, permitindo criar
						soluções personalizadas que se integram com praticamente qualquer sistema ou plataforma.
					</p>

					<div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
						<div
							class="backdrop-blur-sm bg-slate-800/50 border border-slate-700/50 p-6 rounded-sm hover:shadow-lg hover:shadow-purple-900/10 transition-all duration-300 hover:-translate-y-1"
						>
							<div
								class="w-12 h-12 bg-purple-500/20 rounded-sm flex items-center justify-center mb-4"
							>
								<i class="fas fa-bolt text-purple-500 text-xl"></i>
							</div>
							<h3 class="text-xl font-semibold mb-2">Eficiência</h3>
							<p class="text-slate-300">Aceleramos processos e eliminamos gargalos</p>
						</div>

						<div
							class="backdrop-blur-sm bg-slate-800/50 border border-slate-700/50 p-6 rounded-sm hover:shadow-lg hover:shadow-purple-900/10 transition-all duration-300 hover:-translate-y-1"
						>
							<div
								class="w-12 h-12 bg-blue-500/20 rounded-sm flex items-center justify-center mb-4"
							>
								<i class="fas fa-lightbulb text-blue-500 text-xl"></i>
							</div>
							<h3 class="text-xl font-semibold mb-2">Inovação</h3>
							<p class="text-slate-300">Pensamos além do convencional</p>
						</div>

						<div
							class="backdrop-blur-sm bg-slate-800/50 border border-slate-700/50 p-6 rounded-sm hover:shadow-lg hover:shadow-purple-900/10 transition-all duration-300 hover:-translate-y-1"
						>
							<div
								class="w-12 h-12 bg-emerald-500/20 rounded-sm flex items-center justify-center mb-4"
							>
								<i class="fas fa-users text-emerald-500 text-xl"></i>
							</div>
							<h3 class="text-xl font-semibold mb-2">Colaboração</h3>
							<p class="text-slate-300">Trabalhamos juntos pelo seu sucesso</p>
						</div>

						<div
							class="backdrop-blur-sm bg-slate-800/50 border border-slate-700/50 p-6 rounded-sm hover:shadow-lg hover:shadow-purple-900/10 transition-all duration-300 hover:-translate-y-1"
						>
							<div
								class="w-12 h-12 bg-purple-500/20 rounded-sm flex items-center justify-center mb-4"
							>
								<i class="fas fa-shield-alt text-purple-500 text-xl"></i>
							</div>
							<h3 class="text-xl font-semibold mb-2">Confiança</h3>
							<p class="text-slate-300">Segurança em cada etapa do processo</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- CTA Section -->
	<section class="observe-section py-24 relative">
		<div class="container mx-auto px-4">
			<div
				class="backdrop-blur-md bg-gradient-to-r from-purple-900/50 to-blue-900/50 rounded-sm border border-purple-800/50 p-16 relative overflow-hidden shadow-xl shadow-purple-900/20"
			>
				<!-- Elementos decorativos -->
				<div class="absolute top-0 right-0 w-64 h-64 bg-purple-500/10 rounded-full blur-3xl"></div>
				<div class="absolute bottom-0 left-0 w-64 h-64 bg-blue-500/10 rounded-full blur-3xl"></div>

				<div class="relative z-10 text-center max-w-3xl mx-auto">
					<h2 class="text-3xl md:text-4xl font-bold mb-6 text-white">Pronto para automatizar?</h2>
					<p class="text-xl text-slate-300 mb-10">
						Transforme sua empresa hoje com soluções de automação personalizadas que funcionam.
					</p>
					<a
						href="#contato"
						class="inline-block bg-white hover:bg-slate-100 text-purple-600 font-medium py-4 px-10 rounded-sm shadow-xl hover:shadow-white/20 transition-all duration-300 transform hover:-translate-y-1"
					>
						Agende uma demonstração
					</a>
				</div>
			</div>
		</div>
	</section>

	<!-- Contact Section -->
	<section id="contato-form" class="observe-section py-24 relative">
		<div class="container mx-auto px-4">
		  <!-- Section title -->
		  <div class="text-center mb-16">
			<h2 class="text-3xl md:text-4xl font-bold mb-4 inline-block text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500">
			  Entre em Contato
			</h2>
			<div class="w-20 h-1 bg-gradient-to-r from-purple-500 to-blue-500 mx-auto mb-6"></div>
			<p class="text-slate-300 max-w-xl mx-auto">
			  Estamos prontos para ajudar a transformar sua empresa
			</p>
		  </div>
	  
		  <!-- Contact info cards - visible on desktop, condensed on mobile -->
		  <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
			<!-- Location Card -->
			<div class="backdrop-blur-sm bg-slate-900/90 border border-slate-800 p-6 rounded-md hover:border-purple-500/30 transition-all duration-300">
			  <div class="flex flex-col md:flex-row md:items-start space-y-4 md:space-y-0 md:space-x-4">
				<div class="mx-auto md:mx-0 shrink-0 w-12 h-12 bg-purple-900/30 rounded-md flex items-center justify-center">
				  <i class="fas fa-map-marker-alt text-purple-500"></i>
				</div>
				<div class="text-center md:text-left">
				  <h3 class="text-lg font-semibold mb-2 text-white">Localização</h3>
				  <p class="text-slate-300">São Paulo, SP - Brasil</p>
				</div>
			  </div>
			</div>
	  
			<!-- Email Card -->
			<div class="backdrop-blur-sm bg-slate-900/90 border border-slate-800 p-6 rounded-md hover:border-blue-500/30 transition-all duration-300">
			  <div class="flex flex-col md:flex-row md:items-start space-y-4 md:space-y-0 md:space-x-4">
				<div class="mx-auto md:mx-0 shrink-0 w-12 h-12 bg-blue-900/30 rounded-md flex items-center justify-center">
				  <i class="fas fa-envelope text-blue-500"></i>
				</div>
				<div class="text-center md:text-left">
				  <h3 class="text-lg font-semibold mb-2 text-white">Email</h3>
				  <p class="text-slate-300">contato@autono.com.br</p>
				</div>
			  </div>
			</div>
	  
			<!-- Phone Card -->
			<div class="backdrop-blur-sm bg-slate-900/90 border border-slate-800 p-6 rounded-md hover:border-emerald-500/30 transition-all duration-300">
			  <div class="flex flex-col md:flex-row md:items-start space-y-4 md:space-y-0 md:space-x-4">
				<div class="mx-auto md:mx-0 shrink-0 w-12 h-12 bg-emerald-900/30 rounded-md flex items-center justify-center">
				  <i class="fas fa-phone-alt text-emerald-500"></i>
				</div>
				<div class="text-center md:text-left">
				  <h3 class="text-lg font-semibold mb-2 text-white">Telefone</h3>
				  <p class="text-slate-300">+55 (11) 9999-8888</p>
				</div>
			  </div>
			</div>
		  </div>
	  
		  <!-- Social media links - centered on mobile -->
		  <div class="flex justify-center space-x-4 mb-16">
			<a href="#" class="w-12 h-12 bg-slate-900/80 hover:bg-slate-800 border border-slate-800 rounded-md flex items-center justify-center text-purple-500 hover:text-white hover:border-purple-500/50 transition-all duration-300">
			  <i class="fab fa-linkedin-in"></i>
			</a>
			<a href="#" class="w-12 h-12 bg-slate-900/80 hover:bg-slate-800 border border-slate-800 rounded-md flex items-center justify-center text-blue-500 hover:text-white hover:border-blue-500/50 transition-all duration-300">
			  <i class="fab fa-twitter"></i>
			</a>
			<a href="#" class="w-12 h-12 bg-slate-900/80 hover:bg-slate-800 border border-slate-800 rounded-md flex items-center justify-center text-purple-500 hover:text-white hover:border-purple-500/50 transition-all duration-300">
			  <i class="fab fa-instagram"></i>
			</a>
			<a href="#" class="w-12 h-12 bg-slate-900/80 hover:bg-slate-800 border border-slate-800 rounded-md flex items-center justify-center text-slate-300 hover:text-white hover:border-slate-300/50 transition-all duration-300">
			  <i class="fab fa-github"></i>
			</a>
		  </div>
	  
		  <!-- Contact Form Section - This part can be directly linked -->
		  <div id="contato" class="max-w-xl mx-auto">
			<div class="backdrop-blur-sm bg-slate-900/80 border border-slate-800 rounded-md p-8 shadow-md">
			  <form>
				<div class="space-y-6">
				  <!-- Name field -->
				  <div>
					<label for="name" class="block text-slate-300 mb-2 font-medium">Nome</label>
					<input
					  type="text"
					  id="name"
					  class="w-full bg-slate-900/90 border border-slate-700 rounded-md px-4 py-3 text-white focus:outline-none focus:border-purple-500 focus:ring-1 focus:ring-purple-500 transition-all duration-300"
					  placeholder="Seu nome completo"
					/>
				  </div>
	  
				  <!-- Email field -->
				  <div>
					<label for="email" class="block text-slate-300 mb-2 font-medium">Email</label>
					<input
					  type="email"
					  id="email"
					  class="w-full bg-slate-900/90 border border-slate-700 rounded-md px-4 py-3 text-white focus:outline-none focus:border-purple-500 focus:ring-1 focus:ring-purple-500 transition-all duration-300"
					  placeholder="Seu melhor email"
					/>
				  </div>
	  
				  <!-- Company field -->
				  <div>
					<label for="company" class="block text-slate-300 mb-2 font-medium">Empresa</label>
					<input
					  type="text"
					  id="company"
					  class="w-full bg-slate-900/90 border border-slate-700 rounded-md px-4 py-3 text-white focus:outline-none focus:border-purple-500 focus:ring-1 focus:ring-purple-500 transition-all duration-300"
					  placeholder="Nome da sua empresa"
					/>
				  </div>
	  
				  <!-- Message field -->
				  <div>
					<label for="message" class="block text-slate-300 mb-2 font-medium">Mensagem</label>
					<textarea
					  id="message"
					  rows="5"
					  class="w-full bg-slate-900/90 border border-slate-700 rounded-md px-4 py-3 text-white focus:outline-none focus:border-purple-500 focus:ring-1 focus:ring-purple-500 transition-all duration-300"
					  placeholder="Como podemos ajudar?"
					></textarea>
				  </div>
	  
				  <!-- Submit button -->
				  <button
					type="submit"
					class="w-full bg-gradient-to-r from-purple-600 to-blue-600 hover:from-purple-700 hover:to-blue-700 text-white font-medium py-3 px-6 rounded-md shadow-lg hover:shadow-purple-900/20 transition-all duration-300 flex items-center justify-center space-x-2"
				  >
					<i class="fas fa-paper-plane"></i>
					<span>Enviar mensagem</span>
				  </button>
				</div>
			  </form>
			</div>
		  </div>
		</div>
	  </section>
	  


	<!-- Footer -->
	<footer class="bg-slate-900 border-t border-slate-800 pt-16 pb-8">
		<div class="container mx-auto px-4">
			<div class="grid grid-cols-1 md:grid-cols-4 gap-12 mb-12">
				<div>
					<div class="relative">
						<a
							href="/"
							class="text-2xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-purple-500 to-blue-500"
						>
							Autono
						</a>
						<div
							class="absolute -bottom-1 left-0 h-[2px] w-1/2 bg-gradient-to-r from-purple-500 to-blue-400"
						></div>
					</div>
					<p class="text-slate-400 mb-6 pt-4">Potencializando seu negócio com automação inteligente</p>
				</div>

				<div>
					<h3 class="text-lg font-semibold text-white mb-6">Serviços</h3>
					<ul class="space-y-3">
						<li>
							<a
								href="#servicos"
								class="text-slate-400 hover:text-white transition-all duration-300"
								>Hiperautomação</a
							>
						</li>
						<li>
							<a
								href="#servicos"
								class="text-slate-400 hover:text-white transition-all duration-300"
								>Extração de Dados</a
							>
						</li>
						<li>
							<a
								href="#servicos"
								class="text-slate-400 hover:text-white transition-all duration-300"
								>Análise de Sentimentos</a
							>
						</li>
						<li>
							<a
								href="#servicos"
								class="text-slate-400 hover:text-white transition-all duration-300"
								>Integração e ETL</a
							>
						</li>
					</ul>
				</div>

				<div>
					<h3 class="text-lg font-semibold text-white mb-6">Empresa</h3>
					<ul class="space-y-3">
						<li>
							<a href="#sobre" class="text-slate-400 hover:text-white transition-all duration-300"
								>Sobre nós</a
							>
						</li>
						<li>
							<a href="#contato" class="text-slate-400 hover:text-white transition-all duration-300"
								>Contato</a
							>
						</li>
						<li>
							<a href="#" class="text-slate-400 hover:text-white transition-all duration-300"
								>Blog</a
							>
						</li>
						<li>
							<a href="#" class="text-slate-400 hover:text-white transition-all duration-300"
								>Carreiras</a
							>
						</li>
					</ul>
				</div>

				<div>
					<h3 class="text-lg font-semibold text-white mb-6">Legal</h3>
					<ul class="space-y-3">
						<li>
							<a href="#" class="text-slate-400 hover:text-white transition-all duration-300"
								>Termos de Uso</a
							>
						</li>
						<li>
							<a href="#" class="text-slate-400 hover:text-white transition-all duration-300"
								>Política de Privacidade</a
							>
						</li>
						<li>
							<a href="#" class="text-slate-400 hover:text-white transition-all duration-300"
								>Cookies</a
							>
						</li>
					</ul>
				</div>
			</div>

			<div class="border-t border-slate-800 pt-8 text-center">
				<p class="text-slate-500">
					&copy; {new Date().getFullYear()} Autono. Todos os direitos reservados.
				</p>
			</div>
		</div>
	</footer>
</div>

<style>
	/* Importar variáveis de cores */
	@import '../lib/variables.css';

	/* Fade-in animation for mobile menu */
	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(-10px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.animate-fadeIn {
		animation: fadeIn 0.2s ease-out forwards;
	}

	/* Fonte */
	.font-inter {
		font-family: 'Inter', sans-serif;
	}

	/* Animações */
	.animate-in {
		animation: fadeInUp 0.8s ease forwards;
	}

	.animate-pulse-slow {
		animation: pulseSlow 3s infinite;
	}

	@keyframes fadeInUp {
		from {
			opacity: 0;
			transform: translateY(30px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@keyframes pulseSlow {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0.5;
		}
	}

	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(-10px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.animate-fadeIn {
		animation: fadeIn 0.2s ease-out forwards;
	}

	/* Partículas de dados */
	.data-particle {
		position: absolute;
		width: 6px;
		height: 6px;
		border-radius: 50%;
		background: white;
		opacity: 0.7;
	}

	.particle-1 {
		top: 25%;
		left: 30%;
		animation: moveParticle 3s infinite;
	}

	.particle-2 {
		top: 40%;
		left: 25%;
		animation: moveParticle 4s infinite 1s;
	}

	.particle-3 {
		top: 50%;
		left: 55%;
		animation: moveParticle 3.5s infinite 0.5s;
	}

	.particle-4 {
		top: 20%;
		left: 65%;
		animation: moveParticle 2.5s infinite 1.5s;
	}

	@keyframes moveParticle {
		0% {
			transform: translate(0, 0);
			opacity: 0;
		}
		50% {
			opacity: 0.7;
		}
		100% {
			transform: translate(30px, 30px);
			opacity: 0;
		}
	}

	/* Esconder a barra de rolagem */
	.hide-scrollbar {
		-ms-overflow-style: none;
		scrollbar-width: none;
	}

	.hide-scrollbar::-webkit-scrollbar {
		display: none;
	}
</style>
