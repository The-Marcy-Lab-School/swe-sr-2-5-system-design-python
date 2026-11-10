# Short Responses

For this short response assignment, aim to write a response with the following
qualities (your instructor will give you feedback on these areas):

- [ ] Addresses all parts of the prompt
- [ ] Accurately uses relevant technical terminology
- [ ] Is free of grammar and spelling mistakes (double check with Grammarly!)
- [ ] Uses markdown to enhance readability (preview in VS Code with
      Command/Control + Shift + V)
- [ ] Is easy to comprehend

Write your response in the space provided. Make sure to preview your markdown
to check how it is rendered before submitting.

## Question 1

Here is a stakeholder talking. Nothing is written down.

> "We run a tool library. Members borrow tools — drills, ladders, that sort of
> thing. Each tool has a deposit, and some need a safety briefing before anyone
> takes them out. A member can have three tools at once, and we need to know
> who has what and when it is due back. If a tool comes back damaged we take it
> out of circulation until it is fixed."

**First**, draw a UML class diagram for this system. Use at least four classes.
For each one, list its attributes and its methods, and label every relationship
between classes with what it is and which way it points.

ASCII is fine. So is a photo of paper. What matters is that every class, every
attribute and every relationship is on it.

**Second**, mark each attribute `+` for public or `-` for non-public, and give
**one sentence per `-`** saying what would go wrong if any code could set it
directly. A `-` with no reason is not finished — "it feels internal" is not a
reason.

**Third**, name the class that owns the three-tools-at-once rule, and say why
that class and not another.

There is no single right diagram. Two sensible designs for this brief will
differ, and the reasoning is what is being read. What would be wrong is a
diagram where nobody owns the limit, or where a rule has no home.

### Your Response

[Write your response here]

## Question 2

A reference between two classes can point one way or both. Pointing both ways
is not free: every write has to update two places, and the day they disagree is
the day a bug appears that no single class is wrong about.

Here are three pairs from the tool library.

```
A.  Member  and  Loan
B.  Tool    and  Loan
C.  Library and  Member
```

For **each pair**, decide whether the reference is one-way or bidirectional, and
if one-way, which class holds it. Defend each choice by naming:

1. **The question that direction makes easy.** "A member needs their current
   loans, so `Member` holds a list of `Loan`" is the shape of a real answer.
2. **The question it makes hard**, and how you would answer it anyway. Usually
   by asking the coordinator to search.

**Then**, pick whichever one you made bidirectional — or, if you made none of
them bidirectional, pair B — and answer this: name the sequence of operations
that would leave the two sides disagreeing, and say which class you would make
responsible for preventing it.

The project will ask you to make these calls for your own system and defend
them. Getting into the habit of naming the question each direction answers is
how you stop guessing.

### Your Response

[Write your response here]

## Question 3

A `Loan` connects a `Member` to a `Tool`. Something has to make it. Four
candidates:

```py
loan = Loan(member, tool)           # A. whoever is calling
loan = member.borrow(tool)          # B. the member
loan = tool.lend_to(member)         # C. the tool
loan = library.lend(member, tool)   # D. the library
```

All four are legal Python and all four would work.

**First**, pick one and defend it by naming **the real-world action it
mirrors**. Who, at a tool library, actually performs the act of lending?

**Second**, for each of the three you rejected, name one specific thing that
goes wrong. Not "it is bad design" — name the rule that gets skipped, or the
knowledge the class would need that it has no business having.

**Third**, answer this. Option **A** has no rule-checking anywhere, and yet it
is how you will build objects in half your tests. Say why it is fine in a test
and not fine in the program.

**Fourth**, your project has at least one connection class in it. Name it, name
which object you will have create it, and give the real-world action. One
sentence each.

### Your Response

[Write your response here]

## Question 4

This question asks you to **write test code**, not prose. Project requirement 9
asks for exactly this on your own system.

```py
class Library:
    LOAN_LIMIT = 3

    def __init__(self):
        self.loans = []

    def lend(self, member, tool):
        if tool.on_loan:
            raise ValueError(f"{tool.name} is already out.")
        if len(member.loans) >= Library.LOAN_LIMIT:
            raise ValueError(f"{member.name} already has three tools.")
        tool.on_loan = True
        member.loans.append(tool)
        self.loans.append((member, tool))
        return f"{member.name} borrowed {tool.name}"
```

Write **one** test that attempts a refused loan and asserts that **nothing
moved**: not the tool's status, not the member's loans, not the library's.
Use `pytest.raises(ValueError)`.

Use `src/playground.py` in the code repo to run it, then paste it here.

Then answer two things:

1. **Write the broken version of `lend` your test would catch** and a
   `pytest.raises(ValueError)`-only test would not. You have seen it — one of
   this assignment's Debug questions is this bug.
2. There are **three** things to assert here, not one. Say what each one
   protects, and which of the three you would have forgotten if nobody had
   said "and every collection".

The second part is the question. A refusal that half-ran is worse than no
refusal, because the caller has been told it failed while the system says
otherwise, and the two will not be reconciled until someone reads both.

### Your Response

[Write your response here]

