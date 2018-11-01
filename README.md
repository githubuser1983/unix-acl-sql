# unix-acl-sql
This repository contains a sqlite-schema and an example database to handle unix-like acl (access control lists) hence file premissions for users

To use it, download the file "galois.DB" and then open sqlite3.
* >.read galois.db
* >select file_id,uid as user_id,path,can_write,can_read,can_execute from global_rights;
* >select file_id,uid as user_id,path,can_write,can_read,can_execute from global_rights where user_id=3 and file_id = 8;

Use-Cases:
This schema  could be used to develop REST-APIS where 
   * each URL like /galois/home/gauss is considered like a unix directory
   * each URL like /galois/models/iris.pfa is considered like a unix executable file

For exammple, I am developing a server which supports multi-users like in unix, where users can upload and deploy pfa (portable format for analytics) documents and execute those documents per REST-API:

==Rest-API==
* each url for example /galois/home/gauss/ without a model is considered like a directory in linux
* each url for example /galois/home/gauss/iris.pfa with a model is considered like an executable file in linux
* each url belongs to one owner and one group.
* for each url there are the following rights: read, write, execute for owner,group,others; like in linux
* to make it more appealing, there will be command line tools which are named like the linux tools and work like those, for example:
  mkdir, ls, rm, adduser, chown, chmod etc -> this way, everyone who knows something about linux, should be able to use the api without high learning curve
  * Models:
    * POST : Execute a model
    * PUT : Create or update a model
    * GET : Read a model, download it
    * DELETE : Delete a model
    * w = write = PUT & DELETE
    * r = read = GET
    * x = execute = POST (with some json send to the file, so that it can read the json-file and execute the resource based on the contents of the json-file)
  * Directories:
    * r = read = GET, list files and (direct) subdirectories of this directory
    * w = write = POST & DELETE & PUT
    * e = execute = can access files in this directory whose name is known
    * POST = write a model to this directory, if it does not exist. Must specify the variable filename in HTTP-Header, whose value is the name of the model
    * DELETE = delete this directory if it is empty
    * PUT = create an empty subdirectoy in this directory if it does not exist, if the directory exists do not do anything --> PUT is idempotent; Must specify the variable filename in HTTP-Header, whose value is the name of the subdirectory to be created.
  * PATCH not supported
