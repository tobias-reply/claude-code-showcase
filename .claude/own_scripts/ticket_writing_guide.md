# Ticket Writing Guide

## Philosophy

A well-written ticket is **closeable**, **accessible**, **complete**, and **accurate**. Someone other than you should be able to execute on the ticket without consulting you for more info. The assigned engineer should know when they are done, and no one should be able to contest its "doneness".

## Why This Matters

- **Velocity**: Poorly written tickets slow down engineers and tank team velocity
- **Quality**: Clear tickets help engineers get it right the first time
- **Planning**: Well-structured tickets enable better sprint planning and prioritization

## Common Problems with Bad Tickets

1. Writer is in a rush
2. Writer doesn't have a strong grasp of what needs to get done
3. Writer forgets that readers lack context
4. Ambiguous acceptance criteria
5. Missing edge cases

---

## Bug Template

### Brief Summary
What is broken and why is that a problem? The "why" helps determine priority.

### Steps to Reproduce
Detailed step-by-step guidance assuming baseline familiarity with the system. When in doubt, provide more detail.

### Desired Behavior
If the bug were not present, what would we expect to see?

### Current (Undesirable) Behavior
What is it doing now instead?

### Supporting Data
- Stacktraces
- Links to logs
- Screenshots
- Error messages
- Anything that saves engineer time and reduces ambiguity

### When Closing (Required)
- [ ] Concrete proof of the fix (logs, screenshots) matching Desired Behavior
- [ ] Link to relevant code changes (Git, PR)

---

## Feature Template

### Brief Summary
What do we need to implement and why? The "why" informs priority.

### Definition of Done
**CRITICAL**: Detailed, concrete, demonstrable criteria that must be met. Include:
- Main functionality requirements
- Edge cases and how system should behave in each
- Performance requirements (if applicable)
- Security requirements (if applicable)
- Documentation requirements

### Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Edge case handling for X
- [ ] Edge case handling for Y

### When Closing (Required)
- [ ] Concrete proof for each Definition of Done item
- [ ] Supporting documentation (screenshots, logs)
- [ ] Link to relevant code changes (Git, PR)

---

## Investigation/Research Template

### Brief Summary
What needs to be investigated and why?

### Questions to Answer
1. Question 1
2. Question 2
3. Question 3

### Definition of Done
Clear criteria for what questions must be answered or what must be discovered.

### When Closing (Required)
- [ ] Address each question from Definition of Done
- [ ] Summary of findings
- [ ] Recommendations for next steps
- [ ] Links to supporting data/research

---

## Sizing Guidelines

- **XS**: Very small tasks, quick fixes, typos (< 2 hours)
- **S**: Small features or bugs, minor changes (2-8 hours)
- **M**: Medium features, moderate complexity (1-3 days)
- **L**: Large features, significant changes (3-7 days)
- **XL**: Very large features, major changes (1-2 weeks)

---

## Common Pitfalls to Avoid

### Pitfall #1: "Writing the ticket takes longer than doing it"
- **If true**: Your process may be too onerous (keep it lean)
- **Or**: You're underestimating the work (get a gut check from teammate)
- **Or**: It's genuine low-hanging fruit (just do it, but document what you did)

### Pitfall #2: "I don't have enough info to write the ticket"
- **Solution**: Create a spike/investigation ticket first
- Layout unknowns and turn them into knowns
- Capture the work of sussing out significant unknowns
- Prioritize alongside sprint work

---

## Validation Checklist

Before submitting ANY ticket, verify:

- [ ] **Closeable**: Clear Definition of Done exists
- [ ] **Accessible**: Someone else could execute without asking you questions
- [ ] **Complete**: All necessary information is present
- [ ] **Accurate**: Information is correct and up-to-date
- [ ] **Context**: Assumed knowledge is minimal and stated
- [ ] **Edge cases**: Identified and handling specified
- [ ] **Priority**: "Why" is explained to inform prioritization
