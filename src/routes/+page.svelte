<script lang="ts">
    import { onMount } from 'svelte';
    import { Dice5 } from 'lucide-svelte';
    import { Button } from '$lib/components/ui/button';
    import { Card } from '$lib/components/ui/card';
    
    let advice = '';
    let adviceId = 0;
    let isLoading = false;
  
    async function getAdvice() {
      isLoading = true;
      try {
        const response = await fetch('https://api.adviceslip.com/advice');
        const data = await response.json();
        advice = data.slip.advice;
        adviceId = data.slip.id;
      } catch (error) {
        console.error('Error fetching advice:', error);
      }
      isLoading = false;
    }
  
    onMount(() => {
      getAdvice();
    });
  </script>
  
  <div class="min-h-screen bg-[#1f2632] flex items-center justify-center p-4">
    <Card class="max-w-[540px] w-full bg-[#323a49] rounded-[15px] relative flex flex-col items-center p-12 gap-6">
      <h1 class="text-[#52ffa8] tracking-[4px] text-xs font-extrabold">
        ADVICE #{adviceId}
      </h1>
      
      <p class="text-[#cee3e9] text-[28px] text-center font-extrabold">
        "{advice}"
      </p>
  
      <div class="w-full flex items-center gap-4 my-2">
        <div class="h-[1px] bg-[#4f5d74] flex-1"></div>
        <div class="text-[#4f5d74] text-2xl font-bold">❞❞</div>
        <div class="h-[1px] bg-[#4f5d74] flex-1"></div>
      </div>
  
      <Button
        on:click={getAdvice}
        disabled={isLoading}
        class="absolute -bottom-8 w-16 h-16 rounded-full bg-[#52ffa8] hover:bg-green-300 hover:shadow-[0_0_30px_rgba(82,255,168,0.5)] transition-shadow duration-300 p-0 flex items-center justify-center"
      >
        <Dice5 class="w-6 h-6 text-[#1f2632] {isLoading ? 'animate-spin' : ''}" />
      </Button>
    </Card>
  </div>
  
  <style>
    :global(body) {
      margin: 0;
      font-family: 'Manrope', sans-serif;
    }
  </style>
