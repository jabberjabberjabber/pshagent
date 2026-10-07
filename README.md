# PSHAGENT

Powershell first agent based off of the [KoboldCpp Agent](https://github.com/LostRuins/koboldcpp/) created by Lostruins.

## Changes

This agent harness uses the source this commit of [kcpp_agent.py](https://github.com/LostRuins/koboldcpp/commit/62a858419964fbc97065bb65e2bad71ef18cc3cc) 

The following features have been added:

Can run as a MCP client for a local MCP server over stdio using the standard mcp.json configuration
 - `/mcp` lists each server's state, tools, working folder and log file.
 - `/mcp restart [NAME]` restarts one server or all of them.
 - `/mcp reload` re-reads the config, stops the old servers and starts the new ones.
 - Each server runs inside a Windows job object, so it and everything it spawns die with the agent

Static Windows native config and logging.
 - `%APPDATA%\pshagent\config.json` holds defaults.
 - `%LOCALAPPDATA%\pshagent\logs\mcp-<name>.log` contains MCP server logs.

Includes additional tools for document and image extraction (requires xberg CLI in path):
  - `extract_text` Supports text extraction from over 100 file types and OCR, with markdown output by default.
  - `extract_keywords` returns the top keywords from xberg's YAKE or RAKE algorithm, with scores, best first; the detected language; metadata including title, author, creation and modified dates, page count, and EXIF.
  
Remaining context is relayed to the agent after every call. 
 - At the start of each generation, a line like this is added: `[pshagent context: 3,705 of 131,584 tokens used (2%), 127,879 remaining.]`
 - When less than 15% is left, the line tells the model to finish its current step
 - The note isn't saved in the conversation. It's added only to the copy sent with each request, appended to the newest message.
 - `/context` shows current use and whether the note is being sent.
 - `/context on` and `/context off` toggle it while running.
 - `--no-context-relay` or `"context_relay": false` in the config turn it off
 
## Use as Agentic File Sorter

When combined with the [Agentic File Sorter]() MCP server and prompt, you can use this to organize large, disorganized directory trees safely and automatically. 