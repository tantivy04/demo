# Requirement
1. JS
2. ES6
https://github.com/mbeaudru/modern-js-cheatsheet

# Loopback 3:
1. Datasouce
2. Model, validation
3. Model.json, model-config.json
4. Remote method
Ex: POST /Users/login => User.login
4.2 Query data https://loopback.io/doc/en/lb3/Querying-data.html

5. ACL
6. Hook:
  + Remote method
  + operate
9. Boot script

7. Mixins
8. Middleware.

10. Environment
11. Transaction.


# Poplular modules
1. https://caolan.github.io/async/index.html: parallel, series, auto, waterfall,...
2. lodash
3. debug
4. pm2 cluster.

## Unit test: mocha


# Exercise:
Create a project REST API using loopback 3 to manage Company, User and Project.
1. [Model.json] Company:
- id: uuid
- name
- created at (date)
- modified at (date)
- created by (user)
- modified by (user)

2. [Model.json] User
- username
- email
- password
- firstName
- noOfProjects
- lastName
- companyId
- created at (date)
- modified at (date)
- created by (user)
- modified by (user)

3. [Model.json] Project
- name
- companyId
- location (GEO point - lat, lng)
- created at (date)
- modified at (date)
- created by (user)
- modified by (user)

*** Entity Relation
- [Model.json: foreign Key] A user only belongs to 1 Company. 1 Company has many user.
- [Role, RoleMapping] A user can have many role.

*** Project Features
* [PersisModel] CRUD (create, read, update, delete) Project, User and Company. Additional:
- User:
  + [RemoteMethod] EXCUTE: (clientadmin) assign User to belongs to a Company.
  + [RemoteMethod] READ: get list project of logged user.
  + [hook afterRemote] after login, should response his role name.
  + [hook before/after save/delete and transaction] user.noOfProjects will be auto increase/decrease while creating/deleting a Project.

- Company: 
  + [RemoteMethod] EXECUTE: (clientadmin) assign a user to be an employee of a company
  + [RemoteMethod] READ: get list project of company

- Project
  + [hook before save] auto save companyId if creator is clientadmin user.

* [Mixins] Auto save 4 fields: createdAt, modifiedAt, createdBy and modifiedBy.
* [Boot script - hook before all remotes] Auto response JSON should follow the structure:
- Success (HTTP 200)
{
  "data": any // string, object, number
}
- [Middleware] Error (HTTP 400, 404,... or 500)
{
  "error": {
     // error normal fields: name, statusCode,...
     code: string // this is the error code we defined for UI or mobile using to show error message.
  }
}

* [ACL and custom permission] Roles:
- admin: can access all APIs
- clientadmin: only READ, WRITE, EXECUTE on his data at: Company and Project.
- editor of a company: only READ, WRITE his company and his project.
- worker of a company: only READ his Company and his Project belongs to Company.
- unathenticated: deny access all APIs.

* [Environment] development and staging.






