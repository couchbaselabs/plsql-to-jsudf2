# plsql-to-jsudf2

### Use the release executable:
1. create output directory: ```mkdir output```
2. expected arguments:
    *   -u USER, --user USER  < capella-username >
    * -p PASSWORD, --password < capella-password >
    * -cpaddr CPADDR    https://api.cloud.couchbase.com
    * -orgid ORGID      < oid- url param >
    * -cbhost CBHOST    < server ip-addr >
    * -cbuser CBUSER    < server user-id >
    * -cbpassword CBPASSWORD < server user-password >
    * -cbport CBPORT        < query-service port:18093 >

3. to lookup server ip-addr:
from the capella connection string: `couchbases://<connection-string>`
```nslookup -type=SRV _couchbases._tcp.<connection-string>```
    - this is < server ip-addr >

4. create a database access:
   - this would be  < server user-id >,  < server user-password >

A sample usage:
```
# Create a new SQL file
echo "DECLARE
   x NUMBER := 0;
   counter NUMBER := 0;
BEGIN
   FOR i IN 1..4 LOOP
      x := x + 1000;
      counter := counter + 1;
      INSERT INTO temp VALUES (x, counter, 'in OUTER loop');
      --start an inner block 
      DECLARE
         x NUMBER := 0;  -- this is a local version of x
      BEGIN
         FOR i IN 1..4 LOOP
            x := x + 1;  -- this increments the local x
            counter := counter + 1;
            INSERT INTO temp VALUES (x, counter, 'inner loop');
         END LOOP;
      END;
   END LOOP;
   COMMIT;
END;
/
" > script.sql
```

```
./plsql-to-jsudf -u <capella-username> -p <capella-password> 
-cpaddr https://api.cloud.couchbase.com -orgid <oid> -cbhost <server ip-addr> 
 -cbuser <server user-id> -cbpassword <server user-password>  -cbport 18093 script.sql
```

### Or Setup Steps for building from the wheel:
1. create a new virtual environment ```python3 -m venv venv```
2. activate it: ```source venv/bin/activate```
3. create output directory: ```mkdir output```
4. install the distribution: ```pip3 install ./plsql_to_jsudf-0.0.6-py3-none-any.whl```

Usage:
1. ```holy-grail-camelot-scene -h```<br> tells you how the module is expected to be used, note that the optional arguments are *no longer optional* 😅 as we build from the wheel directly

2. expected arguments:
    *   -u USER, --user USER  < capella-username >
    * -p PASSWORD, --password < capella-password >
    * -cpaddr CPADDR    https://api.cloud.couchbase.com
    * -orgid ORGID      < oid- url param >
    * -cbhost CBHOST    < server ip-addr >
    * -cbuser CBUSER    < server user-id >
    * -cbpassword CBPASSWORD < server user-password >
    * -cbport CBPORT        < query-service port:18093 >

3. to lookup server ip-addr:
from the capella connection string: `couchbases://<connection-string>`
```nslookup -type=SRV _couchbases._tcp.<connection-string>```
    - this is < server ip-addr >

4. create a database access:
   - this would be  < server user-id >,  < server user-password >

A sample usage:
```
# Create a new SQL file
echo "DECLARE
   x NUMBER := 0;
   counter NUMBER := 0;
BEGIN
   FOR i IN 1..4 LOOP
      x := x + 1000;
      counter := counter + 1;
      INSERT INTO temp VALUES (x, counter, 'in OUTER loop');
      --start an inner block 
      DECLARE
         x NUMBER := 0;  -- this is a local version of x
      BEGIN
         FOR i IN 1..4 LOOP
            x := x + 1;  -- this increments the local x
            counter := counter + 1;
            INSERT INTO temp VALUES (x, counter, 'inner loop');
         END LOOP;
      END;
   END LOOP;
   COMMIT;
END;
/
" > script.sql
```

```
holy-grail-camelot-scene -u <capella-username> -p <capella-password> 
-cpaddr https://api.cloud.couchbase.com -orgid <oid> -cbhost <server ip-addr> 
 -cbuser <server user-id> -cbpassword <server user-password>  -cbport 18093 script.sql
```
