# Superset Runbook

## Key Locations

| Env                                    | Namespace                     |GitHub Repo                     |
|----------------------------------------|-------------------------------|--------------------------------|
| MOC-ZERO Prod                          | [opf-superset][superset]      | [repo][repo]                   |



## Common errors



### ValueError: Invalid decryption key

When rebooting superset, the initcontainer may crash and report `ValueError: Invalid decryption key` in the logs.

To fix this you may have to set the secret key to null so it may be refreshed. Follow these steps:

Run the following:

```bash
$ oc rsh ${SUPERSET_DB_POD}
sh-4.2$ psql
postgres=# \connect supersetdb
supersetdb=# UPDATE public.dbs SET password = null;
```

### Duplicate key value violates unique constraint

When rebooting superset, the init container might report `duplicate key value violates unique constraint`.

This happens when [oauth mapping][oauth-mapping] has overlapping users. If one user is in multiple groups then it will
try to add the user twice, but run in to a conflict. Ensure that this is not the case.

### Trino Error in Superset

If you keep seeing "Trino errors show up" and in super logs you see:
```
requests.exceptions.ConnectionError: ('Connection aborted.', RemoteDisconnected('Remote end closed connection without response'))
INFO:trino.exceptions:failed after 3 attempts
```
Try going to `Data > Databases` at the top, open edit panel for `opf-trino` database. Test the connection.
If it reports an error, try adding in the password again where it says `XXX..` and see if that works.


[oauth-mapping]: https://github.com/operate-first/apps/blob/master/odh-manifests/zero/superset/base/secret.yaml#L29
[superset]: superset
[repo]: https://github.com/operate-first/apps/tree/master/odh/overlays/moc/zero/kafka
