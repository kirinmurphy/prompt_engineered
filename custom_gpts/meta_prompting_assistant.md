# Claude System Prompt Engineering Guide

## Introduction

This guide provides a comprehensive framework for creating effective system prompts for Claude. The recommendations are based on Anthropic's best practices and are designed to help you structure prompts that maximize Claude's capabilities.

## Key Principles

1. **Use natural language framing** to establish context conversationally
2. **Create purpose-driven tags** that clearly describe their content
3. **Only use tags where helpful** for separating different content types
4. **Be flexible** - add or remove sections based on specific needs
5. **Use consistent, descriptive tag names** that indicate their content
6. **Consider nesting tags** for hierarchical content when appropriate
7. **Combine XML structure with chain of thought** for complex reasoning tasks
8. **Prefill responses** to control output format and eliminate preambles

## Core Template

To prefill Claude's response and control its output format:

```
Human: [Your prompt with instructions]

Claude: <answer>
[This text appears as the beginning of Claude's response]
```

Benefits:

- Eliminates Claude's introductory preambles
- Ensures Claude begins its response in the exact format you specify
- Saves tokens by controlling output structure
- Can be combined with the core template structure

4. **Error prevention**: Use XML tags to prevent Claude from misinterpreting instructions as part of the input:

```
# Without tags (potential misinterpretation)
Rewrite this email to be more polite but don't change anything else:
Show up at 6am tomorrow because I'm the CEO and I say so.

# With tags (clear separation)
Rewrite this email to be more polite but don't change anything else:
<email>Show up at 6am tomorrow because I'm the CEO and I say so.</email>
```

## Common Pitfalls and Solutions

### 1. Misinterpreted Instructions

**Problem**: Claude treats instructions as part of the content to be processed.

**Example**:

```
Summarize this text and identify the main themes:
The novel explores the consequences of technological advancement...
```

**Solution**: Use XML tags to clearly separate instructions from content:

```
<instructions>
Summarize this text and identify the main themes:
</instructions>

<content>
The novel explores the consequences of technological advancement...
</content>
```

### 2. Inconsistent Tag Usage

**Problem**: Mixing different tag names for the same purpose causes confusion.

**Example**:

```
<text>
[First piece of content]
</text>

<content>
[Second piece of content]
</content>
```

**Solution**: Use consistent tag names throughout your prompts:

```
<content>
[First piece of content]
</content>

<content>
[Second piece of content]
</content>
```

### 3. Missing Closing Tags

**Problem**: Forgetting to close tags can cause Claude to misinterpret where sections end.

**Solution**: Always include both opening and closing tags, and double-check that they match exactly:

```
<content>
[Content goes here]
</content>
```

### 4. Overly Complex Nesting

**Problem**: Deeply nested tags can become difficult for Claude to process correctly.

**Example**:

```
<analysis>
  <section>
    <subsection>
      <paragraph>
        <sentence>
          [Content]
        </sentence>
      </paragraph>
    </subsection>
  </section>
</analysis>
```

**Solution**: Keep nesting to a reasonable depth (typically 3-4 levels maximum):

```
<analysis>
  <section_1>
    [Content]
  </section_1>
  <section_2>
    [Content]
  </section_2>
</analysis>
```

## Advanced Techniques

### 1. Multi-shot Examples with XML Structure

For tasks where you want Claude to learn from examples, structure both the examples and desired output format with XML tags:

```
You are a language translator converting English to French.

<examples>
  <example>
    <english>Hello, how are you today?</english>
    <french>Bonjour, comment allez-vous aujourd'hui?</french>
  </example>
  <example>
    <english>I would like to order dinner.</english>
    <french>Je voudrais commander le dîner.</french>
  </example>
</examples>

<input>
Where is the nearest train station?
</input>

<output_format>
Please provide the translation in this format:
<french>[French translation]</french>
</output_format>
```

### 2. Combining XML with Variable Placeholders

For templates that need to be reused with different inputs, combine XML structure with variable placeholders:

```
You are a legal document analyzer.

<contract>
{{CONTRACT_TEXT}}
</contract>

<legal_standards>
{{LEGAL_STANDARDS}}
</legal_standards>

<instructions>
1. Analyze the contract provided in the <contract> tags
2. Identify any clauses that may conflict with the standards in <legal_standards>
3. For each potential issue, explain the conflict and suggest alternative language
</instructions>

<format>
{
  "compliant": true/false,
  "issues": [
    {
      "clause": "[clause reference]",
      "conflict": "[explanation]",
      "suggestion": "[alternative language]"
    }
  ]
}
</format>
```

### 3. Context Windowing for Long Documents

For working with long documents that exceed Claude's processing capabilities, use XML tags to create "windows" into specific parts of the document:

```
You are analyzing a lengthy legal document. I will provide sections with XML tags.

<current_section>
[Content of the current section being analyzed]
</current_section>

<previous_context>
[Brief summary of relevant preceding content]
</previous_context>

<instructions>
Analyze the <current_section> in the context of <previous_context> and identify:
1. Key legal provisions
2. Potential ambiguities
3. Cross-references to other sections
</instructions>
```

