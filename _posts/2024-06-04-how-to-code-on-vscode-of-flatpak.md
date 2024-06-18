---
title: how to code on vscode of flatpak
date: 2024-06-04
---

# How to Code on VSCode of Flatpak

As we all know, Flatpak applications run in a sanbox that isolates runtimes from host environment. This makes our host system safer, but it raises the question of how we can use the host's development environment (such as dev tools, environment variables, etc.).

There are two ways to solve this problem:

### Method 1: Using `flatpak-spawn`

You can use the `flatpak-spawn` command to access the host environment from within a Flatpak application. To do this in Visual Studio Code, insert the following code fragment into your VSCode settings:

```json 
"terminal.integrated.defaultProfile.linux": "flatpak-spawn",
"terminal.integrated.profiles.linux": {
  "flatpak-spawn": {
    "path": "/usr/bin/flatpak-spawn",
    "args": ["--env=TERM=vscode", "--host", "script", "--quiet", "/dev/null"]
  }
}
```

This configuration allows the terminal in VSCode to use the host environment.

### Method 2: Remote Development (Recommend)

Another approach is to use the `Remote Development` extension in VSCode. This extension pack includes tools for remote development.

1. Install the `Remote Development` extension pack.
2. Check the host IP address for the Flatpak runtime. You can use the `ip addr` command to find it. Look for the IP address that belongs to `br-xxxxxxx`. In my case, the host IP address is `172.18.0.1`.
3. Ensure that the SSH daemon (`sshd`) service is configured and running on your host.
4. Connect to the host using the IP address in your VSCode to enjoy a seamless coding experience.

By using either of these methods, you can effectively bridge the gap between your Flatpak sandbox and the host environment, allowing for a more integrated development workflow.
