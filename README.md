<img width="1730" height="521" alt="logo-11" src="https://github.com/user-attachments/assets/5550063b-7ff8-4c1f-b511-83f48ac862fe" />


<p align="center">
  <a href="#quick-start">Quick Start</a> •
  <a href="#features">Features</a> •
  <a href="#how-it-works">How It Works</a> •
  <a href="#learn-more">Learn More</a> •
  <a href="#contributing">Contributing</a>
</p>

---

## 🌟 Introduction

GACUA (Gemini CLI as Computer Use Agent) stands at the forefront of human-computer interaction, offering a powerful, open-source tool that reimagines how we control our digital environment. By leveraging the advanced capabilities of the Gemini CLI, GACUA allows you to operate your computer using simple, natural language commands.

While the field of AI-driven computer agents is still in an early, dynamic phase of development, GACUA is a powerful and remarkably user-friendly tool for exploration. It provides a tangible glimpse into the future of computing, making it an exciting project for developers, researchers, and anyone curious about the next wave of AI innovation.

## ✨ Features

GACUA extends the core capabilities of Gemini to give you hands-on control over your computer. Here’s what makes GACUA stand out:

- **💻 Control Your Computer from Anywhere:** Interact with your computer using natural language commands from your mobile phone or another computer.
- **🎯 High-Accuracy Grounding:** GACUA enhances Gemini 2.5 Pro's grounding capabilities through advanced image slicing, dramatically improving the accuracy of on-screen element identification.
- **🕹️ Master OS-Level Operations:** Effortlessly perform a wide range of computer-based tasks, from playing desktop games and configuring OS settings to manipulating software with simple commands.
- **🔬 Step-by-Step Control & Observability:** Unlike black-box agents, GACUA provides a transparent, step-by-step execution flow. You can review the agent's planning and grounding process and approve or reject each proposed action, giving you full control over the task execution.

## 🚀 Quick Start

Get up and running with GACUA in just a few steps.

### Prerequisites

