<script>
  import { onMount } from 'svelte';
  import { fade, fly, scale } from 'svelte/transition';
  import { spring } from 'svelte/motion';
  
  export let isVisible;
  
  // Create SVG path data
  function createPaths(position) {
    return Array.from({ length: 36 }, (_, i) => ({
      id: i,
      d: `M-${380 - i * 5 * position} -${189 + i * 6}C-${
        380 - i * 5 * position
      } -${189 + i * 6} -${312 - i * 5 * position} ${216 - i * 6} ${
        152 - i * 5 * position
      } ${343 - i * 6}C${616 - i * 5 * position} ${470 - i * 6} ${
        684 - i * 5 * position
      } ${875 - i * 6} ${684 - i * 5 * position} ${875 - i * 6}`,
      width: 0.5 + i * 0.03,
      delay: Math.random() * 5,
      duration: 20 + Math.random() * 10
    }));
  }
  
  const pathsLeft = createPaths(1);
  const pathsRight = createPaths(-1);
  
  let mounted = false;
  
  onMount(() => {
    mounted = true;
    
    // Animation loop for paths
    const elements = document.querySelectorAll('.animated-path');
    elements.forEach(element => {
      const duration = parseFloat(element.dataset.duration);
      const delay = parseFloat(element.dataset.delay);
      
      setTimeout(() => {
        element.style.animation = `flowPath ${duration}s infinite linear`;
      }, delay * 1000);
    });
    
    return () => {
      mounted = false;
    };
  });
</script>

