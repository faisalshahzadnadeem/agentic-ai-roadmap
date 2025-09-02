# agentic-ai-roadmap
Learning and building agentic AI systems step by step using LangChain, LangGraph, CrewAI, AutoGen, and more.
agentic-ai-roadmap/
│
├── apps/                # End-user projects for each phase
│   ├── week1_agentic_fundamentals/
│   │   └── research_writer_agent/
│   ├── week2_langgraph/
│   │   └── email_triage_agent/
│   ├── week3_agentic_rag/
│   └── ...
│
├── packages/            # Reusable code
│   ├── tools/           # web search, calculator, email, file I/O
│   ├── memory/          # SQLite/Redis/vector DB memory
│   ├── orchestration/   # LangGraph graphs, supervisors, routers
│   └── evals/           # rubrics, golden sets, metrics
│
├── infra/               # Deployment setup
│   ├── docker/
│   └── k8s/
│
├── scripts/             # Helper scripts
├── tests/               # Unit tests
└── README.md            # Main documentation

