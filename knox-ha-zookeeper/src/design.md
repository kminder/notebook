
```
/knox
  /{cluster}
    /conf
      /gateway-site.xml
      /log4j.properties
      /security
        /master
        /registry
        /keystores
          /__gateway-credentials.jceks
          /gateway.jks
          /{topology}-credentials.jceks
    /deployments
      /{topology}.xml
```

```plantuml
@startuml
'autonumber
hide footbox
participant "Client" as C
participant "SSO\nServer" as S
participant "Hadoop\nService" as H
C->S: 1. credentials
C<--S: tokens
C->H: 2. access token
C<--H: requested resource
@enduml
```