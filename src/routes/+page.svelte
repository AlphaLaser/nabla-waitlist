<script>
  import { createClient } from '@supabase/supabase-js';


  const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
  const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

  const supabase = createClient(supabaseUrl, supabaseAnonKey);

  let userInput = '';
  let notificationVisible = false;
  let notificationTimeout;

  const handleSubmit = async () => {
    if (!userInput.trim()) return;

    // Insert email into supabase
    const { data, error } = await supabase
      .from('emails')
      .insert([{ email: userInput.trim() }]);

    if (error) {
      console.error('Supabase insert error:', error);
      // optionally show some error notification
      return;
    }

    console.log('Email logged:', data);

    userInput = '';
    showNotification();
  };

  const showNotification = () => {
    notificationVisible = true;
    clearTimeout(notificationTimeout);
    notificationTimeout = setTimeout(() => {
      notificationVisible = false;
    }, 3000); // 3 seconds
  };

  const dismissNotification = () => {
    clearTimeout(notificationTimeout);
    notificationVisible = false;
  };
</script>

<style>
  html, body {
    height: 100%;
    margin: 0;
    padding: 0;
    overflow: hidden;
  }

  :global(body) {
    font-family: 'Crimson Text', serif;
  }

  .background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-image: url('/trial.png');
    background-size: cover;
    background-position: center center;
    background-repeat: no-repeat;
    z-index: -1;
  }

  .content {
    position: relative;
    z-index: 1;
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .text-block {
    text-align: center;
    color: #F5ECE3;
    margin-top: -15vh;
  }

  h1 {
    font-size: 4rem;
    margin: 0;
    text-shadow:
      -1.3px -1.3px 0 #643F21,
      1.3px -1.3px 0 #643F21,
      -1.3px 1.3px 0 #643F21,
      1.3px 1.3px 0 #643F21;
  }

  p {
    font-size: 1.5rem;
    margin-top: 0.5rem;
    text-shadow:
      -1.0px -1.0px 0 #643F21,
      1.0px -1.0px 0 #643F21,
      -1.0px 1.0px 0 #643F21,
      1.0px 1.0px 0 #643F21;
  }

  .input-group {
    margin-top: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.75rem;
    width: 80ch;
    margin-inline: auto;
  }

  .input-group input {
    flex: 1;
    font-size: 1rem;
    padding: 0.5rem;
    border: 1px solid #F5ECE3;
    border-radius: 0;
    outline: none;
    font-family: inherit;
    background: transparent;
    color: #F5ECE3;
  }

  .input-group button {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    border: 1px solid #643F21;
    background-color: #643F21;
    color: #F5ECE3;
    border-radius: 0;
    font-family: inherit;
    cursor: pointer;
  }

  .input-group button:hover {
    background-color: #8b5c35;
  }

  ::placeholder {
    color: #F5ECE3;
    opacity: 0.9;
  }

  .notification {
    position: absolute;
    top: 2rem;
    right: 2rem;
    background: transparent;
    border: 1px solid #F5ECE3;
    color: #F5ECE3;
    font-family: 'Crimson Text', serif;
    padding: 0.5rem 1rem;
    display: flex;
    align-items: center;
    gap: 0.75rem;
    z-index: 100;
    animation: fadeInOut 3s forwards;
  }

  .notification .close-btn {
    background: none;
    border: none;
    color: #F5ECE3;
    font-size: 1rem;
    cursor: pointer;
    padding: 0;
    line-height: 1;
  }

  @keyframes fadeInOut {
    0% { opacity: 0; transform: translateY(-10px); }
    10% { opacity: 1; transform: translateY(0); }
    90% { opacity: 1; transform: translateY(0); }
    100% { opacity: 0; transform: translateY(-10px); }
  }
</style>

<div class="background"></div>

<div class="content">
  {#if notificationVisible}
    <div class="notification">
      <span>by the will of nabla, we shall be in touch.</span>
      <button class="close-btn" on:click={dismissNotification}>✕</button>
    </div>
  {/if}

  <div class="text-block">
    <h1>getnabla.xyz</h1>
    <p>“let it be henceforth that thou ask not chatgpt to unfold a math problem.”</p>
    <div class="input-group">
      <input
        type="text"
        placeholder="pythagoras@samos.edu"
        bind:value={userInput}
        on:keydown={(e) => e.key === 'Enter' && handleSubmit()}
      />
      <button on:click={handleSubmit}>get early access</button>
    </div>
  </div>
</div>