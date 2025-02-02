<script>
    let message = "Welcome to XchangeMate";
    import { onMount } from 'svelte';
  
    onMount(() => {
      setTimeout(() => {
        window.location.href = '/Currency-Exchange'; // Use the path you want to navigate to
      }, 5000); // 5 seconds delay
    });
  </script>
  
  <style>
    @keyframes gradientBG {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
  
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-20px); }
      to { opacity: 1; transform: translateY(0); }
    }
  
    @keyframes backgroundChange {
      0%, 100% { background-image: url('/splashscreen/splash_bg1.jpg'); }
      25% { background-image: url('/splashscreen/splash_bg2.jpg'); }
      50% { background-image: url('/splashscreen/splash_bg3.jpg'); }
      75% { background-image: url('/splashscreen/splash_bg4.jpg'); }
    }
  
    main {
      animation: backgroundChange 30s infinite ease-in-out;
      background-size: cover;
      background-position: center;
    }
    .overlay {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(217deg, rgba(47, 0, 255, 0.8), rgba(255,0,0,0) 70.71%),
              linear-gradient(127deg, rgba(53, 147, 255, 0.8), rgba(0,255,0,0) 70.71%),
              linear-gradient(336deg, rgba(25, 0, 255, 0.8), rgba(0,0,255,0) 70.71%);
    }
  
    h1 {
      position: relative;
      z-index: 1;
    }
  </style>
  
  <main class="flex justify-center items-center h-screen text-center">
    <div class="overlay"></div>
    <h1 class="text-white text-3xl md:text-5xl font-bold drop-shadow-lg animate-[fadeIn_2s_ease-in-out]" style="color: white;text-shadow: 10px 0px 10px #000000;">{message}</h1>
  </main>
  