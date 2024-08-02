Seems theres no way to link together two logs. I identified a way using the number field. Now gotta edit the otel collector to be able to use number.

`The number field is kept in the body by the transform/pull_requests processor, but it is not explicitly set as an attribute or label. If you want the number field to be stored and searchable in Loki, you need to set it as an attribute and include it in the loki.attribute.labels configuration.`

The number field doesnt show up as a label in loki or grafana.

`To ensure the number field is included as a label in Loki, you need to modify the transform/pull_requests and attributes/pull_requests processors to include the number field.
Updated transform/pull_requests Processor
Add a statement to set the number attribute`