<!-- Enhanced Hero Section with Animated Background -->
<section class="pt-12 md:pt-44 pb-24 relative overflow-hidden">
  <!-- Fullscreen animated background that extends beyond boundaries -->
  <div class="absolute -inset-1/4 w-[150%] h-[150%] pointer-events-none z-0 opacity-50">
    <!-- Left side paths -->
    <svg class="absolute top-0 left-0 w-full h-full text-purple-500/20 dark:text-purple-500/10" viewBox="0 0 696 516" fill="none">
      {#each pathsLeft as path}
        <path 
          class="animated-path"
          data-duration={path.duration}
          data-delay={path.delay}
          d={path.d}
          stroke="currentColor"
          stroke-width={path.width}
          stroke-opacity={0.1 + path.id * 0.02}
          stroke-dasharray="1"
          stroke-dashoffset="1"
        />
      {/each}
    </svg>
    
    <!-- Right side paths -->
    <svg class="absolute top-0 right-0 w-full h-full text-blue-500/20 dark:text-blue-500/10" viewBox="0 0 696 516" fill="none">
      {#each pathsRight as path}
        <path 
          class="animated-path"
          data-duration={path.duration}
          data-delay={path.delay}
          d={path.d}
          stroke="currentColor"
          stroke-width={path.width}
          stroke-opacity={0.1 + path.id * 0.02}
          stroke-dasharray="1"
          stroke-dashoffset="1"
        />
      {/each}
    </svg>
  </div>
  
  <div class="container mx-auto px-4 relative z-10">
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-center">
      <div class="z-10">
        {#if isVisible}
          <div
            class="inline-block px-4 py-1 bg-slate-800/80 backdrop-blur-sm border border-slate-700 rounded-sm mb-4"
            transition:fly={{ y: 20, duration: 700, delay: 100 }}
          >
            <span class="text-purple-400 font-medium">Automação Inteligente</span>
          </div>

          <h1
            class="text-4xl md:text-5xl lg:text-6xl font-bold mb-6 leading-tight"
            transition:fly={{ y: 30, duration: 800, delay: 200 }}
          >
            Potencializando seu negócio com <span
              class="text-transparent bg-clip-text bg-gradient-to-r from-purple-500 to-blue-500"
              >Hiperautomação</span
            >
            e
            <span
              class="text-transparent bg-clip-text bg-gradient-to-r from-blue-500 to-emerald-500"
              >IA</span
            >.
          </h1>

          <p
            class="text-lg text-slate-300 mb-8 max-w-xl"
            transition:fly={{ y: 30, duration: 800, delay: 400 }}
          >
            Transforme processos complexos em fluxos eficientes e inteligentes. Soluções de
            automação sob medida que geram resultados reais para sua empresa.
          </p>

          <div
            class="flex flex-col sm:flex-row gap-4"
            transition:fly={{ y: 30, duration: 800, delay: 600 }}
          >
            <a
              href="#contato"
              class="bg-gradient-to-r from-purple-600 to-blue-500 hover:from-purple-700 hover:to-blue-600 text-white font-medium py-3 px-8 rounded-sm shadow-xl hover:shadow-purple-500/20 transition-all duration-300 text-center"
            >
              <i class="fas fa-rocket mr-2"></i>
              Comece agora
            </a>
            <a
              href="#servicos"
              class="bg-slate-800 hover:bg-slate-700 border border-slate-700 text-white font-medium py-3 px-8 rounded-sm transition-all duration-300 text-center"
            >
              <i class="fas fa-arrow-right mr-2"></i>
              Conheça nossas soluções
            </a>
          </div>
        {/if}
      </div>

      <div class="relative">
        {#if isVisible}
          <div
            class="w-full h-[420px] relative z-10"
            transition:scale={{ duration: 800, delay: 300, start: 0.8 }}
          >
            <!-- Animated workflow visualization -->
            <div
              class="absolute inset-0 bg-slate-800/30 backdrop-blur-md rounded-sm border border-slate-700/50 overflow-hidden shadow-2xl"
            >
              <div class="relative w-full h-full p-8">
                <!-- Main workflow container -->
                <div class="relative w-full h-full">
                  <!-- Nodes -->
                  <div class="absolute top-[20%] left-[20%] workflow-node">
                    <div class="w-16 h-16 bg-gradient-to-br from-purple-500 to-purple-700 shadow-lg shadow-purple-500/20 rounded-sm flex items-center justify-center workflow-pulse">
                      <i class="fas fa-bolt text-white text-xl"></i>
                    </div>
                    <span class="absolute top-full left-1/2 -translate-x-1/2 mt-2 text-xs text-white/70 bg-slate-800/60 px-2 py-1 rounded-sm">Trigger</span>
                  </div>
                  
                  <div class="absolute top-[15%] left-[60%] workflow-node">
                    <div class="w-16 h-16 bg-gradient-to-br from-blue-500 to-blue-700 shadow-lg shadow-blue-500/20 rounded-sm flex items-center justify-center">
                      <i class="fas fa-cloud text-white text-xl"></i>
                    </div>
                    <span class="absolute top-full left-1/2 -translate-x-1/2 mt-2 text-xs text-white/70 bg-slate-800/60 px-2 py-1 rounded-sm">API</span>
                  </div>
                  
                  <div class="absolute top-[50%] left-[50%] workflow-node">
                    <div class="w-16 h-16 bg-gradient-to-br from-emerald-500 to-emerald-700 shadow-lg shadow-emerald-500/20 rounded-sm flex items-center justify-center">
                      <i class="fas fa-robot text-white text-xl"></i>
                    </div>
                    <span class="absolute top-full left-1/2 -translate-x-1/2 mt-2 text-xs text-white/70 bg-slate-800/60 px-2 py-1 rounded-sm">Process</span>
                  </div>
                  
                  <div class="absolute top-[70%] left-[25%] workflow-node">
                    <div class="w-16 h-16 bg-gradient-to-br from-blue-500 to-emerald-700 shadow-lg shadow-blue-500/20 rounded-sm flex items-center justify-center">
                      <i class="fas fa-database text-white text-xl"></i>
                    </div>
                    <span class="absolute top-full left-1/2 -translate-x-1/2 mt-2 text-xs text-white/70 bg-slate-800/60 px-2 py-1 rounded-sm">Database</span>
                  </div>
                  
                  <div class="absolute top-[70%] left-[75%] workflow-node">
                    <div class="w-16 h-16 bg-gradient-to-br from-purple-500 to-blue-700 shadow-lg shadow-purple-500/20 rounded-sm flex items-center justify-center">
                      <i class="fas fa-chart-line text-white text-xl"></i>
                    </div>
                    <span class="absolute top-full left-1/2 -translate-x-1/2 mt-2 text-xs text-white/70 bg-slate-800/60 px-2 py-1 rounded-sm">Analytics</span>
                  </div>
                  
                  <!-- Connection lines with animated data particles -->
                  <div class="absolute top-[24%] left-[28%] h-[2px] w-[32%] workflow-connection">
                    <div class="data-flow-particle"></div>
                  </div>
                  
                  <div class="absolute top-[26%] left-[60%] h-[24%] w-[2px] workflow-connection-vertical">
                    <div class="data-flow-particle-vertical"></div>
                  </div>
                  
                  <div class="absolute top-[54%] left-[42%] h-[2px] w-[8%] workflow-connection">
                    <div class="data-flow-particle"></div>
                  </div>
                  
                  <div class="absolute top-[58%] left-[58%] h-[2px] w-[17%] workflow-connection">
                    <div class="data-flow-particle"></div>
                  </div>
                  
                  <div class="absolute top-[58%] left-[42%] h-[12%] w-[2px] transform -translate-x-1/2 workflow-connection-vertical">
                    <div class="data-flow-particle-vertical"></div>
                  </div>
                </div>
                
                <!-- Status indicators -->
                <div class="absolute top-4 right-4 flex items-center gap-3">
                  <div class="flex items-center gap-2 bg-slate-900/30 px-3 py-1.5 rounded-sm border border-slate-700/30">
                    <div class="w-2 h-2 rounded-full bg-green-500 animate-pulse"></div>
                    <span class="text-xs text-white/70">Active</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Glow effect -->
            <div class="absolute -z-10 inset-0 blur-2xl opacity-20 bg-gradient-to-r from-purple-600 via-blue-500 to-emerald-500"></div>
          </div>
        {/if}
      </div>
    </div>
  </div>
</section>

<style>
  /* Path animation */
  @keyframes flowPath {
    0% {
      stroke-dashoffset: 1;
    }
    50% {
      stroke-dashoffset: 0;
    }
    100% {
      stroke-dashoffset: -1;
    }
  }
  
  /* Workflow node animations */
  .workflow-node {
    animation: fadeIn 0.8s ease-out forwards;
    opacity: 0;
  }
  
  .workflow-node:nth-child(1) { animation-delay: 0.3s; }
  .workflow-node:nth-child(2) { animation-delay: 0.5s; }
  .workflow-node:nth-child(3) { animation-delay: 0.7s; }
  .workflow-node:nth-child(4) { animation-delay: 0.9s; }
  .workflow-node:nth-child(5) { animation-delay: 1.1s; }
  
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  /* Connection line styling */
  .workflow-connection {
    background: linear-gradient(to right, rgba(139, 92, 246, 0.5), rgba(16, 185, 129, 0.5));
    overflow: hidden;
    position: relative;
  }
  
  .workflow-connection-vertical {
    background: linear-gradient(to bottom, rgba(139, 92, 246, 0.5), rgba(59, 130, 246, 0.5));
    overflow: hidden;
    position: relative;
  }
  
  /* Data flow animation */
  .data-flow-particle {
    width: 8px;
    height: 2px;
    background-color: white;
    position: absolute;
    border-radius: 4px;
    animation: flowRight 2s infinite;
    opacity: 0.7;
  }
  
  .data-flow-particle-vertical {
    width: 2px;
    height: 8px;
    background-color: white;
    position: absolute;
    border-radius: 4px;
    animation: flowDown 2s infinite;
    opacity: 0.7;
  }
  
  @keyframes flowRight {
    0% { left: -10px; opacity: 0; }
    50% { opacity: 0.7; }
    100% { left: 100%; opacity: 0; }
  }
  
  @keyframes flowDown {
    0% { top: -10px; opacity: 0; }
    50% { opacity: 0.7; }
    100% { top: 100%; opacity: 0; }
  }
  
  /* Pulsing effect for trigger node */
  .workflow-pulse {
    position: relative;
  }
  
  .workflow-pulse::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(139, 92, 246, 0.5);
    border-radius: 0.25rem;
    animation: pulse 2s infinite;
    z-index: -1;
  }
  
  @keyframes pulse {
    0% { transform: scale(1); opacity: 0.8; }
    70% { transform: scale(1.1); opacity: 0; }
    100% { transform: scale(1); opacity: 0; }
  }
</style>