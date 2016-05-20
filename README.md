# PHPCraft-architecture

guide lines for PHPCraft `WORK IN PROGRESS`

## Premises
The following considerations apply primarily to a Linux + Apache + PHP environment

## Definitions
* __resource__: any file referent of an URI 
* __page__: a set of resources accessed through a route that displays and/or performs a piece of domain logic

## Principles
* __DRY__ (Do not Repeat Yourself): write every piece of information once
* __greedy__: do not load, include, define, instance any resource that is not going to be effectively used by the page

## Code organization

### Vertical Hierarchy (top down)
* __application__: the set of code that implements a certain domain logic
* __area__: a set of pages code into an application with at least one strong common trait  (i.e. behind login,  page layout)
* __subject__: a set of pages that performs operations about the same coherent portion of domain logic (i.e. products, customers)
* __action__: the set of operations performed by the page over the subject
 
An application contains at least one area, a area contains at least on subject, one subject contains at least one action

### Horizontal Criteria
#### Scope
* __private__: files protected (by .htaccess rules and/or login) from direct browser access
* __public__: files that can be requested by browser

#### Context
* __global__: all the code shared between different applications
* __application level__: all the code specific to an application

### Function
* __configuration__: file that store application informations tipically in key-value pairs (i.e. ini format) or json
* __lib__: libraries, (i.e. PHP or js) do not included in composer, both developed specifically for application or by third part
* __locale__: translations
* __templates__
* __src__: files that are source for specific tasks but are not to be directly used into application, such as sass .scss files

### Internazionalization
In context where files contain text to be exposed should be always used langauge specific folders and files. Folders should be nominated by language code (i.e. 'en' or 'en_UK'). Language codes should follow a [convention](https://en.wikipedia.org/wiki/Language_code).

## Filesystem
Folders should be organized according to vertical and horizontal logic as more convenient. An example for 'Acme' application:
* private
  * global
  * acme
*public

### Special Folders
* __secret__: files used for application developement and mantainence; folder is http protected
* __vendor__: composer folder for storing libraries

## .htacces
