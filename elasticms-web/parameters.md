# Available environment variables

The environment variables have been grouped by bundles and for the Symfony framework itself.

## Symfony variables

### APP_ENV

[Possible values](https://symfony.com/doc/current/configuration.html#selecting-the-active-environment): `dev`, `prod`, `redis`, `dev`, `test`
 - Example `APP_ENV=dev`
 
But there is 2 more possible values, specific to elasticms:

 - `db` : It's equivalent to a `prod` environment, but PHP sessions are persisted in the RDBMS (does not work with SQLite databases).
 - `redis` : It's equivalent to a `prod` environment, but PHP sessions are saved in a Redis server.
- `store_data` : It's equivalent to a `prod` environment, but PHP sessions are saved in [Store Data services](../recipes/store-data.md).

### APP_SECRET

A secret seed.
 - Example `APP_SECRET=7b19a4a6e37b9303e4f6bca1dc6691ed`

### HTTP_CLIENT_MAX_CONNECTIONS

Define the max connections to the API client, when using async calls. By default it is set to `4`.

### Behind a Load Balancer or a Reverse Proxy

```dotenv
TRUSTED_PROXIES=127.0.0.1,127.0.0.2
TRUSTED_HOSTS=localhost,example.com
HTTP_CUSTOM_FORWARDED_PROTO=HTTP_X_COMPANY_FORWARDED_PROTO#Default value HTTP_X_FORWARDED_PROTO
HTTP_CUSTOM_FORWARDED_PORT=HTTP_X_COMPANY_FORWARDED_PORT#Default value HTTP_X_FORWARDED_PORT
HTTP_CUSTOM_FORWARDED_FOR=HTTP_X_COMPANY_FORWARDED_FOR#Default value HTTP_X_FORWARDED_FOR
HTTP_CUSTOM_FORWARDED_HOST=HTTP_X_COMPANY_FORWARDED_HOST#Default value HTTP_X_FORWARDED_HOST
```

If the reverse proxy's IP change all the time:

```dotenv
TRUSTED_PROXIES=127.0.0.1,REMOTE_ADDR
TRUSTED_HOSTS=localhost,example.com
HTTP_CUSTOM_FORWARDED_PROTO=HTTP_X_COMPANY_FORWARDED_PROTO#Default value HTTP_X_FORWARDED_PROTO
HTTP_CUSTOM_FORWARDED_PORT=HTTP_X_COMPANY_FORWARDED_PORT#Default value HTTP_X_FORWARDED_PORT
HTTP_CUSTOM_FORWARDED_FOR=HTTP_X_COMPANY_FORWARDED_FOR#Default value HTTP_CUSTOM_FORWARDED_FOR
HTTP_CUSTOM_FORWARDED_HOST=HTTP_X_COMPANY_FORWARDED_HOST#Default value HTTP_CUSTOM_FORWARDED_HOST
```

## Swift Mailer

### MAILER_URL
Configure [Swift Mailer](https://symfony.com/doc/current/email.html#configuration)


## Doctrine variables

Default values (sqlite): 
```dotenv
DB_DRIVER='sqlite'
DB_USER='user'
DB_PASSWORD='user'
DB_PORT='1234'
DB_NAME='app.db'
```

### DB_HOST

DB's host. 
 - Default value: `127.0.0.1`
 - Example: `DB_DRIVER='db-server.tl'`
 
### DB_DRIVER

Driver (Type of the DB server). Accepted values are `mysql`, `pgsql` and `sqlite`
 - Default value: `mysql`
 - Example: `DB_DRIVER='pgsql'`
  
### DB_USER

 - Default value `user`
 - Example: `DB_USER='demo'`
  
### DB_PASSWORD

 - Default value `user`
 - Example: `DB_PASSWORD='password'`
  
### DB_PORT

For information the default mysql/mariadb port is 3306 and 5432 for Postgres
 - Default value `3306`
 - Example: `DB_PORT='5432'`
  
### DB_NAME

 - Default value `elasticms`
 - Example: `DB_NAME='demo'`
  
### DB_SCHEMA

This variable is not used by Doctrine but by the dump script with postgres in the docker image of elasticms. 
 - Default value: not defined
 - Example: `DB_SCEMA='schema_demo_adm'`
 
### DB_CONNECTION_TIMEOUT

Usefull when connecting to a string of multiple hosts. To reduce timeout when checking a second host if the first host fails.
The minimum value is 2 https://pracucci.com/php-pdo-pgsql-connection-timeout.html
 - Default value `30`
 - Example: `DB_CONNECTION_TIMEOUT=30`


## Redis
Should be defined only if Redis is defined as session manager.
```dotenv
REDIS_HOST=localhost
REDIS_PORT=6379
```

## Elasticms Client Helper Bundle variables

### EMSCH_LOCALES

List of available locales supported by the client/channels i.e.: `EMSCH_LOCALES=["en","fr","nl"]`

### EMSCH_INSTANCE_ID

Define the list of project's index prefixes, separated by a `|` i.e. `='demo_pgsql_v1_'`, By default it sets to the EMSCO_INSTANCE_ID value.

### EMSCH_TRANSLATION_TYPE

Define the translation content type name. Default value `label` i.e. `EMSCH_TRANSLATION_TYPE='label'`

### EMSCH_ROUTE_TYPE

Define the route content type name. Default value `route` i.e. `EMSCH_ROUTE_TYPE='route'`

### EMSCH_TEMPLATES

Define the template content type structure. Default value `{"template": {"name": "name","code": "body"}}` i.e. `EMSCH_TEMPLATES='{"template": {"name": "label","code": "body"}}'`

### EMSCH_ASSET_LOCAL_FOLDER

Specify a local folder (in the public folder) where to locate `emsch` assets. This is useful in development mode as the zip containing the assets is ignored.
Example base template.
```twig
<link rel="stylesheet" href="{{ asset('css/app.css', 'emsch') }}">
```

### EMSCH_LOCAL_PATH

Overwrite the destination of the local files, by default `emsch:local:*` commands will search in `local/%environment_alias%` folder.

Example for locally loading the demo inside local elasticms-web.
```.dotenv
EMSCH_LOCAL_PATH='../demo/skeleton'
```

### EMSCH_SEARCH_LIMIT

Specify the maximum number of expected document for template, translation and route content types. Default value `1000`

## Elasticms Common Bundle variables

### EMS_ELASTICSEARCH_HOSTS

Define the elasticsearch cluster as an array (JSON encoded) of hosts:
- Default value: EMS_ELASTICSEARCH_HOSTS='["http://localhost:9200"]'

If needed, this variable can also contain an [elastica servers array](https://elastica-docs.readthedocs.io/en/latest/client.html#client-configurations):

```dotenv
EMS_ELASTICSEARCH_HOSTS='[{"transport":"Https","host":"elastic:fewl13@localhost","port":9200,"curl":{"64":false}}]'
```
In this example the cluster contains only one host accessible via HTTPS on the port 9200. But with the CURL option `"64": false` the client doesn't check the validity of the host certificate

```dotenv
EMS_ELASTICSEARCH_HOSTS='[{"transport":"Https","host":"elastic:fewl13@localhost","port":9200,"curl":{"10065":"/opt/local/cacert.pem"}}]'
```
Here the client uses the `/opt/local/cacert.pem` to validate the server certificate.


```dotenv
EMS_ELASTICSEARCH_HOSTS='[{"transport":"Https","host":"localhost","port":9200,"headers":{"Authorization":"Basic ZWxhc3RpYzpmZXdsMTM="},"curl":{"64":false}}]'
```
Another example with an extra HTTP header.

[All PHP CURL integer identifier can be found on GitHub](https://github.com/JetBrains/phpstorm-stubs/blob/master/curl/curl_d.php). More info on [PHP.net](https://www.php.net/manual/en/function.curl-setopt.php).

### EMS_ELASTICSEARCH_CONNECTION_POOL

Define the [elasticsearch sniffing strategy](https://www.elastic.co/guide/en/elasticsearch/client/php-api/7.17/connection_pool.html:
- Default value: EMS_ELASTICSEARCH_CONNECTION_POOL='Elasticsearch\\ConnectionPool\\SimpleConnectionPool' if the EMS_ELASTICSEARCH_HOSTS contains one and only one host configuration; in order to avoid sniffing requests on a cluster that is more likely behind a reverse proxy. Else it contains EMS_ELASTICSEARCH_CONNECTION_POOL='Elasticsearch\\ConnectionPool\\SniffingConnectionPool'.
- Possible values:
    - EMS_ELASTICSEARCH_CONNECTION_POOL='Elasticsearch\\ConnectionPool\\SimpleConnectionPool'
    - EMS_ELASTICSEARCH_CONNECTION_POOL='Elasticsearch\\ConnectionPool\\SniffingConnectionPool'
    - EMS_ELASTICSEARCH_CONNECTION_POOL='Elasticsearch\\ConnectionPool\\StaticConnectionPool'
    - EMS_ELASTICSEARCH_CONNECTION_POOL='Elasticsearch\\ConnectionPool\\StaticNoPingConnectionPool'

### EMS_STORAGES

Used to define storage services. Elasticms supports [multiple types of storage services](https://github.com/ems-project/EMSCommonBundle/blob/master/src/Resources/doc/storages.md). 
- Default value: `EMS_STORAGES='[{"type":"fs","path":".\/var\/assets"},{"type":"s3","credentials":[],"bucket":""},{"type":"db","activate":false},{"type":"http","base-url":"","auth-key":""},{"type":"sftp","host":"","path":"","username":"","public-key-file":"","private-key-file":""}]'`
- Example: `EMS_STORAGES='[{"type":"fs","path":"./var/assets"},{"type":"fs","path":"/var/lib/elasticms"}]'`

### EMS_HASH_ALGO

Refers to the [PHP hash_algos](https://www.php.net/manual/fr/function.hash-algos.php) function. Specify the algorithms to used in order to hash and identify files. It's also used to hash the document indexed in elasticsearch.
- Default value: EMS_HASH_ALGO='sha1'

### EMS_BACKEND_URL

Define backend elasticms url. CommonBundle provides a CoreApi instance.

### EMS_BACKEND_API_KEY

Define backend authentication token. The commonBundle coreApi instance becomes authenticated.

### EMS_CORE_API_HEADERS

Define extra headers for the API client, helpful for adding cookie header for xdebug.

```dotenv
EMS_CORE_API_HEADERS='{"Cookie":"XDEBUG_SESSION=PHPSTORM"}'
```

### EMS_BACKEND_API_TIMEOUT

Adjust the API client's timeout. By default is set to `30` seconds, if you API request may take longueur (e.g. during migration) you can increase the timeout :

### EMS_BACKEND_API_VERIFY

Define the `verify_host` and `verify_peer` for the api client, by default it is set to `true`.

### EMS_CACHE

Define the ems cache type. Default value `file_system`. 
Allowed values: `file_system`, `apc` and `redis`. 

### EMS_CACHE_PREFIX

Unique required value per project, otherwise wipe storage will clear all cached values. 

### EMS_REDIS_HOST

Use a different redis host for the common cache service. Default REDIS_HOST env variable.

### EMS_REDIS_PORT

Use a different redis port for the common cache service. Default REDIS_PORT env variable.

### EMS_METRIC_ENABLED

Default value `false`, if true `/metrics` is added to the routes.

### EMS_METRIC_HOST

Default value empty, symfony route host pattern for allow hosting on /metrics

### EMS_METRIC_PORT

Default value null, if defined will check the SERVER_PORT and throw 404 if not matching

### EMS_SLUG_SYMBOL_MAP

Specify replacement strings, per locale to symbols. E.g. if you want to replace the symbol `@` by the string `at` in your slug in English and French : `{"en":{"@":"at"},"fr":{"@":"at"}}`. Default value `~` ([rely on the default Symfony configuration](https://github.com/symfony/string/blob/f5832521b998b0bec40bee688ad5de98d4cf111b/Slugger/AsciiSlugger.php#L59C42-L61C6))

### EMS_STORE_DATA_SERVICES

Define (JSON format) the store data services, in the priority order. See the [Stora Data documentation](../recipes/store-data.md) for more details. By default, the store data functionalities are disabled.

### EMS_TRUSTED_IPS

Define (JSON format) a range of allowed ip address. Example: `["127.0.0.1", "::1", "192.168.0.1/24"]`
Important the route `/metrics` is not filtered.

### EMS_EXCLUDED_CONTENT_TYPES

Define (JSON format) a list of content type names to exclude from admin backup/restore commands. Example: `["route","template","template_ems","label"]`. Default value `[]`

### EMS_ELASTICSEARCH_PROXY_API

Bollean variable, if specified to `true` all elasticsearch query will be delegated to the admin api. And then you'll need to login on an admin first via the `ems:admin:login` command. By default, this variable is set to `false`.

## Elasticms Form Bundle variables

### EMSF_HASHCASH_DIFFICULTY
Define the [hashcash difficuty](https://github.com/ems-project/EMSFormBundle/blob/master/doc/config.md#hashcash-difficulty) for the form bundle. Set to `16384` by default.


### EMSF_ENDPOINTS
Define the [endpoints](https://github.com/ems-project/EMSFormBundle/blob/master/doc/config.md#endpoints) for the form bundle. Set to `[]` by default.


### EMSF_LOAD_FROMJSON
Define the [load form JSON](https://github.com/ems-project/EMSFormBundle/blob/master/doc/config.md#load-from-json) for the form bundle. Set to `false` by default.


### EMSF_CACHEABLE
Define the [cacheable](https://github.com/ems-project/EMSFormBundle/blob/master/doc/config.md#cacheable) for the form bundle. Set to `true` by default.

### EMSF_TYPE
Define the [type](https://github.com/ems-project/EMSFormBundle/blob/master/doc/config.md#type) for the form bundle. Set to `form_instance` by default.

## Elasticms Submission Bundle variables

### EMSS_CONNECTIONS
Define the [connections](https://github.com/ems-project/EMSSubmissionBundle/blob/master/src/Resources/doc/index.md#connections-) for the submission bundle. Set to `[]` by default.

### EMSS_DEFAULT_TIMEOUT
Define the [default timeout](https://github.com/ems-project/EMSSubmissionBundle/blob/master/src/Resources/doc/index.md#default-timeout) for the submission bundle. Set to `10` by default.

## Deprecated variables

## Since version 5.14.0
- EMS_WEBALIZE_REMOVABLE_REGEX : See [EMS_SLUG_SYMBOL_MAP](#EMS_SLUG_SYMBOL_MAP))
- EMS_WEBALIZE_DASHABLE_REGEX : See [EMS_SLUG_SYMBOL_MAP](#EMS_SLUG_SYMBOL_MAP))
