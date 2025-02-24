## Programming with Pixels:

## Computer-Use Meets Software Engineering

```
Pranjal Aggarwal^1 Sean Welleck^1
```
##### Abstract

```
Recent advancements in software engineering
(SWE) agents have largely followed a tool-
based paradigm, where agents interact with hand-
engineered tool APIs to perform specific tasks.
While effective for specialized tasks, these meth-
ods fundamentally lack generalization, as they
require predefined tools for each task and do not
scale across programming languages and domains.
We introduceProgramming with Pixels
(PwP), an agent environment that unifies soft-
ware development tasks by enablingcomputer-
use agents—agents that operate directly within
an IDE through visual perception, typing, and
clicking, rather than relying on predefined tool
APIs. To systematically evaluate these agents, we
proposePwP-Bench, a benchmark that unifies
existing SWE benchmarks spanning tasks across
multiple programming languages, modalities, and
domains under a task-agnostic state and action
space. Our experiments demonstrate that general-
purpose computer-use agents can approach or
even surpass specialized tool-based agents on a
variety of SWE tasks without the need for hand-
engineered tools. However, our analysis shows
that current models suffer from limited visual
grounding and fail to exploit many IDE tools that
could simplify their tasks. When agents can di-
rectly access IDE tools, without visual interaction,
they show significant performance improvements,
highlighting the untapped potential of leverag-
ing built-in IDE capabilities. Our results establish
PwPas a scalable testbed for building and evaluat-
ing the next wave of software engineering agents.^1
```
##### 1. Introduction

```
Human software developers possess a remarkable ability to
work across a wide range of programming tasks, seamlessly
adapting to new languages, tools, and problem domains.
```
(^1) Carnegie Mellon University. We release code and data at
programmingwithpixels.com.
Realizing a single, general-purpose agent with similar ver-
satility is the overarching goal of many recent efforts in
code generation and software engineering automation (Jiang
et al., 2024; Jin et al., 2024; Wang et al., 2024b). However,
most software engineering agents (SWE agents) still rely
on atool-based paradigm, where an agent takes actions
using hand-engineered functions (e.g., search repository,
run Python code) exposed through a text API (Yang et al.,
2024a;b; Wang et al., 2024a;b). This fundamentally limits
generalization, since tool-based agents can only perform
tasks using the predefined actions. For example, an agent
designed to manage GitHub pull requests lacks debugging
abilities unless it is programmed into the agent’s API. Fur-
thermore, the tool-based paradigm lacks scalability, as hand-
engineering complex tools requires significant human effort
and may not be bug-free. As a result, it remains unclear
whether the tool-based paradigm scales to the diversity of
software engineering tasks, which spans multiple languages,
modalities, and task types.
Our motivating hypothesis is that achieving general-purpose
SWE agents requires a shift tocomputer-use agents(An-
thropic, 2024) that interact with computers as humans do:
by observing the screen, typing, and clicking. To this end,
we recast agentic software engineering as interacting di-
rectly with anintegrated development environment (IDE)
by observing its visual state and using basic actions such
as clicking and typing. This allows the agent to perform
any task possible in an IDE and leverage all of the IDE’s
tools—from debuggers to web browsers—without requir-
ing specialized APIs. However, despite promising results in
web navigation (Anthropic, 2024) and open-ended computer
tasks (Xie et al., 2024), the ability of computer-use agents
to perform software engineering remains underexplored and
we lack a dedicated environment for software engineering.
To close this gap, we introduceProgramming with Pixels
(PwP), the first software engineering agent environment
aimed at general-purpose computer-use agents. ThePwP
environment is a VSCode-based IDE where agents perceive
the screen and use primitive actions such as typing, point-
ing, and clicking. PwPfulfills two key properties. First,
the environment isexpressive, allowing agents to complete
any software engineering task achievable in an IDE, with-

# arXiv:submit/6229626 [cs.LG] 24 Feb 2025


###### Languages:

## PwP: Computer-Use Paradigm

##### PwP Bench

```
Code Gen Pull Requests
Code Editing UI Generation
DevOps Data Science
```
### ⌨

## Tool-Based Paradigm "

```
Fix Bug in Python
Repository
```
```
General
Agent
```
```
BugFix
Agent
```
```
File Viewer
File Editor
Code Search
```
```
Tools
```
```
Create Login UI
design in HTML
```
```
UI
Agent
```
```
UI Preview
CSS Styler
Flex View
```
```
Tools
```
###### Modalities

#### Tools:

Figure 1: Comparison between the traditional tool-based paradigm (left) and our proposedProgramming with
Pixels(PwP) framework (right) for software engineering (SWE) agents. Instead of relying on specialized hand-engineered
tools,PwPenables agents to interact directly with an IDE through basic computer interactions and screen observations.
The framework naturally allows agents to take advantage of existing IDE capabilities across multiple languages and tools.
Furthermore,PwP-Benchis a comprehensive benchmark to evaluate agent performance across different SWE domains.

out language- or domain-specific modifications. Second,
agents naturally interact withany tools available in the IDE–
including debuggers, linters, and code suggestions while
handling diverse data types such as images, videos, and
PDFs–through basic actions such as clicking and typing.
This notion of tool use fundamentally differs from hand-
engineered tool APIs, offering scalability and reducing the
effort needed to hand-engineer tools for AI agents. Namely,
PwPlets agents take advantage of the rich tools already
accessible to humans in an IDE, rather than reinventing
the wheel. Finally, computer-use agents reduce the need for
complex tool pipelines (e.g., tool-specific prompts), opening
up a simplified approach to general-purpose SWE agents.

