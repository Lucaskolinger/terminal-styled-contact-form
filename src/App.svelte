<script>
  import Icon from "@iconify/svelte";

  let mailInputElement;
  let textInputElement;
  let nameInputElement;
  let showMailInput = false;
  let showTextInput = false;
  let showMailError = false;
  let showTextError = false;
  let showNameError = false;

  let message = { name: "", email: "", text: "" };

  setTimeout(() => {
    nameInputElement.focus();
  }, 1);

  const onNameSubmit = () => {
    let nameIsValid = validateName();
    if (nameIsValid) {
      showMailInput = true;
      setTimeout(() => {
        mailInputElement.focus();
      }, 1);
    }
  };

  const onMailSubmit = () => {
    let mailIsValid = validateMail();
    if (mailIsValid) {
      showTextInput = true;
      setTimeout(() => {
        textInputElement.focus();
      }, 1);
    }
  };

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
    if (textIsValid && !showMailError && !showTextError) {
      console.log(message);
      alert("You submitted your message!");
      location.reload();
    } else {
      showTextError = true;
    }
  };
</script>

<article class="terminal-card">
  <div class="controls-row">
    <button class="system-btn close" />
    <button class="system-btn yellow" />
    <button class="system-btn green" />
    <p class="top-line">~ Contact Lucas Kolinger</p>
  </div>

  <form name="contact" on:submit|preventDefault={handleSubmit}>
    <input type="hidden" name="form-name" value="contact" />
    <div class="name-input">
      <p class="prompt">~ What's your name?</p>
      <div class="input-and-enter">
        <input
          type="text"
          name="name"
          bind:value={message.name}
          bind:this={nameInputElement}
        />
        <div class="enter" on:click={onNameSubmit} on:keyup={onNameSubmit}>
          <Icon
            color="hsl(349, 50%, 50%)"
            width="1.3rem"
            icon="icon-park-solid:enter-key-one"
          />
        </div>
      </div>
    </div>
    {#if showNameError}
      <div class="error">I bet your name has more than one letter</div>
    {/if}
    {#if showMailInput}
      <div class="mail-input">
        <p class="prompt">~ What's your e-mail?</p>
        <div class="input-and-enter">
          <input
            type="email"
            name="mail"
            bind:this={mailInputElement}
            bind:value={message.email}
          />
          <div class="enter" on:click={onMailSubmit} on:keyup={onMailSubmit}>
            <Icon
              color="hsl(349, 50%, 50%)"
              width="1.3rem"
              icon="icon-park-solid:enter-key-one"
            />
          </div>
        </div>
      </div>
      {#if showMailError}
        <div class="error">
          Your email is too short or doesn't contain crucial characters
        </div>
      {/if}
    {/if}

    {#if showTextInput}
      <div class="text-input">
        <p class="prompt">~ How can I help you?</p>
        {#if showTextError}
          <div class="text-error">Please write a bit more</div>
        {/if}
        <textarea
          name="Text"
          cols="30"
          rows="5"
          bind:value={message.text}
          bind:this={textInputElement}
        />
      </div>
      <button class="submit-button" type="submit">Send message</button>
    {/if}
  </form>
</article>

<style>
  .terminal-card {
    background-color: var(--terminal-color);
    width: 80vw;
    max-width: 40rem;
    min-height: 30rem;
    border-radius: 0.5rem;
    border: 0.25px solid hsl(0, 0%, 25%);
  }
  .controls-row {
    height: 3.5rem;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: var(--text-color);
    background-color: var(--top-bar-color);
    border-top-left-radius: 0.5rem;
    border-top-right-radius: 0.5rem;
    padding-inline: 1rem;
  }
  .system-btn {
    width: 1rem;
    aspect-ratio: 1;
    border-radius: 100%;
    border: none;
  }

  .top-line {
    margin-left: 1rem;
  }

  .yellow {
    background-color: hsl(60, 50%, 50%);
  }
  .close {
    background-color: hsl(0, 50%, 50%);
  }

  .green {
    background-color: hsl(90, 50%, 50%);
  }

  .name-input,
  .mail-input {
    display: flex;
    flex-direction: column;
    margin-inline: 2rem;
  }

  .name-input {
    margin-top: 1rem;
  }

  .prompt {
    color: var(--text-color);
  }

  .input-and-enter {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  input {
    border: none;
    padding-left: 1rem;
    font-family: "Source Code Pro", monospace;
    width: 80%;
    color: var(--input-color);
    font-size: 1rem;
    background-color: var(--top-bar-color);
    border-radius: 1rem;
    height: 2.5rem;
  }

  input:focus {
    border: none;
    box-shadow: none;
    outline: none;
  }

  .text-input {
    display: flex;
    flex-direction: column;
    margin-inline: 2rem;
    max-width: 25rem;
  }

  .text-input textarea {
    background-color: var(--top-bar-color);
    font-family: "Source Code Pro", monospace;
    font-size: 1rem;
    color: var(--input-color);
    border-radius: 0.5rem;
    padding: 1rem;
    outline: none;
    border: none;
    caret-color: var(--input-color);
  }

  .error {
    margin-inline: 2rem;
    margin-bottom: 1rem;
    margin-top: 0.5rem;
    color: var(--input-color);
  }

  .text-error {
    color: var(--input-color);
    margin-bottom: 1rem;
  }

  .submit-button {
    border-radius: 1rem;
    border: none;
    margin-top: 2rem;
    padding-inline: 1rem;
    padding-block: 0.5rem;
    background-color: var(--text-color);
    color: var(--input-color);
    margin-left: 2rem;
    font-family: "Source Code Pro", monospace;
    margin-bottom: 2rem;
  }

  .submit-button:hover {
    background-color: hsl(349, 50%, 55%);
  }

  @media screen and (min-width: 560px) {
    .name-input,
    .mail-input {
      flex-direction: row;
      align-items: center;
    }
    .input-and-enter {
      width: 100%;
    }
    .prompt {
      width: 25rem;
    }
  }
</style>
