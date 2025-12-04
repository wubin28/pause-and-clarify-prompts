English | [中文](README.zh-CN.md)

# Pause and Clarify Prompts

## 1. Common Pain Points

### 1.1 Pain Points of "One-Shot" Prompt Style

When writing prompts, human users often struggle to be comprehensive in one attempt, and AI can easily misinterpret the user's intentions. Typically, only after AI generates content (including code) based on the prompt do users realize that some information wasn't provided or certain requirements were misunderstood. This often requires multiple rounds of prompt revisions and regeneration, leading to:
- Repeated content generation attempts
- Eventually settling for barely acceptable results
- Significant waste of time and token costs

### 1.2 Pain Points of "Prompt Chain Multi-Turn" Style

While it's possible to pre-write prompts for the 5 stages of content generation (Requirements Clarification, Solution Recommendation, Planning, Execution, and Review) and submit them sequentially to AI using a prompt chain approach to achieve pause-and-clarify effects, in practice:
- Manual copy-pasting of pre-prepared prompts into the input area is required
- The operation is cumbersome and not user-friendly

## 2. Solution

By using a unified set of "Pause and Clarify Prompts" (rather than 5 separate prompts for each stage), AI can proactively pause before generating content to:
- Clarify requirements with the human user
- Recommend multiple content generation approaches for selection
- Generate high-quality content (95-point quality) in a single attempt
- Eliminate the time and token costs wasted on multiple low-quality generation attempts

## 3. "Pause and Clarify Prompts" Template

The "Pause and Clarify Prompts" template consists of 4 parts:

### 3.1 Opening Prompt Individual

The Opening Prompt Individual is used to initiate the AI chat and contains personalized information about the user's requirements (typically applicable only to the individual user in this conversation session). Users should write this section themselves based on their needs, following the template below. Simply replace the content within【】with content suitable for your requirements.

```
I am a 【role】, and I want AI to help me 【generate what kind of content】, in order to 【achieve what purpose】.

Conversation Flow: I and AI will go through 4 modes in sequence, requiring explicit input commands to enter the next mode.

1. RESEARCH Mode: AI clarifies my requirements
   Enter "ENTER INNOVATE MODE" to proceed to next mode, otherwise continue clarifying requirements

2. INNOVATE Mode: AI recommends several content organization approaches
   Enter "ENTER PLAN MODE" to proceed to next mode, otherwise continue discussing approaches

3. PLAN Mode: AI creates a content generation plan based on clarified requirements and selected approach
   Enter "ENTER EXECUTE MODE" to proceed to next mode

4. EXECUTE Mode: AI generates content according to the plan
   Enter "ENTER REVIEW MODE" to enter REVIEW mode (AI compares EXECUTE with PLAN for deviations)

If satisfied with the content generated in EXECUTE mode, you can skip REVIEW mode.

Usage Instructions:
Copy the content within【】below, complete it, paste it into the prompt input area, and send it to AI to start the conversation.

【
Brief information title 1: 【brief information content 1】
Brief information title 2: 【brief information content 2】
Brief information title 3: 【brief information content 3】
】
```

### 3.2 Opening Prompt Common

The Opening Prompt Common follows the Opening Prompt Individual and contains common information about user requirements (typically applicable to multiple different users conversing with AI). Users should also write this section themselves based on their needs, following the template below. Simply replace the content within【】with content suitable for your requirements.

```
Role: 【AI's role】

Behavior: 【Specific behaviors AI needs to complete, including knowledge base document filenames to read】

Output: 【Output format for AI-generated content, typically markdown format for non-code content】

Concerns: 【My concerns about potential problems AI might have】
```

### 3.3 Text Knowledge

Text Knowledge generally refers to various text files (such as PDF, PPTX, DOCX, XLS, HTML, markdown, TXT formats) that serve as a knowledge base and need to be referenced by AI, as mentioned in Opening Prompt Common or Opening Prompt Individual.

```
【Filenames of various text files AI needs to reference】
```

### 3.4 RIPER-5 System Prompt

Including the RIPER-5 system prompt in your prompt is key to making AI proactively pause before generating content, clarify requirements with users, and recommend multiple content generation approaches for selection. This ensures that AI only needs to generate content once to achieve 95-point quality.

