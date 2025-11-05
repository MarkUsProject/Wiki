# Developer Guide: Configure VSCode

To configure your development environment on VSCode the following steps are required:

## Setup

Install the dev containers [extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) inside VSCode.

Devcontainer files define the vscode environment inside a docker container. It allows us to configure VSCode extensions, environment definitions, etc when running VSCode inside the docker container.

Place the following file: `devcontainer.json` at the root of your project inside a `.devcontainer` folder.

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

- On startup, let's open up VSCode inside the `/app` folder of our appplication by defining our workspace folder `workspaceFolder` to point to the `/app` directory of our container.
- We are installing `ruby-lsp` and `gitlens`, both VSCode extensions inside the dev container and specifying their configuration, such as ignoring certain folders from indexing specific paths. Notice that when we open up markus outside the dev container, these extensions will be absent.
- Force the `listen` gem to poll for changes by setting the `LISTEN_POLLING` flag. This removes flakyness in our autoreloading.

## Execution

To run your code inside the dev container extension open up the command palette, either by pressing down `cmd + Shift + P` (on MAC OS) or by going to `view > Command Palette` and typing in `Dev Containers: Reopen in Container`
