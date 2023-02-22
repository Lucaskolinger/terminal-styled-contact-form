<script>
  import Icon from "@iconify/svelte";

  let isCloseCrossShown = false;
  const handleCloseBtnEnter = () => {
    isCloseCrossShown = true;
  };
  const handleCloseBtnLeave = () => {
    isCloseCrossShown = false;
  };

  let mailInputElement;
  let textInputElement;
  let showMailInput = false;
  let showTextInput = false;
  let showMailError = false;
  let showTextError = false;
  let showNameError = false;

  let message = { name: "", email: "", text: ""}

  const onNameSubmit = () => {
     let nameIsValid = validateName();
        if (nameIsValid) {
          showMailInput = true;
          setTimeout(() => {
            mailInputElement.focus();
          }, 1);
        }
  }

  const onMailSubmit = () => {
     let mailIsValid = validateMail();
        if (mailIsValid) {
          showTextInput = true;
          setTimeout(() => {
            textInputElement.focus();
          }, 1);
        }
  }

  const validateName = () => {
    if (message.name.trim().length < 2) {
      showNameError = true;
      return false;
    } else {
      showNameError = false;
      return true;
    }
  };

  const validateMail = () => {
    if (
      message.email.trim().length < 6 ||
      !message.email.includes("@") ||
      !message.email.includes(".")
    ) {
      showMailError = true;
      return false;
    } else {
      showMailError = false;
      return true;
    }
  };

  const validateText = () => {
    if (message.text.trim().length < 6) {
      return false;
      showTextError = true;
    } else {
      showTextError = false;
      return true;
    }
  };

  const handleSubmit = () => {
    let textIsValid = validateText();
    if (textIsValid) {   
      console.log(message)
    } else {
      showTextError = true;
    }
  };
</script>

<article class="terminal-card">
  <div class="controls-row">
    <button
      on:mouseover={handleCloseBtnEnter}
      on:focus={handleCloseBtnEnter}
      on:mouseleave={handleCloseBtnLeave}
      class="system-btn close"
    >
      {#if isCloseCrossShown}
        <Icon width="16" height="16" icon="material-symbols:close-rounded" />
      {/if}
    </button>
    <button class="system-btn yellow" />
    <button class="system-btn green" />
    <p class="top-line">~contact lucas kolinger</p>
  </div>
    <div class="name-input">
      <p>~what's your name?</p>
      <input type="text" bind:value={message.name} autofocus>
      <button on:click={onNameSubmit}><Icon icon="icon-park-solid:enter-key-one"/></button>
    </div>
     {#if showNameError}
        <div class="error">I bet your name has more than one letter</div>
      {/if}
    {#if showMailInput}
      <div class="mail-input">
        <p>~what's your e-mail?</p>
        <input
          type="text"
          bind:this={mailInputElement}
          bind:value={message.email}
        />
         <button on:click={onMailSubmit}><Icon icon="icon-park-solid:enter-key-one"/></button>
      </div>
      {#if showMailError}
        <div class="error">Your email is too short or doesn't contain crucial characters</div>
      {/if}
    {/if}

    {#if showTextInput}
      <div class="text-input">
        <p>~how can I help you?</p>
          {#if showTextError}
      <div class="error">Please write a bit more</div>
      {/if}
        <textarea name="Text"  cols="30" rows="5" bind:value={message.text} bind:this={textInputElement} ></textarea>
      </div>
      <button on:click={handleSubmit}>Submit your message</button>
    {/if}
</article>

<style>
  .terminal-card {
    background-color: purple;
    width: 30rem;
    height: 30rem;
    border-radius: 1rem;
  }
  .controls-row {
    height: 3rem;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    background-color: gray;
    border-top-left-radius: 1rem;
    border-top-right-radius: 1rem;
    padding-inline: 1rem;
  }
  .system-btn {
    width: 1rem;
    aspect-ratio: 1;
    border-radius: 100%;
    border: none;
  }

  .yellow {
    background-color: yellow;
  }
  .close {
    background-color: red;
    /* display: flex;
    justify-content: center;
    align-items: center; */
  }
  .green {
    background-color: green;
  }

  .name-input,
  .mail-input {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-left: 2rem;
  }

  .text-input {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-left: 2rem;
  }

  input {
    background-color: transparent;
    border: none;
  }

  input:focus {
    border: none;
    box-shadow: none;
    outline: none;
  }

  .error {
    color: red;
  }
</style>
