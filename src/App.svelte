<!-- AuthCard.svelte -->
<script>
  import { slide } from 'svelte/transition';  // Correct import from svelte/transition
  let code = ['', '', '', '', '', ''];
  let OTPkey = '123456';
  let remainingDigits = 6;
  const handleInput = (index, event) => {
    const value = event.target.value;
    
    if (value.length === 1 && index < 5) {
      document.getElementById(`code-${index + 1}`).focus();
    }
    code[index] = value;
    let inputkety = code.join('');
    updateRemainingDigits();
  }

  const updateRemainingDigits = () => {
    remainingDigits = 6 - code.filter(num => num).length;
  }

  const handleKeyDown = (index, event) => {
    if (event.key === 'Backspace' && index > 0) {
      document.getElementById(`code-${index - 1}`).focus();
      code[index] = ''; // Clear the previous input
    } else if (isNaN(event.key)) {
      event.preventDefault();
    }
  }
</script>
<div class="auth-card" in:slide={{ x: 1000, duration: 500 }}>
  <div class="icon">
    🔒
  </div>
  <h1>Easy peasy</h1>
  <p>Enter 6-digit code from your two factor authenticator APP.</p>
  
  <div class="code-inputs">
    {#each code as digit, index}
      <input
        id={`code-${index}`}
        type="text"
        inputmode="numeric"
        maxlength="1"
        bind:value={code[index]}
        on:input={(e) => handleInput(index, e)}
        on:keydown={(e) => handleKeyDown(index, e)}
        class="code-input"
      />
    {/each}
  </div>
  
  {#if remainingDigits > 0}
    <div class="status">
      {remainingDigits} digits left
    </div>
  {:else}
    {#if code.join('') === OTPkey}
       <button class="submit-btn btn-primary" style="width: 100%;">Let's Go</button>
    {:else}
       <button class="submit-btn btn-danger" style="width: 100%;">Wrong code!</button>
    {/if}
  {/if}
</div>

<style>
  /* Background */
  :global(body) {
    margin: 0;
    background-color: #e3f3ff;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    font-family: Arial, sans-serif;
  }
  
  /* Card Container */
  .auth-card {
    width: 500px;
    height: 300px;
    padding: 20px;
    background-color: white;
    border-radius: 15px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    text-align: center;
  }

  /* Icon */
  .icon {
    font-size: 2rem;
    background-color: #d8ebf9;
    width: 50px;
    height: 50px;
    margin: 0 auto;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #4b9ce2;
    margin-bottom: 10px;
  }

  /* Title and Subtitle */
  h1 {
    font-size: 1.5rem;
    color: #333;
    margin: 10px 0 5px;
  }

  p {
    color: #777;
    font-size: 0.9rem;
    margin: 0;
  }

  /* Code Input Fields */
  .code-inputs {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin: 15px 0;
  }

  .code-input {
    width: 40px;
    height: 50px;
    font-size: 1.5rem;
    text-align: center;
    border: 2px solid #4b9ce2;
    border-radius: 8px;
    outline: none;
    color: #4b9ce2;
  }

  /* Status Message */
  .status {
    font-size: 1rem;
    color: #999;
    background-color: #eaeaea;
    padding: 8px;
    border-radius: 8px;
  }

  .submit-btn {
    padding: 8px;
    border-radius: 8px;
  }
</style>

