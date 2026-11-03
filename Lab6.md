# Practical Lab 6: Building an ELIZA-Style Chatbot Using Regular Expressions

**Course:** Artificial Intelligence – BSCS 4th Semester  
**Topic:** Historical Case Studies in AI – ELIZA  
**Environment:** Python 3, Google Colab or VS Code  
**Library:** Python `re` module

> **Course-breakup practical:** This lab is prepared from the Week 6 practical requirement in the supplied course breakup. A separate official Lab 6 file was not linked in the supplied instructions.

---

## 1. Objectives

1. Understand the historical ELIZA chatbot.
2. Learn how simple pattern matching can simulate conversation.
3. Use Python's `re` library for regular-expression matching.
4. Extract information from user statements.
5. Build a basic rule-based conversational program.

## 2. Problem Statement

Create a Python chatbot inspired by the historical ELIZA program. The chatbot should accept user statements and generate responses using regular expressions and predefined response rules.

This is a demonstration of pattern matching, not a real therapist or a system capable of understanding emotions or providing mental-health care.

## 3. Background

ELIZA was an early natural-language processing program created by Joseph Weizenbaum in the 1960s. It used pattern matching and response templates to simulate a conversation.

For example:

- User: `I feel tired`
- Chatbot: `Why do you feel tired?`

The program does not understand the meaning in the human sense. It matches text patterns and generates responses based on rules.

## 4. Python Implementation

```python
import re
import random


# Each pattern is paired with a list of possible responses.
# The first capturing group is reused in some response templates.
RULES = [
    (
        r"\bI feel (.+)",
        [
            "Why do you feel {0}?",
            "Can you explain what makes you feel {0}?",
            "How long have you felt {0}?"
        ]
    ),
    (
        r"\bI am (.+)",
        [
            "How does being {0} affect you?",
            "Why do you think you are {0}?",
            "Tell me more about being {0}."
        ]
    ),
    (
        r"\bI need (.+)",
        [
            "Why do you need {0}?",
            "Would having {0} be helpful?",
            "What would happen if you did not have {0}?"
        ]
    ),
    (
        r"\bI like (.+)",
        [
            "What do you like most about {0}?",
            "When did you start liking {0}?"
        ]
    ),
    (
        r"\bI think (.+)",
        [
            "What makes you think {0}?",
            "Can you explain why you think {0}?"
        ]
    ),
    (
        r"\bBecause (.+)",
        [
            "Is that the main reason?",
            "Are there any other reasons?",
            "Does that explanation make sense to you?"
        ]
    ),
    (
        r"\bMy (.+)",
        [
            "Tell me more about your {0}.",
            "What would you like to share about your {0}?"
        ]
    )
]


def generate_response(user_input):
    text = user_input.strip()

    for pattern, responses in RULES:
        match = re.search(pattern, text, re.IGNORECASE)

        if match:
            extracted_text = match.group(1).strip()
            response = random.choice(responses)

            return response.format(extracted_text)

    # Fallback responses when no rule matches.
    fallback_responses = [
        "Please tell me more.",
        "Why do you say that?",
        "How does that make you feel?",
        "Can you explain that further?",
        "What do you think about this?"
    ]

    return random.choice(fallback_responses)


def eliza_chatbot():
    print("=" * 50)
    print("ELIZA-STYLE CHATBOT")
    print("Type 'bye' to end the conversation.")
    print("=" * 50)

    while True:
        user_input = input("\nYou: ").strip()

        if user_input.lower() in ("bye", "exit", "quit"):
            print("ELIZA: Goodbye. Thank you for chatting.")
            break

        if not user_input:
            print("ELIZA: Please type something.")
            continue

        response = generate_response(user_input)
        print("ELIZA:", response)


if __name__ == "__main__":
    eliza_chatbot()
```

## 5. Code Explanation

- `import re` loads Python's regular-expression library.
- `RULES` stores patterns and possible responses.
- `re.search()` checks whether a pattern appears in the user's statement.
- `re.IGNORECASE` allows matching regardless of capitalization.
- `match.group(1)` extracts the text captured by the first pair of parentheses.
- `random.choice()` selects one response from the matching response list.
- Fallback responses are used when no pattern matches.
- The loop continues until the user types `bye`, `exit`, or `quit`.

## 6. Sample Conversation

```text
==================================================
ELIZA-STYLE CHATBOT
Type 'bye' to end the conversation.
==================================================

You: I feel tired
ELIZA: Why do you feel tired?

You: I need help
ELIZA: Would having help be helpful?

You: My family
ELIZA: Tell me more about your family.

You: bye
ELIZA: Goodbye. Thank you for chatting.
```

The chatbot chooses responses randomly, so your exact conversation may differ. The templates can be refined to improve grammar and conversational quality.

## 7. Testing

| User input | Expected behavior |
|---|---|
| `I feel happy` | Matches the `I feel` rule |
| `I am confused` | Matches the `I am` rule |
| `I need guidance` | Matches the `I need` rule |
| `I like programming` | Matches the `I like` rule |
| `Because I was busy` | Matches the `Because` rule |
| `Hello there` | Uses a fallback response |
| `bye` | Ends the conversation |

## 8. Limitations

1. The chatbot relies on predefined patterns.
2. It does not understand context or intent.
3. It can produce repetitive or grammatically awkward responses.
4. It cannot learn new knowledge from conversations.
5. It should not be treated as a real mental-health or medical service.

## 9. Result and Discussion

The ELIZA-style chatbot uses regular expressions to identify familiar sentence structures and select predefined responses. It demonstrates an early symbolic approach to natural-language interaction.

## 10. Conclusion

A simple ELIZA-inspired chatbot was implemented using Python's `re` library, string processing, and response templates. The practical demonstrated how early conversational systems used pattern matching to simulate human dialogue.

## 11. Learning Check

1. Who created ELIZA?
2. What is pattern matching?
3. What is a regular expression?
4. What does `re.IGNORECASE` do?
5. What is the difference between a rule-based chatbot and a modern language model?

**Lecture reference:** [Week 6 – Historical Milestones of Symbolic AI](https://github.com/beyond2013/AI/blob/master/markdown/Week6.md)
