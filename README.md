# Roblox Packages 💻

## Introduction

This repository is a collection of reusable, framework-agnostic Roblox packages.
They are meant to help you build common game systems faster with clean, modular Luau code.

## Tools  & Type checking mode

- Strict module type checking (`--!strict`).
- Roblox services and APIs
- Rojo
- Selene
- Git

## Install (Git + Rojo)

```powershell
git clone https://github.com/<your-name>/roblox-packages.git
cd roblox-packages
```

Build any package:

```powershell
rojo build <package-folder>/default.project.json -o <package-name>.rbxm
```

Or serve it directly to Studio:

```powershell
rojo serve <package-folder>/default.project.json
```

Example:

```powershell
rojo build timer-service/default.project.json -o timer-service.rbxm
```

## Use in Studio

1. Build a package with Rojo.
2. Import the `.rbxm` into Studio.
3. Put it in `ReplicatedStorage`.
4. `require()` it in your scripts.
