openidm-pizza
=============

Testing different openidm setups and integrations, all centered around pizza

# Pizza as a Custom Managed Object

If you startup `openidm` in the normal fashion, and try accessing a `pizza` resource you'll get an error

    curl --header "X-OpenIDM-Username: openidm-admin" --header "X-OpenIDM-Password: openidm-admin" http://localhost:8080/openidm/managed/pizza/?_queryId=query-all-ids | jq '.'
```json
{
  "message": "No route for pizza/",
  "reason": "Not Found",
  "error": 404
}
```

The [OpenIDM documentation](http://openidm.forgerock.org/doc/integrators-guide/index.html#custom-managed-objects) provides instructions for creating our custom object.

We edit `managed-obj-sample/conf/repo.orientdb.json` and add this OrientDB class so the database knows about the new object

```json
	    "managed_pizza" : {
	            "index" : [
                    {
                        "propertyName" : "_openidm_id",
                        "propertyType" : "string",
                        "indexType" : "unique"
                    }
                ]
            },
``` 

Then we edit `managed-obj-sample/conf/managed.json` and tell it about the managed type

```json
        {
            "name" : "pizza"
        }
```

Now if you start up `OpenIDM` useing the sample directory, and your pizza query will work

`./startup.sh -p ../openidm-pizza/managed-obj-sample/`


    curl --header "X-OpenIDM-Username: openidm-admin" --header "X-OpenIDM-Password: openidm-admin" http://localhost:8080/openidm/managed/pizza/?_queryId=query-all-ids | jq '.'
```json
{
  "conversion-time-ms": 0,
  "result": [],
  "query-time-ms": 0
}
```
