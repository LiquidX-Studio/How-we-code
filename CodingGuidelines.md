![image](./img/LiquidXLogo.png)
# Coding Guide
## Solidity
- Use established and well-tested libraries whenever possible (e.g., OpenZeppelin for Solidity smart contracts) to avoid code clones.
- Ensure that the code compiles without any warnings and errors. Provide the exact instructions for building the project and include any implicit dependencies and their versions (e.g., the npm version).
- Document all functions inline. Use NatSpec documentation for public and external functions.
- Ensure that both code and documentation are clear and are in English.
- Run the code through a spellchecker.
- Follow the Checks-Effects-Interactions pattern (possibly with a combination of reentrancy guards) to avoid __reentrancies__. 
- Treat all asset transfers as “interactions”.
- Run a static analyzer (e.g., Slither and MythX for Solidity) and review its output. Although these tools often raise flags for non-issues, they can sometimes catch a low-hanging fruit (e.g., ignored return value).
- Any public function that can be made external should be made external. This is both to save the gas and to reduce the possibility of bugs since external functions cannot be accessed internally.
- Avoid using assembly code unless absolutely necessary. The use of assembly increases audit times as it removes Solidity's guardrails and must be checked much more carefully.
- Document the use of unchecked. Describe why it is safe to skip arithmetic checks on each code block. Preferably for each operation.
- Ensure that the code comes with an extensive test suite. Your unit or functional tests should cover __All Boundary Conditions__
- Provide step-by-step instructions for running the test suite. Include any necessary information related to the setup and environment.
