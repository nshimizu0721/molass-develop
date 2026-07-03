(coding_style)=
# Coding Style

## Purpose

The coding style for Molass Library is designed to be researcher-friendly, making it easy for both researchers and programmers to read, maintain, and contribute to the codebase. All style guidelines support inclusivity and clarity, prioritizing scientific users who may not be professional programmers.

**Note:** To ensure Copilot's advice and code suggestions follow these guidelines, start your chat session with the following magic phrase:

> Please follow the Copilot guidelines in this project for all advice and responses.

This magic phrase instructs Copilot to apply the specific rules and recommendations documented in the project's guidelines, such as those found in [Copilot/copilot-guidelines.md](https://github.com/biosaxs-dev/molass-library/blob/master/Copilot/copilot-guidelines.md) in the [molass-library](https://github.com/biosaxs-dev/molass-library) code repository. For details on these rules, refer to the guidelines file directly.

This helps maintain consistency and ensures Copilot recommendations are aligned with project standards.

## Key Principles

1. **Researcher-Friendly Code**  
   - Avoid advanced language features that may confuse non-programmers.
   - Prefer clear, simple constructs over clever or complex solutions.
   - Write code that is easy to understand, modify, and extend.

2. **Why-Oriented Documentation**  
   - Docstrings and comments should explain the reasoning behind design decisions, not just what the code does.
   - Example:
     ```python
     def find_element(elements, target):
         """
         Finds the index of the target element in the list using linear search.

         Linear search is used because the input list is expected to be small and unsorted.
         Sorting for binary search would add unnecessary overhead in this use case.
         """
         for i, element in enumerate(elements):
             if element == target:
                 return i
         return -1
     ```

3. **Consistent Style**  
   - Follow established formatting and naming conventions.
   - Use clear, descriptive names for variables, functions, and classes.
   - Keep line lengths reasonable and code blocks focused.

4. **Copilot Guidelines Integration**  
   - When seeking advice or code suggestions, always refer to the Copilot guidelines.
   - Prefer solutions and examples that are already part of the Molass Library or documented in the Copilot folder.
   - If a new best practice or rule emerges, propose its addition to the guidelines.

## Practical Advice

- Avoid features such as metaclasses, decorators with complex logic, or advanced type annotations unless necessary.
- Use standard library modules and well-known patterns.
- Document not only how but why code is written a certain way.
- When in doubt, prioritize clarity and maintainability.

## Attribution

These coding style guidelines were developed in collaboration with GitHub Copilot (GPT-4.1) and are periodically reviewed to ensure alignment with project goals and user needs.