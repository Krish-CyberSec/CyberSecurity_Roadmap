# Cybersecurity Learning Journey

> A practical, structured cybersecurity learning journey focused on building strong foundations before specializing.

---

## About This Repository

This repository documents my cybersecurity learning journey from the fundamentals to practical security skills.

The goal is not to collect certificates, memorize definitions, or simply complete random courses.

The goal is to understand:

```text
How technology works
        ↓
How systems communicate
        ↓
How security controls protect them
        ↓
How attackers abuse weaknesses
        ↓
How defenders detect and respond
        ↓
How organizations manage cyber risk
```

I am approaching cybersecurity from both sides:

```text
Technical Security
        +
Governance, Risk & Compliance
```

This means I want to understand both:

* **How systems actually work**
* **Why organizations secure them in a particular way**

---

# Why This Repository Exists

Cybersecurity is a huge field.

There are many domains:

```text
SOC
Security Engineering
Cloud Security
Network Security
Penetration Testing
Incident Response
Digital Forensics
GRC
Security Architecture
Application Security
```

Instead of jumping directly into one specialization, this repository follows a **foundation-first approach**.

The idea is simple:

> Before learning how to secure something, understand how it works.

For example:

```text
Networking
   ↓
Operating Systems
   ↓
Identity & Access
   ↓
Security Controls
   ↓
Detection
   ↓
Incident Response
   ↓
Cloud Security
   ↓
GRC / Security Architecture
```

The exact order may evolve as the learning journey progresses, but every topic should connect to something previously learned.

---

# What I Am Trying to Build

This repository is intended to become more than a collection of notes.

It will gradually become a combination of:

```text
Learning Notes
      +
Interview Preparation
      +
Hands-on Labs
      +
TryHackMe Practice
      +
Technical Diagrams
      +
Security Architecture Thinking
      +
Real-World Scenarios
      +
Portfolio Evidence
```

The objective is to reach a point where I can explain:

> **What a technology does, how it works, how it can fail, how it can be attacked, how it can be defended, and how an organization should manage the associated risk.**

---

# Learning Philosophy

## 1. Understand Before Memorizing

I don't want to memorize:

```text
"/26 = 62 hosts"
```

without understanding why.

Instead:

```text
32 IPv4 bits
      ↓
Network bits
      ↓
Host bits
      ↓
Number of addresses
      ↓
Subnet size
```

The same principle applies to security concepts.

---

## 2. Connect Every New Topic

A new lesson should not exist in isolation.

For example:

```text
IP Address
    ↓
Subnetting
    ↓
Routing
    ↓
Ports
    ↓
TCP/UDP
    ↓
Services
    ↓
Firewalls
    ↓
SOC Investigation
```

The goal is to build a connected mental model rather than a collection of definitions.

---

## 3. Learn From Multiple Perspectives

Whenever possible, a topic will be viewed from different roles.

### SOC Perspective

```text
What would a defender see?

What logs would exist?

What traffic would be suspicious?

How would an alert be investigated?
```

### Security Engineering Perspective

```text
How would this be secured?

What controls would be implemented?

How would the architecture be designed?
```

### Red Team Perspective

```text
How could this technology be discovered?

How could it be misconfigured?

How might an authorized attacker interact with it?
```

### GRC Perspective

```text
What risk does this create?

What policy or control applies?

How would this be assessed?

What evidence would an auditor need?
```

This is especially important because cybersecurity is not only about tools.

---

# OSI Model Mapping

Networking topics will be continuously mapped to the OSI model.

The goal is to eventually understand how information moves through the stack rather than treating each protocol separately.

The working model is:

```text
Layer 7 — Application
     ↓
Layer 6 — Presentation
     ↓
Layer 5 — Session
     ↓
Layer 4 — Transport
     ↓
Layer 3 — Network
     ↓
Layer 2 — Data Link
     ↓
Layer 1 — Physical
```

Every relevant networking lesson will identify:

```text
Which OSI layer?
What does this layer do?
What protocols operate here?
What security concerns exist here?
```

Over time, this should produce a complete end-to-end understanding of network communication.