To further evaluate agents developed for computer-use we
constructPwP-Bench, a unified benchmark of 15 tasks
spanning a variety of software engineering activities, in-
cluding code generation, pull request resolution, UI devel-
opment, and DevOps workflows. We show for the first
time that general-purpose computer-use agents achieve non-
trivial performance on a wide variety of SWE tasks, often
approaching or exceeding state-of-the-art tool-based agents.

However, our analysis reveals substantial opportunities for
future work. First, even state-of-the-art computer-use agents
suffer from visual grounding issues. Second, we show that
current agents lack the ability to use many of the tools avail-
able in the IDE, including ones that could make their tasks
trivial. This suggests that training agents to explore and use
the tools available in the IDE is a fruitful future direction.

```
Finally, we find that only one model, Claude (Anthropic,
2024), performs well, highlighting the need for further re-
search into training and improving computer-use agents.
In summary, our contributions are as follows. First, we in-
troduceProgramming with Pixels(PwP), the first
software engineering-focused environment for evaluating
computer-use agents. Second, we introducePwP-Bench,
a benchmark spanning 15 diverse SWE domains, allowing
for systematic comparison of software engineering agents.
Third, we demonstrate, for the first time, that computer-use
agents can perform a wide variety of software engineering
tasks without additional hand-engineering of their action
or observation spaces. Fourth, we analyze the limitations
of current computer-use agents, identifying the need for
models that better leverage IDE tooling and highlighting
agent training as a key future direction. Finally, both ex-
isting agents and benchmarks can be easily incorporated
into our unified environment, positioningPwPto serve as a
common platform for developing future SWE agents. Over-
all,PwPchallenges the prevailing tool-based paradigm for
SWE agents and provides a platform for developing more
general agents that interact directly with IDEs.
```
##### 2. Related Work

```
2.1. Task-specific SWE benchmarks
```
```
Early neural code generation approaches were typically
evaluated on fixed input-output pairs—for example, gen-
```

erating code from docstrings (Chen et al., 2021) or from
general textual descriptions (Austin et al., 2021). Subse-
quent benchmarks extended these evaluations to interactive
settings, such as resolving GitHub pull requests or writing
unit tests for real-world code repositories (Jimenez et al.,
2023; Zan et al., 2024; M ̈undler et al., 2025). More re-
cently, efforts have broadened the scope of code generation
to include multimodal tasks, where vision models must in-
terpret images to generate correct code or edits (Si et al.,
2024; Shi et al., 2024; Jing et al., 2024; Yang et al., 2024b).
However, each of these benchmarks is confined to specific
languages, modalities, or task types. In contrast, our pro-
posedPwP-Benchunifies these diverse evaluations into a
single framework, encompassing multimodal and multilin-
gual challenges that require interaction with a broad suite
of IDE tools. Using this unified approach we reproduce the
performance of established benchmarks and encourage the
development of general-purpose agents capable of handling
a variety of new software engineering tasks. We further
compare our work with previous efforts in Tables 1 and 2.

2.2. Software Engineering (SWE) Agents

Recent work has explored “code agents” that move beyond
single-step neural code generation toward interactive meth-
ods, where intermediate feedback from tools informs subse-
quent actions. However, many of these approaches special-
ize in particular tools or programming languages (Jin et al.,
2024; Yang et al., 2024b), limiting their broader applicabil-
ity. For example, Agentless (Xia et al., 2024) relies on a
tool that parses files into Python-specific class and function
structures. This fails to perform well in other languages or
settings (Yang et al., 2024b) without manual modifications.
Similarly, the SWE-agent requires modifications to adapt to
different tasks (Abramovich et al., 2024; Yang et al., 2024b).
In contrast, agents designed forPwPare inherently task and
language-agnostic due to the expressive action and observa-
tion spaces mandated by our environment. Moreover, the
diverse tasks inPwP-Benchrequire agents to generalize
across a wide range of SWE challenges rather than excel in
one narrowly defined area such as resolving pull requests.

Many existing agents also depend on hand-engineered tools
that require human effort to implement and are susceptible
to bugs. For instance, Agentless (Xia et al., 2024) leverages
tools for parsing files into Python-specific structures; Code-
Act relies on an IPython kernel (Wang et al., 2024a); SWE-
Agent uses dedicated search and file editing tools (Yang
et al., 2024a); AutoCodeRover requires a linter (Zhang
et al., 2024); SWE-Agent EnIGMA develops specialized
tools for CTF-style competitions (Abramovich et al., 2024);
and SWE-Bench-MM (Yang et al., 2024b) implements a
browser view. InPwP, these tools are inherently available
within the IDE (as detailed in Table 7), and the agent’s task
is to effectively use them rather than being explicitly guided