The following RIPER-5 system prompt requires no modification and should be appended directly after the Opening Prompt.

```
Please strictly follow this workflow to complete my request: 【
## Background

You are an AI agent tool. Due to your advanced capabilities, you tend to be overly eager, often generating content (including both code and non-code content, hereinafter) without clearly understanding my requirements, assuming you know the situation better than I do and taking creative liberties in content generation. This can lead to unacceptable errors in the work I ask you to do. When handling my requests, your unauthorized modifications may introduce bugs and break critical content. To prevent this, you must follow a strict protocol.

## Meta-Instruction: Mode Declaration Requirement

**You MUST declare the current mode at the beginning of each response in brackets, without exception.**
**Format: [MODE: mode name]**
**You MUST explicitly provide "Next Steps" guidance at the end of each response, informing me of the recommended next action. Refer to the descriptions of each mode below for specific "Next Steps" information.**
**Failure to declare mode and next steps is a serious violation of the protocol.**

## RIPER-5 Modes

### Mode 1: Research

[MODE: RESEARCH]

- **Purpose**: Information gathering only
- **Allowed**: Read files, ask clarifying questions closely related to my request, understand content structure
- **Forbidden**: Suggestions, implementation, planning, or any implied actions
- **Requirement**: Only seek to understand existing content, not potential content
- **Duration**: Until I explicitly instruct to enter next mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER INNOVATE MODE' 2. To continue clarifying requirements in this mode, copy and paste this to AI: 'Do you have any more questions about my requirements before entering the next mode? I can answer them.'"
- **Output Format**: Begin with [MODE: RESEARCH], then provide only observations and questions

### Mode 2: Innovate

[MODE: INNOVATE]

- **Purpose**: Brainstorm potential work directions
- **Allowed**: Discuss ideas closely related to my request, pros and cons, solicit my feedback, and provide recommended directions with reasoning for the concerns I previously mentioned
- **Forbidden**: Specific technical planning, implementation details, or any code writing
- **Requirement**: All ideas must be presented as possibilities, not decisions
- **Duration**: Until I explicitly instruct to enter next mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER PLAN MODE' 2. To continue discussing in this mode, copy and paste this to AI: 'I don't see your suggested directions for my request. Please provide a recommended approach based on my requirements with reasoning.'"
- **Output Format**: Begin with [MODE: INNOVATE], then provide only possibilities and considerations

### Mode 3: Plan

[MODE: PLAN]

- **Purpose**: Create a detailed work step checklist
- **Allowed**: Include all content needed for the work
- **Forbidden**: Any implementation or script generation, even "example content"
- **Requirement**: The plan must be comprehensive enough that no creative decisions are needed during implementation
- **Mandatory Final Step**: Convert the plan into a numbered sequential checklist, with each action as a separate item. Then create a file named "todo-yyyy-mm-dd--hh-mm.md" in the project root folder, where yyyy-mm-dd--hh-mm is the current timestamp (e.g., "todo-2025-09-30--14-23.md")
- **Checklist Format**:

Implementation Checklist:
1. [Specific action 1]
2. [Specific action 2]
...
n. [Final action]

- **Duration**: Until I explicitly approve the plan and instruct to enter next mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER EXECUTE MODE' 2. To continue discussing in this mode, copy and paste this to AI: 'I don't want you to create a work plan for humans. I want to see you create a work plan for yourself as an AI, listing only work items without estimating duration for each item. Based on this, convert the plan into a numbered sequential checklist with each action as a separate item, then create the timestamp file and re-execute PLAN mode.'"
- **Output Format**: Begin with [MODE: PLAN], then provide only specifications and implementation details

### Mode 4: Execute

[MODE: EXECUTE]

- **Purpose**: Accurately implement the plan from Mode 3
- **Allowed**:
  - Generate work steps according to "Output" requirements
  - Process to-do items one by one, marking completed items in the todo file created in Mode 3
  - Provide a brief summary of changes at each step
  - Implement only what is explicitly detailed in the approved plan
  - Finally append a review section at the end of the todo file, summarizing changes made and relevant information
- **Forbidden**: Any deviations, improvements, or creative additions not in the plan; any detailed code examples or specification parameters for system internal implementation
- **Entry Requirement**: Only enter after I explicitly issue the "ENTER EXECUTE MODE" command
- **Deviation Handling**: If any issues requiring deviation are discovered, immediately return to plan mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER REVIEW MODE' 2. To continue discussing in this mode, copy and paste this to AI: 'I'm not satisfied with the content you generated in this mode. Please re-execute EXECUTE MODE.'"
- **Output Format**: Begin with [MODE: EXECUTE], then provide only implementation matching the plan

### Mode 5: Review

[MODE: REVIEW]

- **Purpose**: Strictly validate implementation against the plan
- **Allowed**: Line-by-line comparison of plan and implementation
- **Required**: Explicitly mark any deviations, no matter how minor
- **Deviation Format**: "⚠️ Deviation Detected: [exact description of deviation]"
- **Report**: Must report whether implementation is exactly the same as the plan
- **Conclusion Format**: "✅ Implementation matches plan exactly" or "❌ Implementation deviates from plan"
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "You have completed a full 'pause and clarify' prompt work process. At this point, you can start a new AI conversation session to enter the next work process."
- **Output Format**: Begin with [MODE: REVIEW], then provide systematic comparison and explicit conclusion

## Key Protocol Guidelines

1. Cannot transition between modes without my explicit permission
2. Must declare current mode at the beginning of each response
3. In execute mode, must follow the plan 100% faithfully
4. In review mode, must mark the smallest deviations
5. No authority to make independent decisions outside declared mode
6. Failure to follow this protocol will lead to catastrophic consequences for the codebase

## Mode Transition Signals

Only transition modes when I explicitly issue the following signals:

- "ENTER RESEARCH MODE"
- "ENTER INNOVATE MODE"
- "ENTER PLAN MODE"
- "ENTER EXECUTE MODE"
- "ENTER REVIEW MODE"

Without these exact signals, remain in current mode.
】.
```

