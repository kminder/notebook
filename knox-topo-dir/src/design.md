Instead of a single topology file there would be directory.
For example `sandbox.xml` would become `sandbox`.
The creation of `sandbox.war.{timestamp}` would not change but the timestamp would be that of the _newest_ file in the dir.
Could also use a timestamp or trigger file in the directory who's timestamp would be monitored.
The topo dir would contain the following files:
```
{GATEWAY_HOME}/deployments/sandbox/
    providers.xml
    gateway.xml
    services.xml
    shiro.ini (optional, provider specific)
    authorization.ini (optional, provider specific)
    hostmap.ini (optional, provider specific)
```

Thinking around the content of each follows.

# providers.xml

This file catalogs and configures the available providers in the cluster.
NOTE: Need to include a provider out of the box that can contribute a standard servlet filter.

```xml
<providers>
    <provider>
        <role/>
        <name/>
        <enabled/>
        <config/>
        <params>
            <param>
                <name/>
                <value/>
            </param>
        </params>
    </provider>
</providers>
```

/providers
: Collects all of the defined providers.

/providers/provider
: Defines a specific provider.

/providers/provider/role
: Defines the role of a specific provider.

/providers/provider/name
: Defines the name of a specific provider.  Used when there are more that one provider for a given role.

/providers/provider/enabled
: Enabled or disabled the provider.  Optional.  Default is true.

/providers/provider/config
: A reference to the provider's configuration file.  Should be a URL relative to the provider.xml.
This file will be copied to the WEB-INF directory unmodified at deployment time unless the provider's deployment contributor modifies it.
If URL is non-relative it will be "downloads" at deployment time.

/providers/provider/params
: Collects parameters used for simple configuration of the provider.

/providers/provider/param
: A parameter used to configure the provider.

/providers/provider/name
: The name of a provider configuration parameter.

/providers/provider/value
: A provider configuration value.

# gateway.xml

This file configures the gateway for this cluster.  Most importantly it defines provider chains.

```xml
<gateway>
    <chains>
        <default/>
        <chain>
            <name/>
            <providers>
                <provider>
                    <role/>
                    <name/>
                </provider>
            </providers>
        </chain>
    </chains>
</gateway>
```

/gateway
: Collects all of the gateway configuration for this cluster.

/gateway/chains
: Collects all of the provider chains configured for this cluster.

/gateway/chains/default
: Specifies the name of the default provider chain of a given service or endpoint doesn't explicitly identify one.

/gateway/chains/chain
: Defines a specific provider chain

/gateway/chains/chain/name
: Specifies the name of this provider chain.

/gateway/chains/chain/providers
: An ordered collection of the providers to be used in this chain.

/gateway/chains/chain/providers/provider
: The details for a provider that will be used in the chain.

/gateway/chains/chain/providers/provider/role
: The role of a provider to use in the chain.

/gateway/chains/chain/providers/provider/name
: The name of a provider to use in the chain.  Only required if the default provider for a role is insufficient.


# services.xml

This file configures the location and possibly configuration of service endpoints in the cluster.

```xml
<services>
    <service>
        <role>HDFS</role>
        <endpoints>
            <endpoint>
                <role>NAMENODE</role>
                <url>hdfs://localhost:8020</url>
                <chain></chain>
            </endpoint>
            <endpoint>
                <role>WEBHDFS</role>
                <url>http://localhost:50070/webhdfs</url>
                <chain></chain>
            </endpoint>
        </endpoints>
        <params>
            <param>
                <name/>
                <value/>
            </param>
        </params>
    </service>
</services>
```

/services
: Topology information about the services in the given cluster.

/services/service
: The definition of a specific service in the cluster

/services/service/role
: The role of the service (e.g. HDFS, OOZIE).

/services/service/endpoints
: The endpoints exposed by the service.

/services/service/endpoint
: An endpoint exposed by the service.

/services/service/endpoint/role
: The role of the endpoint exposed by the service (e.g. WEBHDFS, NAMENODE)

/services/service/endpoint/url
: The URL of the endpoint exposed by the service.

/services/service/endpoint/chain
: The chain to us for this endpoint if the default chain is insufficient.

/services/service/params
: Simple configuration parameters consumed by the service's deployment contributor if required.

/services/service/params/param
: A configuration parameter.

/services/service/params/param/name
: The name of a configuration parameter.

/services/service/params/param/value
: The value of a configuration parameter.

# shiro.ini

This file would be references withing the <config/> element of the Shrio provider.

```ini
[main]
ldapRealm = org.apache.shiro.realm.ldap.JndiLdapRealm
ldapRealm.userDnTemplate = uid={0},ou=people,dc=hadoop,dc=apache,dc=org
ldapRealm.contextFactory.url = ldap://localhost:33389
ldapRealm.contextFactory.authenticationMechanism = simple

[urls]
/** = authcBasic
```

# authorization.ini

This file would be references withing the <config/> element of the Authorization provider.

```ini
HDFS.acls = guest;*;*
OOZIE.acls = guest;*;*
```

# hostmap.ini

This file would be references withing the <config/> element of the Hostmap provider.

```ini
localhost=sandbox,sandbox.hortonworks.com
```