```
on which tool to use for each specific task.
Finally, current approaches often blur the line between the
agent and the environment, as each agent is designed with
its own specified action and observation spaces within a self-
created environment. Programming with Pixels
addresses this issue by unifying existing environments into
a single, general-purpose platform on which agents operate.
This clear separation of environment design from agent de-
sign standardizes evaluation and also allows any existing
agent to be modeled within our framework, making it an
important testbed for both current and future SWE agents.
```
```
2.3. Visual Agents and Computer-Use Agents
A family of recent multimodal agent benchmarks require
agents to operate user interfaces using a predefined, lim-
ited set of actions (e.g., newtab,goback,click
[element id]) (Koh et al., 2024a; Deng et al., 2023;
Zheng et al., 2024). Thesevisual agentstypically rely on
additional prompting—such as set-of-marks techniques that
supply an HTML accessibility tree containing textual and
positional information—to overcome their inherent poor
visual grounding capabilities (Yang et al., 2023b). Despite
such aids, these agents often fail when faced with the com-
plex and dense IDE interfaces found in our environment.
A separate family ofcomputer-use agents(Anthropic, 2024;
OpenAI, 2025; Gou et al., 2024) are trained to operate with
an expressive action and observation space using primi-
tive operations like clicks and keystrokes, without the need
for external accessibility elements. However, there is no
SWE-specific environment for evaluating and further train-
ing these agents. PwPfills this gap by providing a uni-
fied, expressive IDE platform that challenges computer-use
agents with realistic and diverse SWE tasks.
```
```
2.4. Expressive Agent Environments
Prior work on expressive agent environments has predom-
inantly targeted the web domain (Koh et al., 2024a; Deng
et al., 2023), entire operating systems (Xie et al., 2024;
Bonatti et al., 2024; Rawles et al., 2023), or other general
scenarios (Xu et al., 2024). Some of these environments,
such as OSWorld (Xie et al., 2024), feature general action
and observation spaces similar to ours. However, although
these benchmarks are capable of expressing a wide range of
tasks, they do not focus on the unique challenges inherent
to software engineering within an IDE. For example, while
OSWorld offers a broad set of tasks, it is not specifically
designed for SWE, resulting in increased computational
overhead. Software engineering is a diverse and important
domain that merits its own dedicated environment.
Additionally, we designPwPso that existing tool-based soft-
ware engineering agents can be readily incorporated into our
```

framework. Specifically, we modify the source code of the
IDE to open up API calls that let us test current tool-based
agents. Furthermore,PwP-Benchis tailored specifically
for multimodal SWE tasks within an IDE, encompassing
activities such as pull-request handling, debugging, and
image-based code generation across multiple programming
languages. We also observe that existing agents built for
generic UI control often struggle in thePwPenvironment,
as they must interact with a richer set of tools and achieve
precise visual grounding within a complex interface con-
taining a large number of interactive elements. We further
distinguishPwPfrom other environments in Table 1.

##### 3. Programming with Pixels ( PwP )

Modern software engineering (SWE) requires using multi-
ple programming languages, tools, and modalities. Further-
more, it relies on a wealth of tools that required tremendous
human effort to create—from linters, to visual code de-
buggers, to project management tools. Motivated by these
observations, we createProgramming with Pixels,
an IDE environment that satisfies two properties: (i) it is
expressive, meaning that an agent can perform any task that
is achievable through a sequence of primitive operations
(e.g., typing or clicking) within an IDE; (ii) an agent has
access to any tool implemented within the IDE since using a
tool amounts to performing a sequence of primitive actions.

3.1. PwP environment

We represent thePwPenvironment as a partially observable
Markov decision process (POMDP). We define the PwP
POMDP⟨S, A, O, T, R⟩as:

- Sis the set of states describing the IDE and the operat-
    ing system context, including open files, active editor
    panels, and cursor positions.
- Ais the action space, encompassing all possible key-
    board and mouse events. The atomic actions in PwP
    are provided by thexdotoollibrary (Sissel), which
    allows specifying all possible keyboard and mouse
    events in a simple syntax. The specific action space
    varies based on the agent setting, described in (§5).
- Ois the observation space. The observation space
    varies based on the agent setting, described in (§5).
- Tis the transition function. Actions like inserting a
    character typically lead to deterministic changes in the
    IDE state, whereas background processes can introduce
    stochasticity in timing and responses.
- Ris the reward function that measures performance
    on a given task. For instance, after the agent finishes

```
editing code to fix a bug, the environment can run a
test suite on the updated files to compute a reward.
```
```
Trajectories inPwPcan thus resemble real-world develop-
ment work: an agent can fix a bug in a repository, use a
suggestion tool to help with writing code, or create docu-
mentation. The IDE and its operating system environment
track changes, run tests and return reward signals.
```
```
3.2. Key Features of Programming with Pixels
Expressive observation and action space. A typical ap-
proach to building agents is to engineer a set of high-level
actions for operations like “open file” or “list symbols in
file”, and then engineer an environment that supports each
action (Xia et al., 2024; Yang et al., 2024b; Wang et al.,
2024b). Furthermore, agents receive textual outputs that are
manually reformatted for each action. The key difficulty is
that such engineering does not scale to a large number of
actions or to the full range of software engineering tasks,
and the agent may be specialized to the observation and
action space. In contrast,PwPpreserves standard screen-
based user interaction. The agent can navigate IDE menus
visually, move the cursor, and press keys. This makes the
environment expressive: an agent can achieve any task that
can be achieved through primitive actions in an IDE.
```
```
Full Spectrum of Developer Tools. A modern IDE offers
debuggers, linters, version control, refactoring utilities, inte-
grated terminals, and many extensions. In particular, PwP is
developed on top of VSCode, which has a rich set of built-
in functionalities and extensions. Implementing each of
these into an agent or environment would require significant
human effort. However,PwPprovides these capabilities
out-of-the-box within a single environment. Agents can set
breakpoints, execute code, use language-specific extensions,
review error messages, or run tests in a consistent interface.
```
```
Multimodality and Language Agnosticism. Because the
IDE supports numerous programming languages through
extensions and built-in modules,PwPnaturally covers tasks
across Python, Java, JavaScript, Lean, and more without
requiring separate integrations. For instance, agents can
use the same debugger interface for all languages, or use
pre-existing linters provided by IDE extensions. Beyond
screenshots, the environment provides video streams and
audio, though we leave exploring these for future work.
```
```
Rich Feedback and State Access. PwPcan evaluate per-
formance immediately using testing frameworks or com-
pilation checks. For instance, if the agent modifies a file,
the IDE can automatically trigger a build, update diagnos-
tics, or run tests, generating real-time feedback. In cases
requiring deeper inspection—such as verifying that a bug is
```

Table 1:Comparison of **PwP** with existing environmentsacross different dimensions.PwPuniquely combines comprehen-
sive IDE tool support with full multimodal support, general action space, and execution-based evaluation, while maintaining
software engineering specificity.