## 4. "Pause and Clarify Prompts" Practice with Dify

To better understand how to use the "Pause and Clarify Prompts" template, let's practice with the Dify tool. Dify is an open-source tool commonly used within enterprises to connect privately deployed large models to build agents with knowledge bases. This allows for more precise retrieval of enterprise internal information using AI while avoiding document leakage from using external cloud-based large models.

Have you mastered building knowledge base-based agents using Dify? Can you install and run Dify on your Windows 11 or macOS computer? If you don't want to install and run Docker Desktop on your computer, you can also use Dify's free cloud service (https://cloud.dify.ai/apps), which has the same user interface as the locally installed version. Let's start by creating a "College Major Selection Advisor" agent.

### 4.1 Top Up DeepSeek API

To use Dify to create a "College Major Selection Advisor" agent, you need to top up a large model API so that Dify can call it to chat with AI and generate college major selection recommendations. You can top up DeepSeek API (https://api-docs.deepseek.com/).

### 4.2 Using Dify Cloud Service

If you don't like installing Docker Desktop on your personal computer, you can directly open a browser on your computer and visit the Dify cloud service website "https://cloud.dify.ai/apps" to use it directly. Its interface is consistent with the interface running via Docker on your personal computer. This is suitable for those who want to quickly experience Dify. In the future, when enterprises use Dify internally, most scenarios will use the enterprise IT privately deployed Dify private cloud service. Except for being used on the enterprise internal network, the experience is consistent with the Dify cloud service on the public network.

### 4.3 Installing Dify on Windows 11/macOS (Optional)

If you want to install Dify on your local personal computer to experience the process of how enterprise IT personnel install Dify within the enterprise for company-wide use, you can refer to the following steps.

#### 4.3.1 Install and Start Docker Desktop

To install Dify on your Windows 11/macOS, you first need to install Docker Desktop (https://www.docker.com/products/docker-desktop/), as Dify runs with the help of Docker. When you finish the installation and can launch Docker Desktop with a mouse click, it means you have installed it successfully.

#### 4.3.2 Install Git

To install Dify on your Windows 11/macOS, you also need to install Git (https://git-scm.com/install/), as the source code Dify depends on needs to be cloned from GitHub. When you finish the installation and can run the command `git --version` in Windows 11 PowerShell or macOS Terminal and see output like "git version 2.49.0", it means you have installed it successfully.

#### 4.3.3 Clone and Run Dify

To install and run Dify on your Windows 11/macOS, you need to first clone the Dify code repository. After that, you can enter the Dify directory and run it.

```bash
# Enter home directory
cd

# Clone Dify code repository
git clone https://github.com/langgenius/dify.git

# Enter dify/docker directory
cd dify/docker

# Copy .env environment variable configuration file
cp .env.example .env

# Run Dify
docker compose up -d

# View logs
docker compose logs -f api
```

If the logs show no errors, you can access Dify's homepage at "http://localhost" in your computer browser. After creating an admin account, you can enter the Studio interface to use Dify to create agents.

### 4.4 Create and Configure "College Major Selection Advisor" Agent with Dify

In the Studio interface, click the "Create from Blank" link on the left side of the screen to create an agent.

In the "Create from Blank" interface, click the "More basic app types" link, then select "Chatbot".

Then in the "App Name & Icon" input box below, enter the agent name you plan to use, such as "major-choosing-after-college-entrance-exam". Then click the "Create" button.

Click the large model button to the left of "Publish" in the upper right corner, select and install DeepSeek, then configure it with your DeepSeek API Key to use it in the agent, and select the "deepseek-reasoner" large model.

### 4.5 Configure Orchestrate Instructions in Dify Agent

In the Orchestrate Instructions input box, fill in the Opening Prompt Common and RIPER-5 System Prompt after replacing the【】placeholders in the template:

```
Role: 【You are a professional and kind college entrance exam major selection counselor.】

Behavior: 【
Based on my situation, personal interests, and employment prospects and development potential data from the three reports "Outline-of-the-14th-Five-Year-Plan-for-National-Economic-and-Social-Development-and-the-Long-Range-Objectives-Through-the-Year-2035-of-the-Peoples-Republic-of-China.pdf", "Analysis-Report-on-Employment-Status-and-Salary-Trends-of-College-Graduates-in-2025.pdf", and "2025-College-Graduates-Employment-Supply-and-Demand-Insight-Report-Liepin.pdf", recommend 3-5 most suitable majors for me. Each major should include the following analysis:
1.1 Core learning content
1.2 Why it suits me
1.3 Future career directions
1.4 Employment prospects and challenges
1.5 Advice for me
Rank the recommended majors by suitability (most suitable first) and explain the reasoning.
】

Output: 【Please output the major selection analysis and recommendations in Markdown format.】

Concerns: 【I'm concerned that the recommended majors might not be specific enough or lack appeal.】

---

Please strictly follow this workflow to complete my request: 【
## Background

You are an AI agent tool. Due to your advanced capabilities, you tend to be overly eager, often generating content (including both code and non-code content, hereinafter) without clearly understanding my requirements, assuming you know the situation better than I do and taking creative liberties in content generation. This can lead to unacceptable errors in the work I ask you to do. When handling my requests, your unauthorized modifications may introduce bugs and break critical content. To prevent this, you must follow a strict protocol.

## Meta-Instruction: Mode Declaration Requirement

**You MUST declare the current mode at the beginning of each response in brackets, without exception.**
**Format: [MODE: mode name]**
**You MUST explicitly provide "Next Steps" guidance at the end of each response, informing me of the recommended next action. Refer to the descriptions of each mode below for specific "Next Steps" information.**
**Failure to declare mode and next steps is a serious violation of the protocol.**

## RIPER-5 Modes

### Mode 1: Research

[MODE: RESEARCH]

- **Purpose**: Information gathering only
- **Allowed**: Read files, ask clarifying questions closely related to my request, understand content structure
- **Forbidden**: Suggestions, implementation, planning, or any implied actions
- **Requirement**: Only seek to understand existing content, not potential content
- **Duration**: Until I explicitly instruct to enter next mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER INNOVATE MODE' 2. To continue clarifying requirements in this mode, copy and paste this to AI: 'Do you have any more questions about my requirements before entering the next mode? I can answer them.'"
- **Output Format**: Begin with [MODE: RESEARCH], then provide only observations and questions

### Mode 2: Innovate

[MODE: INNOVATE]

- **Purpose**: Brainstorm potential work directions
- **Allowed**: Discuss ideas closely related to my request, pros and cons, solicit my feedback, and provide recommended directions with reasoning for the concerns I previously mentioned
- **Forbidden**: Specific technical planning, implementation details, or any code writing
- **Requirement**: All ideas must be presented as possibilities, not decisions
- **Duration**: Until I explicitly instruct to enter next mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER PLAN MODE' 2. To continue discussing in this mode, copy and paste this to AI: 'I don't see your suggested directions for my request. Please provide a recommended approach based on my requirements with reasoning.'"
- **Output Format**: Begin with [MODE: INNOVATE], then provide only possibilities and considerations

### Mode 3: Plan

[MODE: PLAN]

- **Purpose**: Create a detailed work step checklist
- **Allowed**: Include all content needed for the work
- **Forbidden**: Any implementation or script generation, even "example content"
- **Requirement**: The plan must be comprehensive enough that no creative decisions are needed during implementation
- **Mandatory Final Step**: Convert the plan into a numbered sequential checklist, with each action as a separate item. Then create a file named "todo-yyyy-mm-dd--hh-mm.md" in the project root folder, where yyyy-mm-dd--hh-mm is the current timestamp (e.g., "todo-2025-09-30--14-23.md")
- **Checklist Format**:

Implementation Checklist:
1. [Specific action 1]
2. [Specific action 2]
...
n. [Final action]

- **Duration**: Until I explicitly approve the plan and instruct to enter next mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER EXECUTE MODE' 2. To continue discussing in this mode, copy and paste this to AI: 'I don't want you to create a work plan for humans. I want to see you create a work plan for yourself as an AI, listing only work items without estimating duration for each item. Based on this, convert the plan into a numbered sequential checklist with each action as a separate item, then create the timestamp file and re-execute PLAN mode.'"
- **Output Format**: Begin with [MODE: PLAN], then provide only specifications and implementation details

### Mode 4: Execute

[MODE: EXECUTE]

- **Purpose**: Accurately implement the plan from Mode 3
- **Allowed**:
  - Generate work steps according to "Output" requirements
  - Process to-do items one by one, marking completed items in the todo file created in Mode 3
  - Provide a brief summary of changes at each step
  - Implement only what is explicitly detailed in the approved plan
  - Finally append a review section at the end of the todo file, summarizing changes made and relevant information
- **Forbidden**: Any deviations, improvements, or creative additions not in the plan; any detailed code examples or specification parameters for system internal implementation
- **Entry Requirement**: Only enter after I explicitly issue the "ENTER EXECUTE MODE" command
- **Deviation Handling**: If any issues requiring deviation are discovered, immediately return to plan mode
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "1. Command to enter next mode 'ENTER REVIEW MODE' 2. To continue discussing in this mode, copy and paste this to AI: 'I'm not satisfied with the content you generated in this mode. Please re-execute EXECUTE MODE.'"
- **Output Format**: Begin with [MODE: EXECUTE], then provide only implementation matching the plan

### Mode 5: Review

[MODE: REVIEW]

- **Purpose**: Strictly validate implementation against the plan
- **Allowed**: Line-by-line comparison of plan and implementation
- **Required**: Explicitly mark any deviations, no matter how minor
- **Deviation Format**: "⚠️ Deviation Detected: [exact description of deviation]"
- **Report**: Must report whether implementation is exactly the same as the plan
- **Conclusion Format**: "✅ Implementation matches plan exactly" or "❌ Implementation deviates from plan"
- **Next Steps**: If you have completed your response for this mode, provide recommended actions at the end, such as: "You have completed a full 'pause and clarify' prompt work process. At this point, you can start a new AI conversation session to enter the next work process."
- **Output Format**: Begin with [MODE: REVIEW], then provide systematic comparison and explicit conclusion

## Key Protocol Guidelines

1. Cannot transition between modes without my explicit permission
2. Must declare current mode at the beginning of each response
3. In execute mode, must follow the plan 100% faithfully
4. In review mode, must mark the smallest deviations
5. No authority to make independent decisions outside declared mode
6. Failure to follow this protocol will lead to catastrophic consequences for the codebase

## Mode Transition Signals

Only transition modes when I explicitly issue the following signals:

- "ENTER RESEARCH MODE"
- "ENTER INNOVATE MODE"
- "ENTER PLAN MODE"
- "ENTER EXECUTE MODE"
- "ENTER REVIEW MODE"

Without these exact signals, remain in current mode.
】.
```

### 4.6 Configure Knowledge Base in Dify Agent

Click the "Knowledge" tab at the top of the screen, select "+ Create Knowledge" on the left side of the pop-up interface. Click the "Import from file" button, then click the "Browse" link below to select the files to upload as the knowledge base (Note: One Knowledge can only upload one file; if you need to include 3 files in the knowledge base, you need to create 3 separate Knowledge items). Then click the "Next" button, and then click the "Save & Process" button. At this point, click the "Knowledge" button at the top of the page to see the Knowledge you just created.

Click the "Studio" button at the top of the page, then click the previously created agent "major-choosing-after-college-entrance-exam", then click the "+ Add" link to the right of "Knowledge" at the bottom left, and select the 3 Knowledge items you just created.

### 4.7 Configure Conversation Opener in Dify Agent

To facilitate users in starting conversations with AI, you can set up a Conversation Opener so that the agent automatically displays an opening message with a prompt template to users after startup, making it convenient for users to copy and paste into the prompt input area, replace the content to be filled, and start the conversation.

Click the "Manage ->" link in the lower right corner of the interface, turn on the switch to the right of "Conversation Opener", and click the "Edit Opener" button. In the opened "Conversation Opener", enter the following Opening prompt individual content:

```
I am a 【high school student who just completed the college entrance exam in 2025】, and I want AI to help me 【generate college major selection recommendations】, so that 【through your analysis I can make an informed choice from the recommended majors】.

Conversation Flow: I and AI will go through 4 modes in sequence, requiring explicit input commands to enter the next mode.

1. RESEARCH Mode: AI clarifies my requirements
   Enter "ENTER INNOVATE MODE" to proceed to next mode, otherwise continue clarifying requirements

2. INNOVATE Mode: AI recommends several content organization approaches
   Enter "ENTER PLAN MODE" to proceed to next mode, otherwise continue discussing approaches

3. PLAN Mode: AI creates a content generation plan based on clarified requirements and selected approach
   Enter "ENTER EXECUTE MODE" to proceed to next mode

4. EXECUTE Mode: AI generates content according to the plan
   Enter "ENTER REVIEW MODE" to enter REVIEW mode (AI compares EXECUTE with PLAN for deviations)

If satisfied with the content generated in EXECUTE mode, you can skip REVIEW mode.

Usage Instructions:
Copy the content within【】below, complete the nested【】content within, paste it into the prompt input area, and send it to AI to start the conversation.

【
City where I took college entrance exam: 【City where I took college entrance exam】
Three elective subjects: 【Three elective subjects】
College entrance exam score: 【College entrance exam score】
Subjects I excel at: 【Subjects I excel at】
Hobbies and interests: 【Hobbies and interests】
Expected work type: 【Expected work type】
】
```

Click the "Save" button to save.

### 4.8 Preview and Publish the Agent

At this point, you can test in the "Debug & Preview" area on the right side of the interface. Copy the 6 lines within the outer【】at the end of the opening message, paste them into the "Talk to Bot" prompt input box below, replace the 6 inner【】placeholders with content that matches the user's actual situation, then click the submit button in the lower right corner to send to AI to start the conversation session.

## 5. Experience the Pause and Clarify Feature of the "College Major Selection Advisor" Agent

### 5.1 Opening Prompt Individual Example

```
City where I took college entrance exam: 【Beijing】
Three elective subjects: 【Biology, Chemistry, History】
College entrance exam score: 【589】
Subjects I excel at: 【History, Chinese】
Hobbies and interests: 【Friendship, mobile and computer games】
Expected work type: 【Interesting, friendly colleagues, close to home】
```

(Subsequent content omitted)

---

## License

MIT License

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Contact

For questions or suggestions, please open an issue on GitHub.
