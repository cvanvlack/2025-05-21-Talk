# 2025-05-21-Talk

# AI-Specific Tools Used

- [ChatGPT](https://openai.com/chatgpt/download/)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Cursor](https://www.cursor.com/)
- [Repomix](https://repomix.com/)
- [Whispr Flow](https://wisprflow.ai/)

# Reference Documents

- [Harper Reed - My LLM codegen workflow atm](https://harper.blog/2025/02/16/my-llm-codegen-workflow-atm/)
- [Harper Reed - Basic Claude Code](https://harper.blog/2025/05/08/basic-claude-code/)
- [Harper Reed - Claude Commands](https://github.com/harperreed/dotfiles/tree/master/.claude/commands)

# Things that require a while to setup that AI isn't very good at

- While AI is very good at writing your code, it's still pretty shaky at getting the environments setup. So if you're starting from a blank slate, it can be a little rocky
- You really want your project setup so that it's always running code analysis, linting, and unit testing before every commit. This means setting up
  - Python - [pre-commit](https://pre-commit.com/), [Black](https://pypi.org/project/black/), [Ruff](https://github.com/astral-sh/ruff), [Mypy](https://mypy-lang.org/), [pytest](https://docs.pytest.org/en/stable/)
  - React/Javascript/Typescript - [biome](https://biomejs.dev/), [typescript](https://www.typescriptlang.org/), [lefthook](https://github.com/evilmartians/lefthook), [zod](https://zod.dev/), [vitest](https://vitest.dev/)
  - C# - [format](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-format), [build](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-build), [test](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-test?tabs=dotnet-test-with-vstest) [Nunit](https://nunit.org/), [Coverlet](https://github.com/coverlet-coverage/coverlet)/[dotCover](https://www.jetbrains.com/dotcover/)

Repos I have already setup:
- https://github.com/cvanvlack/csharp_backend_template
- https://github.com/cvanvlack/python_fast_api_template
- https://github.com/cvanvlack/react_front_end_template

# TLDR of those articles
- Start with ChatGPT and use the smartest model available to you. The order is
  - o1 pro mode([$200 USD Subscription required](https://openai.com/chatgpt/pricing/))
  - o3 ([$20 USD Subscription required](https://openai.com/chatgpt/pricing/)). Limited to 100 messages a week.
  - o4-mini or o4-mini-high ([$20 USD Subscription required](https://openai.com/chatgpt/pricing/)) Limited to 300 messages a day with o4-mini, and 100 messages a day with o4-mini-high.
- Chat with one of those super smart models to hone your idea with this prompt ([example](https://chatgpt.com/share/682d12ed-0674-8006-9fa6-e1b3f54f1e1e))
```
Ask me one question at a time so we can develop a thorough, step-by-step spec for this idea. Each question should build on my previous answers, and our end goal is to have a detailed specification I can hand off to a developer. Let’s do this iteratively and dig into every relevant detail. Remember, only one question at a time.

Here’s the idea:

<IDEA>
```

If you think it's starting to get too deep in the weeds, or when things start to wrap up, finish with
```
Now that we’ve wrapped up the brainstorming process, can you compile our findings into a comprehensive, developer-ready specification? Include all relevant requirements, architecture choices, data handling details, error handling strategies, and a testing plan so a developer can immediately begin implementation.
```
- Open a new context window, and paste in the following
```
Draft a detailed, step-by-step blueprint for building this project. Then, once you have a solid plan, break it down into small, iterative chunks that build on each other. Look at these chunks and then go another round to break it into small steps. Review the results and make sure that the steps are small enough to be implemented safely with strong testing, but big enough to move the project forward. Iterate until you feel that the steps are right sized for this project.

From here you should have the foundation to provide a series of prompts for a code-generation LLM that will implement each step in a test-driven manner. Prioritize best practices, incremental progress, and early testing, ensuring no big jumps in complexity at any stage. Make sure that each prompt builds on the previous prompts, and ends with wiring things together. There should be no hanging or orphaned code that isn't integrated into a previous step.

Make sure and separate each prompt section. Use markdown. Each prompt should be tagged as text using code tags. The goal is to output prompts, but context, etc is important as well.

<SPEC>
```
After that, add
```
Can you make a `todo.md` that I can use as a checklist? Be thorough.
```
- Drop `spec.md`, `prompt-plan.md` and `todo.md` into the root of your git repo. Ideally a Github repo because `claude` knows how to interact with Github via the `gh` tool to interact with issues.
- Start with one of the project templates where all of the tooling has been setup. Specifically:
  - Repomix configs: `repomix.config.json`
  - Claude Code Prompts: `.claude\commands`
  - Tooling for linting, code formating, static code analysis, building and running tests
  - Precommit hooks for calling all of the above
- Start `claude` and feed in the `/project:do-todo` command. Add any additional information you need to tell it how to migrate from the current template to where you want it to go.

# Super Simple Mistakes People Frequently Make

- Not deeply understanding the context window. I reset the context window any time my topic changes even slightly. This is true in chatGPT, Claude Code or Cursor.
- While it can get you really far, [AI coding is still not a panacea](https://albertofortin.com/writing/coding-with-ai)
<blockquote>
One morning, I decide to actually inspect closely what’s all this code that Cursor has been writing. It’s not like I was blindly prompting without looking at the end result, but I was optimizing for speed and I hadn’t actually sat down just to review the code. I was just building building building.  

So I do a “coding review” session. And the horror ensues.  

Two service files, in the same directory, with similar names, clearly doing a very similar thing. But the method names are different. The props are not consistent. One is called "WebAPIprovider", the other one "webApi". They represent the same exact parameter. The same method is redeclared multiple times across different files. The same config file is being called in different ways and retrieved with different methods. 

No consistency, no overarching plan. It’s like I'd asked 10 junior-mid developers to work on this codebase, with no Git access, locking them in a room without seeing what the other 9 were doing.  
</blockquote>


# Tricky Details that will stop you
 - Claude Code works with Mac, Linux, and Windows Subsystem for Linux (`wsl`). Not Windows Powershell. But `dotnet` does not come installed by default in `wsl`. You need to install it manually. Check to make sure you `wsl` is `Ubuntu`
 ```
 $ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy
```
Then install `dotnet 8`
```
sudo apt-get update && sudo apt-get install -y dotnet8
```
This is what will let Claude Code actually run the commands to make sure everything is working. Good Reference:[Building .NET apps on WSL | .NET Conf 2022](https://www.youtube.com/watch?v=zJ0XGIDpQr8).
- Claude wants to run in `wsl`. If you are using Cursor, Visual Studio, VS, etc... it will use your Windows environment instead. This can cause the precommit hooks to work fine from `claude` if they are set assuming a unix environment, but fail in VS Code because some of the scripting doesn't work in powershell.