```
Multi- General Observation State Tools Execution-Based SWE
Environment modal Action Space Space Checkpointing Reward Specific
GAIA (Mialon et al., 2023) ✗✗ Text ✗ Limited ✗✗
SWE-Bench (Jimenez et al., 2023) ✗✗ Text ✗ Limited ✓✓
SWE-Bench-MM (Yang et al., 2024b) Text, Image ✗ Text ✗ Limited ✓✓
OpenHands (Wang et al., 2024b) Text, Image ✗ Tool Output + Browser ✗ SWE ✓✓
TheAgentCompany (Xu et al., 2024) Text, Image ✗ Tool Output + Browser ✗ SWE ✓✗
WEBSHOP (Yao et al., 2023) ✗✗ Text ✗ Browser ✗✗
WEBARENA (Zhou et al., 2024) ✗✗ Text ✗ Browser ✓✗
VWEBARENA (Koh et al., 2024a) Text, Image ✓ Screen ✗ Browser ✓✗
BrowserGym (Chezelles et al., 2024) Text, Image ✓ Tool Output + Browser ✗ Browser ✓✗
OSWORLD (Xie et al., 2024) Text, Image ✓ Screen ✗ OS ✓✗
WindowsAgentArena (Bonatti et al., 2024) Text, Image ✓ Screen ✗ OS ✓✗
PwP(Ours) Text, Image, Video, Audio ✓ Screen ✓ All IDE Tools ✓✓
```
truly fixed—the environment can reveal file-system changes,
process states, or test results.

Future Adaptability. As agents continue to evolve,PwP
provides a unified environment for incorporating new bench-
marks and training. For example,PwPis amenable to re-
inforcement learning due to its use of the standard gymna-
sium (Towers et al., 2024) interface and its checkpointing
functionality. We show an example of how to interact with
PwPin Figure 2. Checkpointing also allows for backtrack-
ing in search-based methods (Koh et al., 2024b; Putta et al.,
2024). As software engineering practices progress and new
IDE tools emerge,PwPincorporates them without addi-
tional engineering overhead. Further, as agents become
more ubiquitous, it is imperative to evaluate their capabili-
ties in pair-programming scenarios with human developers.
PwPsupports concurrent user-agent interaction, potentially
enabling new kinds of pair programming or real-time col-
laboration studies. Finally, adding new tasks requires only
minimal modification to configuration and evaluation files.

3.3. Infrastructure and Implementation

PwPis deployed in a secure sandboxed environment. In
particular, we run a modified version of Visual Studio
Code (VSCode) and a minimal operating system inside
a Docker container, ensuring a secure and isolated envi-
ronment. We chose VSCode for its extensive language
support, rich ecosystem of extensions, widespread adoption
in the developer community, and open-source nature that
enables customization and modification of its core function-
ality. Each container instance maintains its own file system
and processes, preventing interference between experiments,
facilitates reproducibility, and ensuring parallelization of
evaluation. We further provide the ability to checkpoint the
environment state, which is especially useful for backtrack-
ing in search algorithms or while training RL agents.

The environment interfaces with VSCode through multiple

```
channels: 1.) A controller that manages Docker container
lifecycle and configuration, 2.) A port-forwarding system
for real-time screen and video capture, 3.) A modified
VSCode codebase that exposes DOM state information, and
4) The VSCode Extension API for accessing fine-grained
IDE state. This multi-channel approach enables both high-
level environment control and detailed state observation.
Screen capture is handled viaImageMagickfor static
screenshots andffmpegfor streaming video output. These
tools were selected for their low latency and ability to
handle various screen resolutions and color depths. For
actions, a lightweight controller executesxdotoolcom-
mands within the container, which in turn simulates key-
board and mouse events on the IDE. Agents can thus insert
code, open new files, or navigate menus using the same
actions that a human developer would.
As shown in 2, a Python API is provided for interaction,
following a style similar to common reinforcement learning
libraries such as gymnasium (Towers et al., 2024). The API
abstracts away the complexity of container management,
benchmark management, and handling observations and ac-
tions, allowing researchers to focus on agent development.
Users can query the environment for the latest screenshot,
issue anxdotoolcommand, and receive updated states
or rewards. The environment’s container configuration is
flexible, allowing for software installations, customizable
CPU/memory limits, and display settings (e.g., resolution).
This versatility is crucial for large-scale evaluation, espe-
cially when tasks vary in complexity and resource needs.
```
##### 4. PwP-Bench

```
We introducePwP-Bench, a benchmark comprising 15
diverse software engineering tasks that span 8 program-
ming languages and multiple modalities. Each task provides
agents with access to the complete suite of tools available
in thePwPenvironment. The purpose ofPwP-Benchis to
```

1 bench = PwPBench(dataset=’swebench’)
2 # Replace with any dataset from PwP-
Bench
3 dataset = bench.get_dataset()
4
5 # Set up environment and get initial
observation
6 env = bench.get_env(dataset[0])
7 observation: PIL.Image = env.
get_observation()[’screenshot’]
8
9 # Generate and execute action
10 action = agent.get_action(observation)
11 print(action)
12 # Output: xdotool mousemove 1000 1200
13 # click 1 && xdotool type ’hello world
’
14 observation, info = env.step(action)
15
16 env.render()
17
18 # Environment control
19 env.pause()
20 env.resume()
21
22 # Get reward and reset
23 is_success = env.get_reward()
24 env.reset()
25

