# Model Variants and Capabilities

Google released Gemma 4 in four distinct variants, each optimized for different use cases and hardware constraints. This section covers all four and helps you choose the right one.

## The Four Gemma 4 Variants

| Variant | Parameter Count | Best For | Hardware Needed |
|---------|----------------|----------|-----------------|
| **E2B** | ~2.6 billion | Lightweight tasks, quick answers | Any machine |
| **E4B** | ~4 billion | Balanced performance | Most laptops |
| **26B** | 26 billion | High-quality coding | Gaming/workstation |
| **32B** | 32 billion | Maximum capability | High-end desktop |

### Detailed Breakdown

#### 1. Gemma 4 E2B (2.6B Parameters)

The lightweight champion. Designed for speed and efficiency.

```text
RAM required:  2-4 GB
Speed:         Very fast (works on CPU)
Best for:      Quick code completion, simple Q&A,
               formatting, documentation generation
Quality:       Good for simple tasks
Limitation:    Struggles with complex reasoning
```

**Ideal Use Cases:**
- Quick code formatting and linting.
- Simple git commit message generation.
- Basic code explanations.
- Running on low-power devices (older laptops, Raspberry Pi).
- Always-on background assistant.

#### 2. Gemma 4 E4B (4B Parameters)

The balanced option. Good quality without requiring high-end hardware.

```text
RAM required:  4-6 GB
Speed:         Fast (CPU or basic GPU)
Best for:      Daily coding tasks, test writing,
               bug fixing, refactoring
Quality:       Good — handles most tasks well
Limitation:    May struggle with very complex architecture
```

**Ideal Use Cases:**
- Everyday coding assistance.
- Writing unit tests.
- Simple refactoring.
- Code review for individual files.
- Running on a standard laptop.

#### 3. Gemma 4 26B (26B Parameters)

The high-quality option for serious coding work.

```text
RAM required:  16-20 GB (with quantization)
Speed:         Moderate (needs GPU)
Best for:      Complex code generation, architecture design,
               debugging tricky issues
Quality:       Very good — competitive with GPT-4o-mini
Limitation:    Requires decent hardware
```

**Ideal Use Cases:**
- Writing complex algorithms.
- Designing application architecture.
- Debugging subtle bugs.
- Code review for entire codebases.
- Refactoring large code sections.

#### 4. Gemma 4 32B (32B Parameters)

The flagship model. Maximum capability.

```text
RAM required:  20-24 GB (with quantization)
Speed:         Moderate (needs good GPU)
Best for:      The hardest coding challenges
Quality:       Excellent — competes with Claude 3.5 Sonnet
Limitation:    High-end hardware required
```

**Ideal Use Cases:**
- Complex multi-file refactoring.
- Security audit and vulnerability analysis.
- Designing large systems.
- Translating code between languages.
- Generating comprehensive documentation.

## Capabilities Comparison

### Coding Benchmarks (Approximate)

| Capability | E2B | E4B | 26B | 32B |
|------------|:---:|:---:|:---:|:---:|
| Code Completion | Good | Good | Very Good | Excellent |
| Bug Fixing | Basic | Good | Very Good | Excellent |
| Refactoring | Limited | Good | Very Good | Excellent |
| Architecture Design | Poor | Basic | Good | Very Good |
| Test Writing | Basic | Good | Very Good | Excellent |
| Code Explanation | Good | Good | Very Good | Excellent |
| Multi-File Edits | Poor | Basic | Good | Very Good |
| Security Analysis | Poor | Basic | Good | Very Good |
| Documentation | Good | Good | Very Good | Excellent |

### Speed Comparison

```text
E2B (2.6B):  🚀🚀🚀🚀🚀  Very fast, even on CPU
E4B (4B):    🚀🚀🚀🚀    Fast, good balance
26B:         🚀🚀🚀      Moderate, needs GPU
32B:         🚀🚀        Slower but most capable
```

## Choosing the Right Variant

### Decision Flowchart

```text
What hardware do you have?
    │
    ├── Low-end laptop (4-8 GB RAM)
    │       └── Use E2B or E4B
    │
    ├── Standard laptop (8-16 GB RAM)
    │       ├── Use E4B for daily tasks
    │       └── Use 26B (quantized) for complex tasks
    │
    ├── Gaming desktop (16-32 GB RAM, GPU)
    │       ├── Use 26B for most tasks
    │       └── Use 32B for the hardest problems
    │
    └── High-end workstation (32+ GB RAM, good GPU)
            └── Use 32B for everything
```

### Task-Based Selection

```text
┌─────────────────────────────────────────────────────┐
│           TASK                    RECOMMENDED        │
├─────────────────────────────────────────────────────┤
│ Quick format / lint                  E2B             │
│ Write a simple function              E4B             │
│ Debug an error message               E4B             │
│ Refactor a class                     26B             │
│ Design system architecture           32B             │
│ Security audit                       32B             │
│ Generate documentation               E2B / E4B       │
│ Write unit tests                     E4B / 26B       │
│ Complex multi-file refactoring       32B             │
└─────────────────────────────────────────────────────┘
```

## Pros of Gemma 4 Compared to Other Models

| Versus | Gemma 4 Advantage |
|--------|-------------------|
| **GPT-4o** | Open weights, free to run locally, privacy |
| **Claude 3.5 Sonnet** | Open weights, no API costs, runs offline |
| **Llama 3** | Better at code (Google's Gemini lineage), more efficient |
| **DeepSeek Coder** | Google backing, regular updates, broader capabilities |
| **CodeLlama** | More modern architecture, better performance per parameter |

## What This Means for OpenCode

With different Gemma 4 variants, you can create a **tiered setup** in OpenCode:

```jsonc
{
  "provider": {
    "models": {
      "fast": {
        "name": "ollama",
        "model": "gemma4:e2b",
        "description": "Quick tasks"
      },
      "balanced": {
        "name": "ollama",
        "model": "gemma4:e4b",
        "description": "Daily coding"
      },
      "powerful": {
        "name": "ollama",
        "model": "gemma4:26b",
        "description": "Complex work"
      },
      "max": {
        "name": "openrouter",
        "model": "google/gemma-4-32b",
        "description": "Hardest problems"
      }
    }
  }
}
```

Then switch between them in OpenCode:

```text
> /model fast
> /model powerful
```

This gives you cost flexibility — use the lightweight variant for simple tasks, and the full 32B for complex problems.

## Summary

```text
E2B  (2.6B):  Lightweight, fast, any hardware — for simple tasks
E4B  (4B):    Balanced, good quality, most laptops — for daily use
26B  (26B):   High quality, needs GPU — for serious coding
32B  (32B):   Maximum power, needs good GPU — for hardest problems

All variants are free, open-weight, and work with OpenCode.
```
