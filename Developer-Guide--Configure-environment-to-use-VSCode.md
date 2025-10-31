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

* We are telling VSCode, that on startup, we be placed inside the `/app` folder of our appplication by defining our workspace folder `workspaceFolder` to point to the `/app` directory of our container.
* We are installing `ruby-lsp` and `gitlens`, both VSCode extensions inside the dev container and specifying their coniguration, such as ignoring certain folders from their indexing specific paths. Notice that when we open up markus outside the dev container, these extensions will be absent.
* Force the `listen` gem to poll for changes by setting the `LISTEN_POLLING` flag. This removes flakyness in our autoreloading. 

## 2. Install ruby-lsp

For us to have the modern programming features such as file tracking, go to navigation, symbol search, code completetion, etc, we must use an lsp server. We use `ruby-lsp` to provides us these features. 

Go to your `gemfile` and add the following line to the `:development` group. `gem 'ruby-lsp', require: false`. 

This will install `ruby-lsp` when we run the `dep-updater` command and give us modern programming features. We refrain from commiting this change to the `gemfile` as the RubyMine IDE already comes with a Ruby LSP preinstalled. Be sure to ignore this file when commiting changes.

