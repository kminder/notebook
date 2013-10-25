Instead of a single topology file there would be directory.
For example `sandbox.xml` would become `sandbox`.
The creation of `sandbox.war.{timestamp}` would not change but the timestamp would be that of the _newest_ file in the dir.
The topo dir would contain the following files
```
{GATEWAY_HOME}/deployments/sandbox/
    providers.xml
    gateway.xml
    services.xml
    shiro.ini (optional, provider specific)
    authorization.ini (optional, provider specific)
    hostmap.ini (optional, provider specific)
```

# gateway.xml
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

# providers.xml
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

# services.xml
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

# shiro.ini
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
```ini
HDFS.acls = guest;*;*
OOZIE.acls = guest;*;*
```

# hostmap.ini
```ini
localhost=sandbox,sandbox.hortonworks.com
```

