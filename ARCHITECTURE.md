# OMP-Qt: Hardened Fork of Oh My Pi with Native Windows GUI

## Core Goals
- Context & Prompt Pinning: Dynamic reinjection / pinning of system instructions to prevent drift across compaction cycles.
- Dual Interface: Shared harness daemon supporting both CLI and a native Qt (Windows) GUI.

## IPC & Security
- Transport: Windows Named Pipes via QLocalSocket using NDJSON / JSON-RPC.
- Access Control: Restrictive DACL scoped to current user SID (QLocalServer::UserAccessOption), per-session handshake token, and GetNamedPipeClientProcessId verification.

## Execution Sandboxing
- Ephemeral Docker containers over WSL2 with only the target project directory bind-mounted.
- Configurable egress (--network=none by default, opt-in network toggle per session).

## Telemetry & Evaluation
- Per-session local JSONL traces + OpenTelemetry metrics (token burn, cache hit rate, tool latency).
- Side-by-side evals against stock OMP: strict system rule injection (e.g., protect config/, run tests after edits) followed by 50 turns of noisy build logs to trigger compaction, verified via automated git diff and trace checks.
