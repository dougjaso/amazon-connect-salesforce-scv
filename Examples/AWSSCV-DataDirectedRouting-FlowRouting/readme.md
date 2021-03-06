# Data-Directed Routing: Flow Routing
This example shows how you can collect data from a customer in Amazon Connect, pass that to Salesforce via REST API and invoke a flow that analyzes the data and returns routing destinations as requested. This is intended to demonstrate how existing routing logic built in Salesforce Flows can be used with Service Cloud Voice and Amazon Connect to control routing. 

**NOTE:** This example has been configured for Service Cloud Voice only. Modifications would need to be made to support routing in a non-SCV environment.

## Components
This example relies on configuration in Salesforce to work appropriately. The components used are:
- Salesforce
    - Queues
    - Skills
    - Users
    - Service Resources (agents) with skills assigned
    - Apex Classes
    - Flows
- AWS
    - AWS Lambda function
    - Amazon Connect contact flow
    - Amazon Connect Queues

## Example Flow
1. Customer calls into Amazon Connect
2. Contact flow invoked to get intent, subintent, and possibly focus
3. Lambda passes received values from Lex to flow via REST API to get routing targets
4. Flow finds an agent with the appropriate skills and a matching queue
5. Flow returns the targets to Lambda
6. Lambda processes the response to generate routable targets for Amazon Connect
7. Amazon Connect first tries to direct route to matched agents, if provided. If agent is online andavailable, call is route to agent
8. Next, if an agent was not found, call is routed to received queue
9. Agent connects to customer

## Variations
While this example invokes a flow, you could just as easily do a simple query using parameters gathered in Connect, or do a lookup in a custom table in Salesforce, or find the data in whatever makes sense for the use case. The critical piece is receiving the destination in a predictable format, and then formatting that data so that it becomes a routable destination, such as a queue ARN or an agent ID. 

## Disclaimer
This is simply an example that shows how one *could* approach this use case. There are cerrtainly other ways to accomplish this type of routing and there are obvious considerations that need to be made with this approach. This example is intended to guide thought, not to serve as a strict description of how one should accomplish data-directed routing.

## Installation
To install this example, start with the [prep instructions](Docs/prep.md).