

📄 Product Requirements Document (PRD)

🧠 Product Name:

NextMCP

⸻

📌 Summary:

NextMCP is a specialized backend service (MCP Server) that equips LLM-powered agents with tools to deeply understand and manipulate Next.js applications. It provides structural insights and contextual access to project files, enabling agents to intelligently assist in building, navigating, and extending a Next.js codebase. NextMCP will expose a type-safe interface for agents to query page locations, associated actions, frontend components, and contextual file content.

⸻

🎯 Goals and Objectives:

Goal	Description
🧭 Structural Awareness	Provide a detailed map of the Next.js project including pages, routes, actions, components, and their interrelationships.
📖 Contextual File Interpretation	Enable agents to access and understand the content of TypeScript, JSX/TSX, and JSON files with semantic insights.
🤖 Agent-Oriented Tooling	Provide tools agents can invoke to get context, file descriptions, code summaries, and structure-based navigation.
🔐 Safe and Controlled Access	Offer secure, read-only APIs to prevent unauthorized modifications while allowing deep introspection.


⸻

🏗️ Features:

1. Project Structure Indexing
	•	🔍 Crawl and index the entire Next.js project directory:
	•	/app or /pages for route-aware files
	•	/components for reusable components
	•	/lib, /utils, /hooks for logic modules
	•	/actions, /server, /api for backend logic and server actions
	•	🧾 Store metadata such as:
	•	File path
	•	Component or function name
	•	Export type (default, named)
	•	Description extracted from comments or docblocks
	•	Type definitions and inferred props (using static analysis)

2. LLM-Aware Tools Interface
	•	Tools provided via API or embedded via Agent SDK:
	•	getProjectStructure(): Returns the file/folder tree and metadata
	•	describeComponent(filePath: string): Returns description, props, usage
	•	describeAction(filePath: string): Returns description, input/output schema
	•	getFileContext(filePath: string, lineRange?: [number, number]): Returns raw or summarized context
	•	findPagesUsingComponent(componentName: string): Traces usage via import graph
	•	listServerActions(): Lists all server/client actions with inferred type signatures

3. Intelligent Summarization & Parsing
	•	Static parsing of .ts, .tsx, .js, .jsx, .json files
	•	Zod type inference (if applicable)
	•	JSDoc or comment-based description extraction
	•	Component and hook signature inference
	•	Custom AST-based visitors for advanced use cases

4. Real-Time Sync and File Watching (Optional v2)
	•	WebSocket-based change feed
	•	Agents get notified of file changes
	•	Re-index on file save

⸻

📦 APIs and Tooling Contracts

Tool Contract (Agent-facing interface):

type ProjectToolingAPI = {
  getProjectStructure(): Promise<ProjectTree>;
  describeComponent(filePath: string): Promise<ComponentDescription>;
  describeAction(filePath: string): Promise<ActionDescription>;
  getFileContext(filePath: string, lineRange?: [number, number]): Promise<FileContext>;
  findPagesUsingComponent(componentName: string): Promise<string[]>;
  listServerActions(): Promise<ActionDescription[]>;
};


⸻

🧩 Internal Architecture

[File Watcher] --> [Parser Engine] --> [Indexer DB] --> [Tooling API] --> [LLM Agent]

                      ↑                               ↓
               [AST Visitors]                   [Zod / TS Inference]

	•	Parser Engine: Uses Babel, TypeScript compiler API, and AST traversal
	•	Indexer DB: In-memory store or lightweight KV (e.g. Redis or Vercel KV)
	•	Tooling API: REST or RPC endpoints exposed over HTTP or Agent message bus

⸻

🔐 Security & Access Control
	•	Agents only receive read-only access to pre-approved project paths
	•	File path access is validated and sanitized
	•	Future support for scoped org/team-based project access

⸻

📅 Milestones

Milestone	Description	ETA
✅ M1	Project scaffolding, CLI tools, initial file indexer	Week 1
✅ M2	API: getProjectStructure, describeComponent, getFileContext	Week 2
⏳ M3	Advanced parsing: AST visitors, Zod/TS type inference	Week 3
⏳ M4	Tooling API & SDK for Agents	Week 4
⏳ M5	Live project sync + WebSocket tooling (v2)	Week 6


⸻

📈 Success Metrics
	•	⌛ Average response time for agent queries < 300ms
	•	✅ > 95% accuracy of component/action descriptions
	•	🔍 Zero agent tooling failures in production
	•	📚 Agents using describeComponent() in 50%+ codegen attempts

⸻

📌 Stretch Goals
	•	🔁 GitHub/GitLab integration for source-of-truth diffs
	•	🧪 Integration with Jest or Vitest to trace test coverage
	•	📊 Frontend visualization of structure (DevTool UI)

⸻