---

# Daily Learning Structure

The learning process is intended to be consistent rather than overwhelming.

Each lesson may contain:

```text
1. Concept
2. Explanation
3. Real-world analogy
4. Technical examples
5. Security perspective
6. GRC perspective
7. OSI mapping
8. Interview questions
9. Hands-on lab
10. TryHackMe practice
11. Diagrams
12. Checkpoint
```

Not every topic will require every component equally.

The purpose is to understand the topic first and document it second.

---

# TryHackMe Integration

Hands-on practice is an important part of this repository.

The plan is to pair learning with an appropriate **TryHackMe room or practical activity** whenever one exists.

The idea is:

```text
Learn the concept
      ↓
Apply it in a lab
      ↓
Practice in TryHackMe
      ↓
Document what was learned
      ↓
Connect it back to cybersecurity
```

TryHackMe activities should be relevant to the lesson rather than random rooms chosen simply for completion.

All practical security activity should remain within authorized environments.

---

# GitHub Documentation Structure

Each major lesson will follow a consistent structure.

```text
Lesson-XX-Topic/
│
├── README.md
├── InterviewQuestions.md
├── Labs.md
├── TryHackMe.md
│
└── Diagrams/
    ├── ...
    └── ...
```

### `README.md`

Contains the complete conceptual lesson.

It should explain:

* What the technology is
* Why it exists
* How it works
* Examples
* Security implications
* GRC relevance
* Connections to previous topics
* OSI mapping

---

### `InterviewQuestions.md`

Contains:

```text
Beginner questions
Intermediate questions
Scenario questions
Security questions
GRC questions
Interview challenges
```

The objective is understanding rather than memorizing prepared answers.

---

### `Labs.md`

Contains practical exercises.

Labs should gradually move from:

```text
Understand
   ↓
Observe
   ↓
Configure
   ↓
Troubleshoot
   ↓
Investigate
```

---

### `TryHackMe.md`

Contains the practical learning associated with the lesson:

```text
Room
Purpose
What to complete
Commands/concepts
Observations
Lessons learned
```

The goal is to document what was learned, not simply record that a room was completed.

---

### `Diagrams/`

Important concepts should be visualized when a diagram improves understanding.

Examples:

```text
Network flow
OSI mapping
Protocol behavior
Architecture
Attack surface
Security controls
Decision processes
```

---

# LinkedIn Strategy

LinkedIn posts will **not** be created for every lesson.

The purpose of LinkedIn is not:

> "I completed another lesson today."

Instead, posts will be created around meaningful milestones such as:

```text
Major concepts
Important networking milestones
Security insights
Completed projects
Interesting technical discoveries
Practical lessons worth teaching others
```

The goal is to publish something that another learner or security professional can actually learn from.

The repository will contain the detailed learning.

LinkedIn will contain selected insights and milestones.

---

# Learning Roadmap

The repository is intended to progress roughly from fundamentals toward practical cybersecurity.

A high-level direction is:

```text
FOUNDATIONS
    │
    ├── Networking
    ├── Operating Systems
    ├── Linux
    ├── Windows
    └── Basic Scripting
    │
    ▼
CORE SECURITY
    │
    ├── Authentication
    ├── Authorization
    ├── IAM
    ├── Cryptography
    ├── Security Controls
    └── Security Architecture
    │
    ▼
DEFENSIVE SECURITY
    │
    ├── Logging
    ├── SIEM
    ├── Detection
    ├── Threat Hunting
    ├── Incident Response
    └── Digital Forensics Fundamentals
    │
    ▼
TECHNICAL SECURITY
    │
    ├── Network Security
    ├── Web Security
    ├── Vulnerability Management
    ├── Security Testing
    ├── Active Directory Security
    └── Cloud Security
    │
    ▼
GOVERNANCE & RISK
    │
    ├── Risk Management
    ├── Security Frameworks
    ├── Compliance
    ├── Control Assessment
    ├── Audit
    └── Security Architecture
    │
    ▼
SPECIALIZATION
    │
    ├── SOC
    ├── Cloud Security
    ├── GRC
    ├── Security Engineering
    └── Red Team / Offensive Security
```