```
Figure 2: Example demonstrating interaction with PwP envi-
ronment, including keyboard/mouse actions, checkpointing,
and state management. The code shows basic initialization,
action execution, environment control, and reward handling.
```
```
evaluate how well agents handle a broad range of software
engineering (SWE) activities, thereby testing the generality
of their code generation and SWE capabilities.
```
```
Tasks. PwP-Benchcontains 5400 instances covering 15
tasks, sourced from 13 existing code-generation datasets and
2 newly created by us. These tasks are designed to be repre-
sentative of the breadth of software engineering—including
tasks beyond conventional code generation to capture real-
world complexities—and can be expanded as models ex-
cel in newer tasks. We followed three guiding principles:
(1) tasks should require significant interaction with various
SWE tools, (2) each task should necessitate multiple steps
to complete, and (3) the overall benchmark should span
multiple programming languages and modalities. Based
on these principles, we collected diverse tasks and grouped
them into four categories:
```
- Code Generation and Editing:Evaluates the ability of
    agents to generate and edit code. This category includes
    datasets such as HumanEval for code completion, SWE-
    Bench (Jimenez et al., 2023) and SWE-Bench-Java (Zan

```
et al., 2024) for resolving pull requests, DSBench (Jing
et al., 2024) for data science tasks, and Res-Q (LaBash
et al., 2024) or CanITEdit (Cassano et al., 2024) for code
editing. Each dataset benefits from different tools. For ex-
ample, SWE-Bench can take advantage of debuggers and
linters, while DSBench may leverage an IPython kernel
and tools for analyzing large data files. Code editing tasks
can leverage refactoring utilities and repository searches,
covering varied input-output formats and end goals.
```
- Multimodal Code Synthesis: Involves creating code
    based on input images or other visual data. Examples
    include Design2Code (Si et al., 2024) for UI development,
    Chart2Mimic (Shi et al., 2024) for generating Python code
    from chart images, SWE-Bench-MM (Yang et al., 2024b)
    for multimodal code editing, and DSBench tasks that rely
    on images or PDF documents during data analysis.
- Domain-Specific Programming:Focuses on specialized
    fields such as ethical hacking (CTF) (Yang et al., 2023a)
    and interactive theorem proving (miniCTX) (Hu et al.,
    2024). These tasks demand significant interactivity with
    IDE components. For example, theorem proving requires
    continuously inspecting states via the IDE, while CTF
    tasks often involve analyzing images, running executables,
    or installing VSCode extensions (e.g., hexcode readers).
- IDE-Specific and General SWE Tasks:Recognizing
    that code generation is only one aspect of software en-
    gineering, we introduce two novel task sets to evaluate
    broader SWE skills. The first,IDE Configuration, eval-
    uates an agent’s ability to modify IDE settings—such as
    themes, extension installations, and preferences—that are
    critical for effective tool use in a complex environment.
    The second, which we termGeneral-SWE, targets non-
    code activities such as profiling, designing UI mockups,
    managing Kanban boards, and project refactoring. These
    tasks capture essential operational skills typically required
    by human developers but largely absent from conventional
    code generation benchmarks.

```
Figure 3 shows the distribution of tasks across all categories.
An agent that succeeds across these tasks demonstrates
strong potential for automating a wide range of software
engineering activities. For instance,PwP-Benchcovers
Python, Java, JavaScript, HTML, CSS, Bash, SQL, and
Lean, and requires agents to work with text, images, data
files, and other data types. Furthermore, effective interaction
with IDE tools is essential.
```
```
Benchmarking Design and Task Setup. Every task is
evaluated within thePwPenvironment. Unlike traditional
benchmarks that provide structured, well-formatted con-
text (for example, supplying all relevant schemas in text-to-
SQL),PwP-Benchpresents agents with an IDE containing
```

Table 2:Comparison of existing software engineering benchmarks.PwP-Benchprovides the largest dataset (
instances) and uniquely covers all aspects: multiple languages and modalities, real IDE interaction, interactive coding, and
both code generation and general software engineering tasks.

```
#Instances Multiple Multiple Real IDE Interactive Non-Code Code-Generation
Benchmark Languages Modalities Env Coding SWE Tasks SWE Tasks
SWE-Bench (Jimenez et al., 2023) 2K ✗✗✗✓✗✓
SWE-Bench-MM (Yang et al., 2024b) ≤ 1 K ✗✓✗✓✗✓
LiveCodeBench (Jain et al., 2024) ≤ 1 K ✗✗✗✓✗✓
Aider Polyglot (Aider, 2024) ≤ 1 K ✓✗✗✓✗✓
TheAgentCompany (Xu et al., 2024) ≤ 1 K ✗✓✗✓✓✗
VisualWebArena (Koh et al., 2024a) ≤ 1 K ✗✓✗✗✗✗
OSWORLD (Xie et al., 2024) ≤ 1 K ✗✓✓✗✓✗
WindowsAgentArena (Bonatti et al., 2024) ≤ 1 K ✗✓✓✗✓✗
PwP-Bench(Ours) 5.4K ✓✓✓✓✓✓
```
```
Multimodal
Code Gen
```
```
SWE
Tasks
```
```
Code Gen
& Editing
```
```
Domain
Speci:c
```
```
Canit-
Edit
```
```
Res-Q
```
```
SWE
Bench
```
```
Human
Eval
```
```
BIRD
```
```
MiniCTX
```
```
CTF
```
```
SWE
Bench
Java
SWT
Bench
```
```
VSCode General
SWE
```
```
DSBench
```
```
Chart
Mimic
```
```
Design
2Code
SWE
Bench-MM
```
Figure 3: Distribution of tasks in PWP-Bench across four
main categories: Code Generation and Editing, Multimodal
Code Synthesis, Domain-Specific Programming, and Gen-
eral SWE Tasks. The inner ring shows the main categories
while the outer ring shows specific datasets and tasks within
each category.

a codebase rich in information. Specifically, an agent is pro-
vided with an initial environment stateSiand an instruction
I. The agent’s goal is to update the codebase to satisfyI
and transition to a final stateSf. OnlySfis evaluated, us-
ing execution-based criteria (e.g., running unit tests). This
setup requires agents to autonomously discover and extract
relevant information from files, directories, and other re-
sources—mirroring the challenges faced in real-world soft-
ware development.

```
Many tasks inPwP-Benchrequire extensive multi-turn
interactions and can be time-consuming. To support large-
scale evaluations,PwPenables parallelized testing in a sand-
boxed environment, ensuring both security and reproducibil-
ity. Tasks can also be configured to restrict or partially allow
internet access based on experimental needs.
Furthermore, as software engineering tasks evolve with ad-
vancements in model capabilities, our benchmark is de-
signed to grow over time. New tasks can be incorporated
by creating simple setup scripts that define the IDE’s ini-
tial state and evaluation logic, ensuring thatPwP-Bench
remains modular and adaptable.
```
```
PwP-Bench -Lite. BecausePwP-Benchcontains more
than 5400 instances in total, running a full evaluation can
be computationally expensive. To address this, we also pro-
videPwP-Bench-Lite—a smaller subset of 300 instances,
made up of 20 random samples per task. This subset pre-
serves the overall difficulty and distribution while ensuring
equal representation for each task, thereby making rapid
experimentation more accessible.
```
##### 5. Agents in Programming with Pixels

```
The primary objective of Programming with Pixels is to
enable general-purpose SWE agents. We evaluate state-
of-the-art agents based on vision-language models in our
environment. Each agent operates in a turn-based manner,
receiving a screenshot each turn and returning an action
(keyboard or mouse action) to progress toward the goal.
This design is the same as used in previous works (Xie
et al., 2024; Koh et al., 2024a) and we refer them to as
computer-use agents. In practice, most vision-language
models struggle with raw image inputs. To mitigate this,
we incorporateSet-of-Marks (SoM)(Yang et al., 2023b), in
which the agent receives both the raw image and a parse of
available interface element (e.g., buttons, text fields). The
```

```
Table 3: Performance Evaluation of Different Agents onPwP-Benchby Task Categories
```
```
Inputs Outputs Model Code Generation Multimodal Domain-Specific General Overall
& Editing Code Generation Code Generation SWE Tasks Avg
```
```
Screenshot
+ SoM
```
```
Keyboard
+ Mouse
```
```
GPT-4o 0.8% 12.4% 1.7% 10.0% 5.3%
GPT-4o-mini 0.8% 3.7% 0.0% 2.5% 1.7%
Gemini-Flash 0.0% 4.3% 0.0% 0.0% 1.2%
Gemini-Pro 2.5% 5.7% 0.0% 7.5% 3.%
Claude-Sonnet 10.0% 8.2% 5% 20.0% 10.2%
```
```
Screenshot
+ SoM
+ Tool
Output
```
```
Keyboard
+ Mouse
+ Tool
Call
(File, Bash)
```
```
GPT-4o 37.0% 41.9% 28.3% 5.0% 32.3%
GPT-4o-mini 23.6% 17.5% 15.0% 5.0% 17.8%
Gemini-Flash 9.5% 11.7% 8.3% 2.5% 8.9%
Gemini-Pro 30.9% 16.7% 3.3% 5.0% 18.1%
Claude-Sonnet 52.1% 55.1% 43.3% 20.0% 46.8%
```
```
agent then interacts with element IDs instead of raw pixel
coordinates.
We evaluate two categories of computer-use agents. The first
category only outputs keyboard and mouse clicks. That is,
the agent uses the following observation and action space:
```
- O: a screenshot and set-of-marks annotations.
- A: keyboard and mouse clicks with set-of-marks.

The second category of computer-use agents has access
to file and bash commands supplied by the environment
through an API. Furthermore, a screenshot is received only
when the screenshot action is called, instead of receiving
it every turn. These actions are provided in PwP in a simi-
lar design principle as Anthropic computer-use (Anthropic,
2024) which consists of file operations such as ‘read file’,
‘create file’, and ‘string replace’. In summary, these agents
use the following observation and action space:

- O: a screenshot and set-of-marks annotations and text
    output from tools if used.
- A: keyboard, mouse, and file and bash operation actions.

```
Finally, in Analysis 6.2, we create a tool-based agent design
compatible withPwP. Specifically, we provide agents with
domain-specific API calls that represent high-level actions
(such as getting file structure) instead of UI interactions.
Each high-level action is implemented as a sequence of
low-level actions executed inPwP. This setting lets us test
current state-of-the-art SWE agent patterns (e.g., Wang et al.
(2024b)) within the PwP environment.
```
##### 6. Experiments

```
Experimental setup. We evaluate two categories of base-
line agents as described in Section 5. For each configura-
tion, we test five state-of-the-art vision-language models
```
```
(VLMs): Gemini-Flash-1.5, Gemini-Pro-1.5, GPT-4o, GPT-
4o-mini, and Claude-3.5 Sonnet. With the exception of
Claude—which is natively trained for UI interaction—the
remaining models are provided with SoM.
At each timestep, an agent receives an observation (with the
observation space determined by its category) and returns
an action. The complete history of observations and actions
is incorporated into the model’s context. For each task in-
stance, the maximum number of iterations is capped at 20
steps; if the agent either exhausts these steps or issues a stop
command, the environment’s final state is evaluated using
task-specific metrics (see Appendix B for full details). No-
tably, the agent design remains the same throughout all tasks.
Due to computational and budget constraints, we evaluate
onPwP-Bench-Lite, which has 300 task instances.
```
```
6.1. Results
```
```
Table 3 summarizes performance across different agent ar-
chitectures and base models over the four categories of
PwP-Bench. As seen in the top half of the table, when us-
ing only primitive keyboard and mouse actions most agents
have poor performance, with a maximum overall average of
10.2%. We attribute this poor performance primarily to lim-
ited visual grounding and an inability to interact effectively
with the IDE—particularly for file editing and tool usage
(see Section 6.2 for further analysis).
In contrast, when agents are granted access to file editing
and bash operations through API calls rather than relying
solely on UI interactions, we observe consistent improve-
ments across all categories, with maximum average accuracy
reaching 46.8%. Among the evaluated models, the Claude
computer-use agent performs best, likely because it is specif-
ically trained for UI interactions. We found that this agent
is able to leverage basic IDE tools—such as HTML live
preview, chart visualization, and file navigation—to boost
performance on tasks requiring visual understanding and
IDE navigation. Importantly, we find for the first time that a
```

Figure 4:Example of Successful Use of Live Preview Tool
in the UI Replication TaskThe agent successfully uses the
live preview tool in the VSCode browser to compare the UI
design it made versus the reference design.

```
Figure 5:Example of Successful Use of Tool in the Chart
Generation TaskThe agent can compare the generated chart
with the reference chart side by side and refine its code ac-
cordingly.
```
single computer-use agent can achieve performance compa-
rable to and often surpassing (See Appendix C) state-of-the-
art methods across a wide variety of software engineering
(SWE) tasks—encompassing multiple languages, modali-
ties, and domains—while operating within a single unified
environment and interface, and no specific hand-crafted
tools, unlike existing SoTA methods.

Nonetheless, as detailed in Section 6.2, the models struggle
to fully take advantage of the IDE tooling. This is evidenced
by the poor performance on the ‘General SWE’ dataset,
where tasks are often as simple as editing IDE settings and
often require fewer than four clicks to complete. As we show
in Section 6.2, these tasks become simpler if the models
could use the IDE tooling more effectively.

Overall, while the results point toward a promising direction
for developing general computer-use SWE agents, signifi-
cant improvements are still needed in visual grounding, tool
usage, and planning. We analyze these next.

6.2. Analysis

Agents Demonstrate Poor Visual Grounding Capabili-
ties. Our qualitative analysis across multiple VLMs on
PwP-Benchreveals significant limitations in visual under-
standing—even for basic IDE interactions. We identify two
primary failure modes. First, models frequently fail to cor-
rectly identify the UI elements intended for interaction, as
demonstrated in Figure 7, 8. In agents using set-of-marks
(SoM), this issue manifests as incorrect element selection,
while without SoM it leads to inaccurate mouse position-
ing. Second, models struggle to comprehend the current UI
state. As shown in Figure 9, 10, they consistently fail to
recognize highlighted elements, cannot detect linter errors
indicated by wavy underlines (Figure 11), and often confuse
active panels—resulting, for example, in typing into search
bars rather than file editors (Figure 9). While similar issues

```
have been documented in web and OS domains (Koh et al.,
2024a; Xie et al., 2024), these limitations were primarily
observed in models without UI-specific training. However,
our work, shows even models explicitly trained for UI inter-
action (Anthropic, 2024), including Claude-Computer Use,
exhibit these issues inPwP—likely due to the increased
complexity of the IDE interface.
Agents Fail to Edit Files.File editing is a basic capabil-
ity required in most SWE tasks. However, we find that
the deficiencies in visual grounding significantly impact
the file editing capabilities of current agents that use basic
actions (clicking and typing). For example, even when pro-
vided with cursor location information in textual form, these
models struggle to interpret such data amid complex UI
elements. Models fine-tuned for UI interactions still commit
basic editing errors—such as incorrect indentation and text
misplacement—and are unable to recover from these errors
(see Appendix for examples). We speculate these limitations
could stem from two factors: (i) model overfitting to user
interfaces in their training domains, or (ii) the increased
complexity of thePwPIDE interface, which contains sub-
stantially more interactable elements than typical web or
OS environments. Addressing these limitations represents
an important direction for future work. Although direct
file access via tool operations is available, UI-based editing
confers unique advantages for tasks such as editing Jupyter
notebooks, comparing changes, or modifying specific sec-
tions of large files. These results underscore two limitations:
(i) current VLMs are challenged by complex UI interactions
beyond simple web/OS interfaces (Xie et al., 2024; Koh
et al., 2024a), and (ii) the inability to effectively perform
UI-based editing prevents agents from leveraging valuable
IDE features that could have improved their performance.
```
```
Claude Computer-Use Agent Demonstrates Basic IDE
Tool Proficiency. Our analysis shows that the Claude
```

```
Step 1: The agent is given a repository, and
needs to 5x a bug
```
```
Step 2: The agent use VSCode 5le search tool,
that performs name match across repository.
```
```
Step 3: The agent searches for the term
“process” and reads "def process function".
```
```
Step 6: The agent edits the required line
with correct content, solving the task!
```
```
Step 4: The agent identi5es the line to edit,
and jumps to it, using VSCode tool.
```
```
Step 5: The agent reads the surrounding
code and selects the line to edit
```
Figure 6: Example of the Claude Computer-Use agent successfully using multiple IDE tools to complete a repository level
code-editing task.

Computer-Use agent successfully uses fundamental IDE
functionalities, including file explorer navigation, file edit-
ing, search, browser-based live preview, and image genera-
tion and visualization capabilities. Figure 4 demonstrates
the agent’s effective use of browser tools in UI replication
tasks. Similarly, Figure 6 illustrates the agent’s ability to
coordinate multiple tools while editing specific lines in a
repository, relying solely on screenshot observations and
primitive keyboard/mouse actions.

Second, we hypothesize that the agent has additional abili-
ties to use tools that can be uncovered through prompting or
fine-tuning. To investigate this, we considered the symbol
renaming task in our ‘General-SWE’ benchmark, in which
Claude initially achieves 0% accuracy when attempting the
task. However, when explicitly instructed to use the renam-
ing tool, its accuracy improves to 50% (see Appendix C).

Agents Struggle to Use IDE Functionality. Despite these
successes with basic IDE functionality, computer-use agents
demonstrate significant limitations when interfacing with
more complicated IDE tools. We observe no successful
instances of debugger usage or symbol listing operations.
Models without specific UI interaction training struggle
even with fundamental tools such as HTML live preview, im-
age visualization, and graph generation capabilities. While
Claude demonstrates competency with basic tools, it fails to
effectively use advanced tools like profilers or debuggers.

```
To further evaluate these capabilities, we developed the
‘General-SWE’ dataset, which focuses on software engi-
neering activities (e.g., profiling, refactoring, debugging)
that can be completed without direct code modification. Al-
though these tasks sometimes require only 4-5 steps when
using appropriate IDE tools, agents achieve minimal perfor-
mance, highlighting substantial room for improvement.
```
```
Training Models to Use IDE Tools Better Would Im-
prove Performance. While a single computer-use agent
design can perform well across a wide variety of tasks,
our results indicate that these models do not fully exploit
domain-specific tools. As an indication of the potential for
performance gainsifthe agent was able to effectively use
the IDE, we perform an “assisted” experiment.
For the assisted experiment, we manually engineer a set
of API calls that are useful for the tasks. For example, in
Design2Code, the assisted agents have an API call for a live
HTML preview, while for SweBench it has API calls for
retrieving repository structure and symbol outlines. Impor-
tantly, each API call is achievable using basic operations in
the IDE, meaning that in principle an agent could learn to
perform it. To ensure that each API call is achievable in the
IDE, we implement each API call by executing a fixed se-
quence of low-level IDE actions, with the details abstracted
away from the agent. A complete list of tools available in
different environments is in Table 7 (see Appendix C).
```

```
The input *eld for `python.linting.pylintArgs` now contains
"--disable=import-error”.
```
```
The search results for "python.linting.pylintArgs" are displayed.
```
```
Model Response
```
Figure 7:Example of Agent Hallucinating Screen Con-
tentsWhile the agent correctly mentions, search results are
displayed (green text), it hallucinates an input field contain-
ing “disable import error” (red)

```
Figure 8: Example of wrong mouse click by Claude-
Computer Use AgentThe agent attempted to click Settings
icon but clicked at the wrong location.
```
Figure 9:Example of Agent Misidentifying UI Elements
The agent fails to identify the correct input field, typing ‘50’
into the settings search bar instead of the word wrap column
setting field (red arrow).

```
Figure 10:Example of Agent Misidentifying Active Panel
The agent fails to recognize the active editor panel, incor-
rectly typing into the search bar (red arrow) instead of the
file editor.
```
Table 4: Comparison of Different Agent Types Across Se-
lected Tasks

SWE-Bench Design2Code Chartmimic BIRD (T2 SQL)
Primitive Agents 0% 23.5% 2.7% 0%
Computer Use 15% 48.1% 25.3% 7%
Assisted 19% 79.5% 61.6% 17%

Table 4 compares the performance of these assisted agents
with that of standard computer-use agents across four
datasets for which we manually created tools. The assisted
agents achieved up to a 13.3% improvement in average
scores relative to the non-assisted agents. This suggests that
training agents to explore and use the built-in IDE function-
ality would yield performance gains. It also suggests that in
the near term, we can get performance gains by introducing
hand-engineered tools into the computer-use agent and in-
corporating existing agent designs in ourPwPenvironment.

Agents Are Incapable of Recovering from Errors.Next,

```
we find that current agents show limited error recovery ca-
pabilities. When an action fails to execute correctly, models
tend to persistently repeat the same failed action without
exploring alternatives. Similarly, if an agent selects an in-
correct action, it continues along an erroneous solution path
without recognizing or correcting the mistake. In an ex-
periment designed to probe this behavior, we deliberately
suppressed one of the model’s actions. Despite the envi-
ronment’s screenshot clearly showing an unchanged state,
the models proceeded with their planned action sequence as
though the suppressed action had succeeded. This behavior
suggests a heavy reliance on memorized action sequences
rather than dynamic responses to visual feedback, resulting
in exponentially increasing errors and poor performance.
```
##### 7. Conclusion

```
In this work, we introducePwP, a unified environment that
challenges the prevailing tool-based paradigm by enabling
```

Figure 11:Example of Agent Missing Visual Error Indica-
torsThe agent fails to recognize linter error indicators (wavy
underlines).

```
Figure 12:Example of Agent’s Inability to Perform File
EditingThe agent incorrectly positions new content in the
file editor.
```
direct interaction with IDEs through basic computer-use ac-
tions like typing and clicking. This approach allows for
a wide range of tasks to be modeled without language-
or domain-specific modifications. Second, we introduce
PwP-Bench, a unification of existing SWE datasets eval-
uated inPwP. We find that general-purpose computer-use
agents can approach or outperform previous state-of-the-art
results, without any task-specific modifications. This sug-
gests that the dominant paradigm of building specialized
text-based tools for SWE agents may be superseded by end-
to-end computer-use agents. However, our analysis reveals
that state-of-the-art agents are still incapable of using the
extensive set of tools available inPwP, and could perform
better if they could use them. Our work opens up an excit-
ing new direction of development of computer-use agents
for SWE tasks, an important step towards reaching truly
general-purpose SWE agents. Finally,PwPprovides a com-
mon platform that supports existing agents and benchmarks,
making it a foundation for future research on SWE agents.