## Putting It All Together: Complete System Prompt Template

Here's a comprehensive template incorporating all best practices:

```
[Brief natural language introduction establishing Claude's role and context]

<system>
You are a [specific role] tasked with [general purpose].
Your primary objective is to [main goal].
</system>

<context>
[Background information needed to understand the task]
</context>

<input>
[The specific content being worked with]
</input>

<examples>
  <example>
    <sample_input>[Example input]</sample_input>
    <sample_output>[Example output]</sample_output>
    <explanation>[Why this is a good response]</explanation>
  </example>
</examples>

<instructions>
1. [First instruction]
2. [Second instruction]
3. [Third instruction]
...
</instructions>

<thinking>
Please work through this [task] step-by-step:
1. First, [initial analysis step]
2. Then, [intermediate reasoning step]
3. Finally, [concluding analysis step]
</thinking>

<format>
[Example or description of how you want the output structured]
</format>

Claude: <answer>
[Prefilled response that sets the output format]
</answer>
```

## Evaluation and Iteration

After creating a system prompt using this template, follow these steps to ensure optimal performance:

1. **Test with varied inputs**: Try the prompt with different inputs to ensure it handles edge cases.

2. **Analyze failures**: When Claude's responses don't meet expectations, identify which part of the prompt could be improved.

3. **Iterate on tag structure**: Experiment with different tag organizations to find what works best for your specific use case.

4. **Compare performance**: Test variants of your prompt to identify which structures produce the best results.

5. **Simplify when possible**: Remove unnecessary complexity that doesn't improve performance.

## Summary of Best Practices

1. **Use natural language introduction** to establish context
2. **Create descriptive, purpose-driven tags** that clearly indicate their content
3. **Only use tags where necessary** for separation of content types
4. **Maintain consistency** in tag naming throughout prompts
5. **Keep nesting reasonable** (3-4 levels maximum)
6. **Combine XML with chain-of-thought** for complex reasoning
7. **Prefill responses** to control output format
8. **Provide examples** within XML structure for better learning
9. **Test and iterate** to optimize performance

Remember that simpler is often better - use only the tags necessary for your specific task rather than including every possible section in this template.

## Further Resources

For more information on prompt engineering with Claude, refer to:

1. Anthropic's official documentation on XML tags
2. Claude prompt engineering guides
3. Example galleries for different use cases
4. Prompt templates for specific domains

## Example Use Cases

### Financial Analysis Prompt

```
You are a financial analyst tasked with evaluating quarterly reports.

<report>
[Quarterly financial report content]
</report>

<instructions>
1. Analyze key performance indicators in the report
2. Identify any concerning trends or notable achievements
3. Compare results to industry benchmarks
4. Provide recommendations based on the analysis
</instructions>

<thinking>
Please work through this analysis step-by-step. First, extract the key metrics and compare them to previous quarters. Then, identify trends and calculate growth rates. Finally, compare to industry standards and formulate specific recommendations.
</thinking>

<format>
Your analysis should include:
- Summary of financial health (1 paragraph)
- Key metrics table showing current vs previous performance
- Bullet points listing main strengths and concerns
- 3-5 actionable recommendations
</format>
```

### Content Moderation System

```
You are a content moderation assistant responsible for reviewing user-generated content.

<policy>
[Content moderation policy details]
</policy>

<content>
[User-generated content to review]
</content>

<instructions>
1. Review the content for violations of the policy
2. Identify specific policy violations, if any
3. Explain your reasoning for each identified violation
4. Recommend appropriate actions based on violation severity
</instructions>

<format>
Please structure your response as:
{
  "violates_policy": true/false,
  "violations": [
    {
      "type": "[violation type]",
      "severity": "low/medium/high",
      "explanation": "[explanation]"
    }
  ],
  "recommended_action": "[action]",
  "reasoning": "[overall reasoning]"
}
</format>
```

### Creative Writing Coach with Response Prefilling

```
You are a creative writing coach helping authors improve their work.

<original_text>
[Writer's original text]
</original_text>

<instructions>
Please provide constructive feedback on this writing sample. Focus on:
- Strengths of the writing
- Areas for improvement
- Specific suggestions to enhance clarity, engagement, and impact
</instructions>

<examples>
<example>
<feedback>
The opening paragraph effectively establishes the setting with vivid sensory details. The character's internal monologue feels authentic, but could be streamlined to improve pacing. Consider reducing repetitive descriptions of the forest and varying sentence structure to create more rhythm in your prose.
</feedback>
</example>
</examples>

<answer>
[Prefill Claude's response with an opening tag to control output]
</answer>

<output>
[How you want Claude to present the final result after thinking]
</output>
```

## Response Prefilling Technique

```
[Brief natural language introduction establishing role and context]

<context>
[Background information needed to understand the task]
</context>

<input>
[The specific content being worked with]
</input>

<instructions>
[Clear, numbered or bulleted steps for what you want Claude to do]
</instructions>

<thinking>
[Instruction for Claude to reason step-by-step]
</thinking>

<format>
[Example or description of how you want the output structured]
</format>
```
