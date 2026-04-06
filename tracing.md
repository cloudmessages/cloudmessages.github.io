# Tracing

CloudMessages is designed so a query can lead to a command and then to resulting events while preserving `correlationid`, `requestid`, and distributed tracing metadata across the whole flow.
