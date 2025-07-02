# Gemini CLI 扩展

Gemini CLI 支持扩展功能，可用于配置和扩展其功能。

## 工作原理

启动时，Gemini CLI 会在两个位置查找扩展：

1.  `<workspace>/.gemini/extensions`
2.  `<home>/.gemini/extensions`

Gemini CLI 会从这两个位置加载所有扩展。如果同名扩展在两个位置都存在，工作区目录中的扩展优先级更高。

在每个位置中，单个扩展作为包含 `gemini-extension.json` 文件的目录存在。例如：

`<workspace>/.gemini/extensions/my-extension/gemini-extension.json`

### `gemini-extension.json`

`gemini-extension.json` 文件包含扩展的配置。该文件具有以下结构：

```json
{
  "name": "my-extension",
  "version": "1.0.0",
  "mcpServers": {
    "my-server": {
      "command": "node my-server.js"
    }
  },
  "contextFileName": "GEMINI.md"
}
```

- `name`：扩展的名称。用于唯一标识扩展。这应该与您的扩展目录名称匹配。
- `version`：扩展的版本。
- `mcpServers`：要配置的 MCP 服务器映射。键是服务器的名称，值是服务器配置。这些服务器将在启动时加载，就像在 [`settings.json` 文件](./cli/configuration.md) 中配置的 MCP 服务器一样。如果扩展和 `settings.json` 文件都配置了同名的 MCP 服务器，则 `settings.json` 文件中定义的服务器优先级更高。
- `contextFileName`：包含扩展上下文的文件名称。这将用于从工作区加载上下文。如果未使用此属性，但扩展目录中存在 `GEMINI.md` 文件，则将加载该文件。

当 Gemini CLI 启动时，它会加载所有扩展并合并它们的配置。如果存在任何冲突，工作区配置优先级更高。
