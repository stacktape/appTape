# **appTape** (by Stacktape)

appTape is a free, local, open-source AI app builder (like Lovable, Bolt.new or Replit) that deeply integrates with your AWS account.

By simply using human language, it allows:
- non-developers to create simple fullstack apps, workflows and automations
- developers to quickly create MVPs and leapstart new projects

Thanks to integrating Stacktape, it has all of the features of a modern PaaS platform conveniently integrated into the UI.

Unlike the incumbents:
- It uses real backend (not just Supapase)
    - Users can work with technologies they already know
    - Can generate code for any type of app (frontend apps, fullstack apps, APIs, ETL pipelines, etc.)
- It's deeply integrated with AWS
    - **CAN WORK WITH EXISTING APPS AND DATA-SOURCES INSIDE THE AWS ACCOUNT** 
    - Can deploy any type of app (serverless, frontend, backend, fullstack, microservices,...)
    - Allows users to manage apps it deploys (browse logs, metrics, shell to containers, etc.) 
    - AI to be used to fix bugs (having context of the whole environment, and ability to access logs)
    - Cheapest hosting for individuals/small apps (thanks to generous AWS free tier), and lucrative for growing startups (thanks to AWS activate program) 
- It's local (distributed as a desktop app running on user's system). Thanks to this, it's
    - Much more performant
    - No compromises on development experience (no quirky web container emulation)
    - Compliant (SoC 2, HIPAA), as no user data are process by 3rd party (us)
- It's open-source (more precisely, open-core)
    - Usable entirely for free (Bring-Your-Own-API-Key)
    - Has better trust from developers
    - Has superior distribution channel

Thanks to being both local and open-source, it can be sold to enterprises with compliance requirements (SoC 2, HIPAA), as no user data is ever processed by a 3rd party.

# Business model

## appTape Hobby

"Managed mode" - frictionless experience with very-fast time-to-value. No need to configure anything.
"High-control mode" - for developers, who want to use their own API keys, preferred models, and own AWS account.

- AI models:
  - Use appTape AI credits (max $3 worth of tokens per month). Sufficient to generate smaller applications. We'll use Cerebras GPT-OSS-120b model (cheap, super-fast)
  - or Bring-Your-Own-API-Key
- Infrastructure (Serverless only - Lambda functions, OpenNext Next.js, DynamoDb, S3, CloudFront):
  - Deploy to Stacktape sandbox (maximum ~$3 worth of infrastructure per month). Suitable for apps and websites without large amounts of users.
  - or Deploy to your own AWS account (requires connecting, so it can create friction for non-developers)
- Basic context optimization techniques
- Community support

Price: $0 / user / month

## appTape Pro Individual (distributed as an addon to the downloadable desktop app)

- Unlimited users, team management
- Advanced context optimization techniques
- Advanced infrastructure capabilities (Aurora SQL databases, Redis, OpenSearch, private networking)
- Slack/Email support

Price (2 modes):
- Bring-Your-Own-API-Key - $19 / month
- Managed - $39 / month (includes $25 worth of usage, then billed based on usage)
- Max - $199 / month ($250 worth of usage, then billed based on usage)

Upsell trigger: 
- LLMs start to halucinate when their context is ~50-80% full, so without a proper context management, users will get stuck when developing non-trivial projects. When we see that users is stuck (trying to fix something multiple times without being successful), we offer appTape Pro with a discount for 1 month.
- Proper context management is also necessary to reduce costs (up to 80% less) and increase speed

## appTape Pro Team

- Everything in appTape Pro Individual
- Unlimited users, team management, usage stats
- Code security scanning
- AWS Infrastructure guardrails (enforcing security and scalability best practices)
- Centralized team billing
- SAML/OIDC SSO

Price:
- Managed - $49 / user / month (includes $25 worth of usage)
- Bring-Your-Own-API-Key - $29 / user / month

# How it works internally

Apptape will also be open-source (app)

Apptape is an open-source project (Aapache 2.0) forked from dyad.sh (https://github.com/dyad-sh/dyad). It improves on the already great product in multiple ways.

# PaaS UI features
Integrates existing PaaS UI components and functionality from Stacktape Console UI. This includes logs browser, metrics browser, deployment history, CI/CD, rollbacks, etc. directly to the appTape UI.

# AWS deployment engine
Integrates Stacktape (which will also be made open-source under Apache 2.0 license). This allows users to deploy apps to their own AWS account. This allows appTape to create a much broader set apps:
- Frontend: both static sites and Next.js Server-side rendered apps (using S3, Cloudfront and Serverless Next.js hosting via OpenNext)
- Backend: APIs, data pipeleines, CRON jobs and much more (using Containers, Lambda functions, RDS, DynamoDb, Redis, etc.)

# Other changes
- Tweaks and improvments to the UI and UX
- Changes to the code-generation system prompt, so that by default it generates a tRPC API hostable on Lambda function that saves data to DynamoDb, instead of using Supabase.
- ...and many more...

# Context optimization engine

This is a very important part, and technologically challenging part. Giving only the relevant context to LLMs helps with:
- reducing hallucinations
- improving output quality
- speed of the generation
- costs

There are know techniques:
1. Prompt Engineering & Summarization: Use LLM to summarize files/projects; include summaries in prompts instead of full code.
2. Retrieval-Augmented Generation (RAG) with Embeddings: Embed code, store in vector DB, retrieve semantically similar ones for queries.
3. Memory Management (Sliding Windows) - Track session history (multi-turn conversations); use vector stores for long-term memory, evict old/irrelevant context.

Interestingly, one twitter user has reverse-engineered how Cursor does it: https://x.com/archiexzzz/status/1952062945204355233

Context-optimization engine will be developed as a closed-source project. Source will be available on demand (should enterprise users need it for compliance/security audit reasons).
It will be distributed as a binary "addon", pluggable to the appTape core, communicating with the core using IPC channel.

The embeddings and vector store data will be saved locally, on the user's machine, for compliance/security reasons, and also for increased performance. Plus, to save our costs for infrastructure.
