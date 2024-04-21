# Password Cracking system
Design a system for a brute-force password-cracking service using the following components:

1. **Black Box System**: Validates whether the generated password is correct.
2. **AWS Instances**: Available for use at a cost.
3. **Standalone Servers**: Free-of-cost worker servers.

The system design should be as cost-effective and efficient as possible.

## Non-functional Requirements
- Minimize the cost
- Maximize the speed

## System Components
- **Black Box System:** This system will validate if the generated password is correct. We will assume it has an API endpoint for validation.
- **AWS Instances:** These instances will be used judiciously due to their cost. They can be used for managing the distribution of tasks and more complex processing requirements.
- **Standalone Worker Servers:** These servers are free and will be used to generate and test passwords. They can run simple scripts to generate passwords and send them to the black box system for validation.

## High level diagram
The design revolves around a master-worker architecture, where a central system (Master Node) coordinates multiple workers (Worker Nodes) that generate and check password candidates.
<src img="img/HighLevelDiagram.png">
