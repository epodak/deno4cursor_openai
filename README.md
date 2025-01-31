# curxy

#### _cursor_ + _proxy_ = **curxy**

[![JSR](https://jsr.io/badges/@ryoppippi/curxy)](https://jsr.io/@ryoppippi/curxy)
[![JSR](https://jsr.io/badges/@ryoppippi/curxy/score)](https://jsr.io/@ryoppippi/curxy)

An proxy worker for using ollama in cursor

## What is this?

This is a proxy worker for using ollama in cursor. It is a simple server that
forwards requests to the ollama server and returns the response.
这个项目的作用是将 Cursor 编辑器原本调用 OpenAI API 的请求，通过一个代理服务器（curxy）转发到本地运行的 Ollama 服务器，从而实现使用 Ollama 替代 OpenAI 提供语言模型服务。

具体来说，Cursor 编辑器在默认情况下会将请求发送到官方的 Cursor 服务器，而该服务器会进一步将请求转发到 OpenAI 的 API。通过使用这个代理服务器（curxy），可以绕过官方的 Cursor 服务器，直接将请求转发到本地运行的 Ollama 服务器，从而实现以下功能：

1. **使用本地的 Ollama 模型**：用户可以在本地运行 Ollama，并通过这个代理服务器使用自己的模型，而不需要依赖 OpenAI 的远程服务。
2. **保护隐私和数据安全**：由于请求被转发到本地服务器，用户的数据不会离开本地网络，从而提高了数据的安全性和隐私性。
3. **降低成本**：如果用户使用自己的 Ollama 模型，可以避免使用 OpenAI API 产生的费用。

通过设置 `OPENAI_API_KEY` 环境变量，还可以对代理服务器的访问进行限制，增加安全性。

## Why do you need this?

When we use llm prediction on cusor editor, the editor sends to the data to the
official cursor server, and the server sends the data to the ollama server.
Therefore, even if the endpoint is set to localhost in the cursor editor
configuration, the cursor server cannot send communication to the local server.
So, we need a proxy worker that can forward the data to the ollama server.

## requirements

- deno
- ollama server

## How to use

1. Launch the ollama server

2. Launch curxy

   ```sh
   deno run -A jsr:@ryoppippi/curxy
   ```

   if you limit the access to the ollama server, you can set `OPENAI_API_KEY`
   environment variable.

   ```bash
   OPENAI_API_KEY=your_openai_api_key deno run -A jsr:@ryoppippi/curxy

   Listening on http://127.0.0.1:62192/
   ◐ Starting cloudflared tunnel to http://127.0.0.1:62192                                                                                                                                                                                                                                                           5:39:59 PM
   Server running at: https://remaining-chen-composition-dressed.trycloudflare.com
   ```

   You can get the public URL hosted by cloudflare.

3. Enter the URL provided by `curxy` with /v1 appended to it into the "Override
   OpenAl Base URL" section of the cursor editor configuration.

![cursor](https://github.com/user-attachments/assets/83a54310-0728-49d8-8c3f-b31e0d8e3e1b)

4. Add model names you want to "Model Names" section of the cursor editor
   configuration.

![Screenshot 2024-08-22 at 23 42 33](https://github.com/user-attachments/assets/c24fed7c-c61e-46a0-b735-ccf594a96363)

5. (Optional): Additionally, if you want to restrict access to this Proxy Server
   for security reasons, you can set the OPENAI_API_KEY as an environment
   variable, which will enable access restrictions based on the key.

6. **Enjoy!**

Also, you can see help message by `deno run -A jsr:@ryoppippi/curxy --help`

## Related

[Japanese Article](https://zenn.dev/ryoppippi/articles/02c618452a1c9f)

## License

MIT