This is a direction rather than a rigid schedule.

Topics may be reordered when a prerequisite needs to be strengthened.

---

# Frameworks and Governance

The governance side of the journey will eventually include frameworks and standards such as:

```text
NIST RMF
NIST CSF
FIPS
NIST SP 800-53
ISO 27001
CIS Controls
SOC 2
MITRE ATT&CK
```

The purpose will not be to memorize framework names.

The goal is to understand:

```text
Risk
   ↓
Requirements
   ↓
Controls
   ↓
Implementation
   ↓
Assessment
   ↓
Monitoring
```

and how these frameworks fit into real organizations.

---

# Connecting Technical Security With GRC

One of the major goals of this repository is to avoid treating technical security and GRC as completely separate worlds.

For example:

```text
Technical:
"Port 3306 is reachable."

GRC:
"Why is the database exposed,
who approved it, and what control
restricts access?"
```

Or:

```text
Technical:
"MFA is implemented."

GRC:
"Is MFA required by policy,
is it properly enforced,
and is there evidence demonstrating compliance?"
```

The goal is to become comfortable asking both questions.

---

# Projects

As the learning progresses, practical projects will be added.

Projects should demonstrate:

```text
Understanding
    +
Implementation
    +
Security
    +
Documentation
    +
Risk Awareness
```

Whenever appropriate, projects should connect back to concepts learned earlier.

For example:

```text
Networking
   ↓
Network architecture
   ↓
Security controls
   ↓
Logging
   ↓
Detection
   ↓
Risk assessment
```

The purpose is to turn learning into portfolio evidence rather than leaving everything as theoretical notes.

---

# Interview Preparation

Interview preparation is built into the learning process.

Rather than studying interviews separately at the end, each topic will include questions such as:

```text
What is it?
Why does it exist?
How does it work?
What can go wrong?
How would you secure it?
How would you investigate it?
How would you explain it to management?
```

The target is the ability to **reason about a problem**, not recite definitions.

---

# Progress Tracking

Progress will be tracked through completed lessons, labs, TryHackMe practice, projects, and assessments.

A lesson should not be considered complete simply because the README exists.

The intended progression is:

```text
Learn
 ↓
Understand
 ↓
Explain
 ↓
Practice
 ↓
Solve
 ↓
Document
 ↓
Move Forward
```

---

# What Success Looks Like

The goal of this repository is not:

```text
"Complete 100 rooms."
```

or:

```text
"Collect 20 certificates."
```

The goal is to reach a point where I can take a real security problem and reason through it.

For example:

> A server is exposing an unexpected service.

I should be able to think:

```text
Which host?
      ↓
Which port?
      ↓
Which protocol?
      ↓
Which service?
      ↓
Which process?
      ↓
Who can reach it?
      ↓
Why is it exposed?
      ↓
Is it required?
      ↓
What risk does it create?
      ↓
What controls should exist?
      ↓
How would we monitor it?
```

That is the type of thinking this repository is designed to develop.

---

# Repository Principles

```text
Understand, don't memorize.

Connect topics instead of studying them in isolation.

Practice what is learned.

Use authorized environments for security testing.

Document mistakes as well as successes.

Ask "why?" before asking "what command?"

Think from both technical and governance perspectives.

Use diagrams when they make a concept clearer.

Publish meaningful insights, not activity updates.
```

---

# Final Goal

This repository is intended to become a visible record of my progression from:

```text
Cybersecurity Beginner
        ↓
Strong Technical Foundation
        ↓
Hands-on Security Skills
        ↓
Security Investigation
        ↓
Risk & Governance Understanding
        ↓
Practical Security Professional
```

The end goal is not simply to know more cybersecurity terminology.

It is to understand **how systems work, how they can fail, how they can be attacked, how they can be defended, and how organizations can manage the resulting risk.**

---

## Repository Status

This repository is a work in progress.

The structure, roadmap, and learning sequence may evolve as new concepts expose gaps in previous knowledge.

That is part of the process.

> **The goal is not to finish the repository. The goal is to become capable.**