- **Node.js (version 20 or higher):** GACUA is built on Node.js. If you don't have it installed, you can download it from the [official Node.js website](https://nodejs.org/en/download/). The installer will also install npm (Node Package Manager).
- **Gemini Authentication:** GACUA needs to authenticate with the Gemini API. While `gemini-cli` is not required to *run* GACUA, the easiest way to set up authentication is by installing and configuring the [Gemini CLI](https://github.com/google-gemini/gemini-cli) first. GACUA will automatically reuse the configuration created by it.

### Installation and Execution

Simply run the following command in your terminal to start GACUA:

```bash
npx @gacua/backend
```

This command uses `npx` (Node Package Execute) to download and run the GACUA backend package without needing to install it globally. The first time you run this, it may take a few moments to download the necessary files. To see detailed installation progress, place the `--verbose` flag immediately after `npx`:

```bash
npx --verbose @gacua/backend
```

Alternatively, you can install GACUA globally using npm:

```bash
npm install -g @gacua/backend && gacua
```

This will install the GACUA package on your system, allowing you to run it from any directory by simply typing `gacua`.

### ❗️ Important: Network Configuration

GACUA operates as a local web server, allowing you to control your PC from another device, like a mobile phone. For this to work, **both devices must be on the same network**.

- **Connect to the same Wi-Fi:** The simplest method is to connect your computer and your controlling device (e.g., your phone) to the same Wi-Fi network.
- **Use a mobile hotspot:** If you don't have a shared Wi-Fi network, you can use your phone's hotspot and connect your computer to it.
- **Check your firewall:** Your computer's firewall might block incoming connections. If you can't connect, ensure that your firewall settings allow access to the port GACUA is running on. You may need to create a new inbound rule for Node.js or the specific port.

## 🔧 How It Works

GACUA includes a specialized MCP (Model-View-Controller-Prompter) tool for computer control and operates as a web server. This architecture creates a seamless connection between the computer you want to control and the device you're using to issue commands.

### Running GACUA in Decoupled Mode

By default, GACUA runs as an all-in-one application. However, for more advanced use cases, such as controlling a computer on a different network, you can run its core components separately. This "decoupled mode" separates GACUA's **🧠 Brain** (which requires API access) from its **💪 Body** (which executes commands), allowing them to operate on different machines.

**A stable network connection between the two machines is crucial for this mode to function correctly.**

1. **Start the MCP computer server (the 💪 Body):**

   On the computer you want to control, run the following command. This machine **does not** need your Gemini API keys.

   ```bash
   npx @gacua/mcp-computer --host <MCP_HOST> --port <MCP_PORT>
   ```

   This command starts the MCP server, which will listen for commands to execute on the local machine.

2. **Launch the GACUA backend (the 🧠 Brain):**

   On the controlling device with authenticated access to the Gemini API, run the following command:

   ```bash
   GACUA_MCP_COMPUTER_URL=http://<MCP_HOST>:<MCP_PORT>/mcp npx @gacua/backend
   ```

   - `GACUA_MCP_COMPUTER_URL`: This environment variable tells the "Brain" the endpoint of the "Body" you started in the previous step.

## 🛠️ Development

Interested in contributing to GACUA? Here’s how you can get your development environment set up and run the project from source.

### Initial Setup

After cloning the repository, you need to install the dependencies and perform an initial build. This is a required first step.

1.  **Install all package dependencies:**

    ```bash
    npm install
    ```

2.  **Build all packages:**
    This command compiles all the packages within the monorepo.

    ```bash
    npm run build
    ```

### Run in Development Mode

For active development, this mode provides hot-reloading for the frontend and backend.

1.  **Start the development servers:**

    ```bash
    npm run dev:gacua
    ```

This command starts the Vite frontend server (on port `5173`) and the Express backend server (on port `3000`). Follow the link printed in your terminal, but **remember to change the backend URL's port from `3000` to `5173` in the UI**. The Vite server is configured to proxy requests to the backend.

**Note:** The `dev` command only watches for changes in the `@gacua/backend` and `@gacua/frontend` packages. If you modify any other package, you will need to stop the server and run `npm run build` again.

### Run After Building (Production Simulation)

To run the application as it would be in production, where the backend serves the built frontend files:

1.  **Build the project (if you have new changes):**

    ```bash
    npm run build
    ```

2.  **Start the application:**

    ```bash
    npm run start:gacua
    ```

In this mode, the frontend artifacts are served by the backend, so you can access the entire application on port `3000`.

### Testing the Local Binary with npx

To test the `gacua` command-line interface from your local build (simulating how a user would run it), follow these steps carefully:

1.  **Install dependencies:**

    ```bash
    npm install
    ```

2.  **Build all packages:**

    ```bash
    npm run build
    ```

3.  **Install again to link the binary:**

    ```bash
    npm install
    ```

    This second `npm install` is crucial. After the `build` step creates the executable files, this command links the local `gacua` binary into the `node_modules/.bin` directory, making it available to `npx`.

4.  **Run GACUA:**

    ```bash
    npx gacua
    ```

## 📚 Learn More

- **Why We Built GACUA: Our Story and Learnings:** Dive into the technical challenges and key insights from our development journey.
- **Under the Hood: GACUA's Architecture:** A deep dive into GACUA's core decoupled components.
- **Troubleshooting:** Solutions to common issues, such as the agent capturing black screenshots when run via SSH.

## 🤝 Contributing to GACUA

We welcome contributions from the community! Whether you want to fix a bug, add a new feature, or improve the documentation, your help is valuable. To get started, check out our [Contributing to GACUA](CONTRIBUTING.md) guide to set up your local development environment and learn how you can help shape the future of GACUA.

## ⭐ Stay Ahead

Star GACUA on [GitHub](https://github.com/) to be instantly notified of updates. Your support means everything to us! ❤️



<img width="1731" height="358" alt="image" src="https://github.com/user-attachments/assets/57389a49-4bb0-4246-8b6c-3d4cc2b90c6e" />

<img width="1731" height="313" alt="image" src="https://github.com/user-attachments/assets/e2d9b778-9451-4634-87ba-7becce5cae13" />

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/c53486e2-9a93-4c45-bd7c-c6611c7006c6" />

## 📜 License

GACUA is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
