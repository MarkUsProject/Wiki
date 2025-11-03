# Developer Guide: Configure VSCode


To configure your development environment on VSCode the following steps are required:

## 1. Configure the devcontainer

Devcontainer files define the vscode environment inside a development container. It allows us to configure VSCode extensions, environment definitions, amongst others. 

Place the following file: `devcontainers.json` at the root of your project inside a `.devcontainers` folder.
```json
{
  "name": "MarkUs Dev",
  "dockerComposeFile": ["../compose.yaml"],
  "service": "rails",
  "workspaceFolder": "/app",
  "remoteUser": "markus",
  "overrideCommand": true,
  "forwardPorts": [3000],
  "mounts": ["source=${localWorkspaceFolder}/.ruby-lsp,target=/home/vscode/.ruby-lsp,type=bind"],
  "customizations": {
    "vscode": {"extensions": ["Shopify.ruby-lsp", "eamodio.gitlens", "koichisasada.vscode-rdbg"]},
    "settings": {
      "terminal.integrated.defaultProfile.linux": "bash",
      "rubyLsp.useBundler": true,
      "rubyLsp.rubyVersionManager": "none",
      "rubyLsp.bundleGemfile": "/app/Gemfile",
      "rubyLsp.exclude": [
        "**/.git/**",
        "node_modules/**",
        "vendor/**",
        "log/**",
        "tmp/**",
        ".bundle/**"
      ],
      "git.openRepositoryInParentFolders": "always",
      "git.detectSubmodules": false,

      "[ruby]": {
        "editor.defaultFormatter": "Shopify.ruby-lsp",
        "editor.formatOnSave": true
      }
    }
  },
  "containerEnv": {
    "HOME": "/home/markus",
    "LISTEN_POLLING": "1" 
  }
}
```

Brief explanation of what is happening above:

* On startup, let's open up VSCode inside the `/app` folder of our appplication by defining our workspace folder `workspaceFolder` to point to the `/app` directory of our container.
* We are installing `ruby-lsp` and `gitlens`, both VSCode extensions inside the dev container and specifying their configuration, such as ignoring certain folders from indexing specific paths. Notice that when we open up markus outside the dev container, these extensions will be absent.
* Force the `listen` gem to poll for changes by setting the `LISTEN_POLLING` flag. This removes flakyness in our autoreloading. 
