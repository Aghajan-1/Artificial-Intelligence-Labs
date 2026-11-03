# Practical Lab 1: Building a Rule-Based Knowledge-Based System

**Course:** Artificial Intelligence – BSCS 4th Semester  
**Topic:** Knowledge-Based Systems and Rule-Based Expert Systems  
**Environment:** Python 3, Google Colab or VS Code  

---

## 1. Objectives

After completing this practical, students should be able to:

1. Explain the basic structure of a Knowledge-Based System (KBS).
2. Distinguish between a knowledge base and an inference mechanism.
3. Represent knowledge using facts and IF–THEN rules.
4. Implement a simple expert system using Python conditional logic.
5. Test the system with different user inputs.

## 2. Problem Statement

Develop a small **computer troubleshooting expert system**. The program asks the user about common computer problems and suggests a possible cause and a basic troubleshooting action.

This is an educational rule-based demonstration, not a replacement for professional technical support.

## 3. Background

A Knowledge-Based System stores facts and rules about a particular domain and uses them to reach conclusions.

- **Knowledge Base:** Stores facts and IF–THEN rules.
- **Inference Engine:** Applies the rules to the facts supplied by the user.
- **User Interface:** Collects answers and displays the result.

Example rule:

`IF the computer has no power AND the power cable is disconnected THEN suggest checking the power cable.`

## 4. Python Implementation

Copy the following code into a Python file or a Google Colab code cell.

```python
def ask_yes_no(question):
    """Ask a question and accept yes/no answers."""
    while True:
        answer = input(question + " (yes/no): ").strip().lower()

        if answer in ("yes", "y"):
            return True
        elif answer in ("no", "n"):
            return False
        else:
            print("Please enter yes or no.")


def computer_expert_system():
    print("=" * 55)
    print("COMPUTER TROUBLESHOOTING EXPERT SYSTEM")
    print("=" * 55)
    print("Answer the following questions to identify a possible issue.\n")

    # Collect facts from the user
    no_power = ask_yes_no("Does the computer fail to turn on?")
    if no_power:
        cable_connected = ask_yes_no("Is the power cable securely connected?")
        if not cable_connected:
            print("\nPossible issue: Disconnected power cable.")
            print("Suggestion: Connect the cable securely and try again.")
        else:
            print("\nPossible issue: Power supply, battery, or hardware fault.")
            print("Suggestion: Check the power adapter or contact a technician.")
        return

    no_internet = ask_yes_no("Is the computer unable to access the internet?")
    if no_internet:
        wifi_on = ask_yes_no("Is Wi-Fi enabled?")
        if not wifi_on:
            print("\nPossible issue: Wi-Fi is disabled.")
            print("Suggestion: Enable Wi-Fi and reconnect to the network.")
        else:
            router_ok = ask_yes_no("Are other devices connected to the same router?")
            if not router_ok:
                print("\nPossible issue: Router or internet service problem.")
                print("Suggestion: Restart the router and contact your provider if needed.")
            else:
                print("\nPossible issue: Device-specific network configuration.")
                print("Suggestion: Reconnect to Wi-Fi and check network settings.")
        return

    slow = ask_yes_no("Is the computer running unusually slowly?")
    if slow:
        print("\nPossible issue: High resource usage or too many startup applications.")
        print("Suggestion: Check Task Manager, close unnecessary applications,")
        print("and make sure sufficient storage space is available.")
        return

    print("\nNo matching problem was identified by this simple rule set.")
    print("Please describe the issue to a qualified technician if it continues.")


if __name__ == "__main__":
    computer_expert_system()
```

## 5. How the Code Works

1. `ask_yes_no()` collects a yes/no answer and validates it.
2. `computer_expert_system()` contains the rules of the expert system.
3. Each `if`, `else`, and nested condition represents a decision rule.
4. The program follows the matching branch and displays a suggestion.
5. `if __name__ == "__main__":` starts the program when the file is run directly.

## 6. Sample Execution

```text
=======================================================
COMPUTER TROUBLESHOOTING EXPERT SYSTEM
=======================================================
Answer the following questions to identify a possible issue.

Does the computer fail to turn on? (yes/no): yes
Is the power cable securely connected? (yes/no): no

Possible issue: Disconnected power cable.
Suggestion: Connect the cable securely and try again.
```

## 7. Testing

| Test | User answers | Expected result |
|---|---|---|
| 1 | Does not turn on; cable disconnected | Check the power cable |
| 2 | Does not turn on; cable connected | Possible power or hardware fault |
| 3 | Internet unavailable; Wi-Fi disabled | Enable Wi-Fi |
| 4 | Computer is slow | Check resource usage and startup applications |
| 5 | None of the listed problems | No matching rule found |

## 8. Result and Discussion

The program demonstrates a basic rule-based Knowledge-Based System. It collects facts from the user and applies predefined IF–THEN logic to suggest a possible solution. The knowledge base is represented through conditional statements, while Python's control flow performs the inference.

## 9. Conclusion

In this practical, a simple computer troubleshooting expert system was developed using Python. The exercise demonstrated how facts, rules, and conditional logic can be combined to simulate a basic expert system.

## 10. Learning Check

1. What is a Knowledge-Based System?
2. What is the difference between a knowledge base and an inference engine?
3. What is a production rule?
4. Why are conditional statements useful in a rule-based system?
5. Mention one limitation of this expert system.

**Reference:** [Course repository – Lab 1](https://github.com/beyond2013/AI/blob/master/markdown/Lab1.md)
