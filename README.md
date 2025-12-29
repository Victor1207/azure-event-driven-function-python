# Azure Secure Event-Driven System (Manual Deployment)

## Overview
This project demonstrates a **secure, event-driven architecture on Microsoft Azure**, built **manually via the Azure Portal** to showcase core Azure engineering skills before Infrastructure as Code automation.
The system processes messages asynchronously using **Azure Storage Queues** and **Azure Functions (Python)**, following cloud-native and serverless best practices.


## Architecture
The solution follows an **event-driven serverless pattern**:
1. Events are sent to an **Azure Storage Queue**
2. A **Python Azure Function** is triggered automatically
3. The function processes the message and writes logs
4. Monitoring and diagnostics are enabled for observability
> This architecture is designed to be **scalable, cost-efficient, and secure**, suitable for modern cloud workloads.
> 
## Azure Services Used
- **Azure Function App** (Python, Queue Trigger)
- **Azure Storage Account**
  - Queue Storage
- **Azure Monitor / Log Stream**
- **Managed Identity**
- **Azure Portal (Manual Deployment)**
All services were selected to remain **free-tier / student subscription safe**.

## Deployment Method
This project was deployed **manually via the Azure Portal** to:
- Deepen understanding of Azure services
- Validate architectural decisions
- Identify real-world configuration challenges

## Key Configuration Details
### Azure Function
- Runtime: **Python**
- Trigger type: **Storage Queue Trigger**
- Queue name: `events-queue`
- Authentication: **AzureWebJobsStorage**
- Execution model: **Event-driven / serverless**

### Queue Message Processing Logic
```python
import azure.functions as func
import logging

app = func.FunctionApp()

@app.queue_trigger(
    arg_name="azqueue",
    queue_name="events-queue",
    connection="AzureWebJobsStorage"
)
def ProcessEventQueue(azqueue: func.QueueMessage):
    logging.info(
        "Processed message: %s",
        azqueue.get_body().decode("utf-8")
    )

## Validation & Testing

The solution was validated by:
Manually adding messages to the storage queue
Confirming function execution via Log Stream
Verifying message processing logs
Ensuring no public access exposure

## Challenges Encountered & Solutions
1. Azure Permission & Propagation Delays

Issue: Role assignments and identities did not appear immediately.

Solution:
Verified Managed Identity using Azure CLI
Allowed time for Azure RBAC propagation
Confirmed correct scope assignment

2. Application Insights Not Available

Issue: Code-less Application Insights was not supported due to subscription/runtime constraints.

Solution:
Used Azure Log Stream for real-time validation
Confirmed function execution via logs

3. Function Not Appearing Immediately

Issue: Queue-triggered function did not show expected behavior initially.

Solution:
Ensured correct queue name configuration
Verified AzureWebJobsStorage connection
Redeployed function code and re-tested

## Security Considerations
No hard-coded secrets
Storage access via managed configuration
Serverless execution reduces attack surface
Ready for future integration with Key Vault

## Author
Olasehinde Victor
