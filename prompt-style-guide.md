# Prompt and Prompt Commands

The prompt is a symbol (such as `>` or `$`) that displays on an interface when a computer is ready to receive an input. A prompt command is what the user inputs after the prompt.

## Standards

- You must surround all prompt commands with either:
  - singular backticks for in-line quotation, such as \`rm -rf folder\` which Markdown shows as `rm -rf folder`
    or
  - triple backticks for block quotation, such as

    \```

    rm -rf folder

    \```

    which Markdown shows as
    ```
    rm -rf folder
    ```
    Generally, block quotation is preferable, especially if the quotation can - and should - be copied and pasted for easy reuse.

- Block quotation is necessary for multiple lines of prompt commands.
  
- When you provide a sample prompt command, exclude the prompt and only include the command.
  For example:
  ``` 
  pip3 install -U openai
  ```
  - Exception: for certain applications or executables, the prompt may change, and documenting the distinction is necessary.
    For example:
    ```
    C:\> ollama run deepseek-r1:1.5b
    >>> 
    >>> What's the highest mountain in the world?
    ```
