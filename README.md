# Figma to React Native Screen Generator - Setup Guide

This guide will help you set up the automated Figma to React Native screen generation system using MCP (Model Context Protocol) integration.

## Prerequisites

- **Podman** installed on your machine
- **VS Code** installed
- **Figma account** with access to design files
- **Copilot** access in VS Code

## Step-by-Step Setup

### 1. Clone this project and Navigate to it
```bash
git clone https://github.com/bbalakriz/figma-rn-screen-generator
cd figma-rn-screen-generator
```

### 2. Configure Figma Personal Access Token (PAT)

1. **Create a Figma PAT:**
   - Visit: https://help.figma.com/hc/en-us/articles/8085703771159-Manage-personal-access-tokens
   - Follow the instructions to generate your Personal Access Token
   - **Important:** Copy and save this token securely - you won't be able to see it again

2. **Update Environment Configuration:**
   - Open the `compose.yml` file in the project root
   - Locate the `FIGMA_KEY` environment variable
   - Replace the placeholder value with your actual Figma PAT:
   ```yaml
   environment:
     - FIGMA_KEY=your_actual_figma_token_here
   ```

### 3. Start the Figma MCP Server

```bash
# Start the services in detached mode
podman-compose -f compose.yml up -d

# Verify the figma-mcp-server is running
podman ps -a
```

**Expected Output:** You should see `figma-mcp-server` with status `Up` or `Running`

**Troubleshooting:** If the server is not running, check the logs:
```bash
podman logs figma-mcp-server
```

### 4. Configure VS Code MCP Integration

1. **Open MCP Configuration:**
   - In VS Code, press `Cmd+Shift+P` (macOS) or `Ctrl+Shift+P` (Windows/Linux)
   - Type and select: `MCP: Open User Configuration`

2. **Update Configuration:**
   Replace the entire content with:
   ```json
   {
     "servers": {
       "figma-mcp-server": {
         "url": "http://localhost:3333/sse",
         "type": "http"
       }
     },
     "inputs": []
   }
   ```

3. **Restart VS Code** to apply the MCP configuration changes

### 5. Prepare the Prompt File

1. **Locate the prompt file contained in this repository :** `figma-to-screen-prompt.md`
2. **Copy to accessible location:** Move this file to a location you can easily reference (e.g., your Documents folder)
3. **Remember the path** - you'll need to reference this file in VS Code

### 6. Open the Mobile App Project

1. **Open VS Code**
2. **Open the `MobileAppCheckIn` folder** in VS Code

### 7. Generate Screens from Figma

#### Get Figma Design URL:
1. **Open your Figma design**
2. **Navigate to the specific screen/component** you want to convert
3. **Right-click on the artboard/frame**
4. **Select:** `Copy as` → `Copy link to selection`
5. **Copy the generated URL**

#### Generate React Native Code:
1. **Open Copilot in VS Code**
2. **Switch to Agent mode**
3. **Add the prompt file to context:**
   - Reference the `figma-to-screen-prompt.md` file you copied earlier
4. **Issue the generation command:**

```
Using the Figma design at [PASTE_FIGMA_URL_HERE], generate a complete React Native screen following all the architectural patterns and guidelines specified in the prompt file. Include:

1. Extract all design elements and download images
2. Create the main component with proper TypeScript interfaces
3. Generate the accompanying styles file using the theme system
4. Implement proper error handling and responsive design
5. Integrate with existing navigation if it's a screen-level component
6. Ensure pixel-perfect accuracy and proper asset management

Please analyze the Figma design first, then proceed with the complete implementation.
```
