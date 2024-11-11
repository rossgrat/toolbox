# Ross' Engineering Toolbox

- TODO: Table of contents here

# Theory

## Programming Lanuages
### Compilers
- A compiler is a program that translates one programming lanuage into another programming language, usually, a higher level programming language is translated by a compiler to a lower level programming language, such as assembly or machine code
- When building a new programming language, the initial compiler for the language must be written using a different programming language. This is called [bootstrapping a compiler](https://en.wikipedia.org/wiki/Bootstrapping_(compilers)) Ex. the first compiler for Go was written in C. Once Go was stable, the Go compiler code that was written in C was rewritten in Go ([link](https://github.com/golang/go/tree/master/src/cmd/compile).

### Interpreted vs. Compiled
- Languages are often said to be either interpreted or compiled, but this is not strictly true. An **implementation** of a language can either be interpreted or compiled.
- Interpreted implementations of a language will move line by line through the language and transform it into some intermediary, platform agnostic [bytecode / p-code (portable code)](https://en.wikipedia.org/wiki/Bytecode). This bytecode can then either be run on a virtual machine ([portable code machine (p-code machine)](https://en.wikipedia.org/wiki/P-code_machine) that is specifically designed to execute the bytecode, OR compiled with [Just-In-Time (JIT)](https://en.wikipedia.org/wiki/Just-in-time_compilation) compilation into machine code and run directly on the host system.
  - Note that bytecode is distinct from an [instruction set architecture (ISA)](https://en.wikipedia.org/wiki/Instruction_set_architecture)

# Client Design
## Filtering
When designing a data driven client, you may want to filter data. Filtering is easiest to think about in terms of sets. Selecting multiple filter fields can either result in the UNION (OR) of two underlying sets of data, or the INTERSECTION (AND).
  - Ex. - Selecting 'Group 1' and 'Football' can either return all users that are in Group 1 AND play Football, or all users that are in Group 1 OR play Football.

# Databases

## [Postgres](https://www.postgresql.org/docs/current/index.html)
### Quick Links
These are some quick links to postgres documentation that I find myself using often.
- [Roles and Privileges](https://www.postgresql.org/docs/17/sql-grant.html)
- [SELECT](https://www.postgresql.org/docs/17/sql-select.html)

### New Install
Upon installing a new version of postgres the first thing you will need to to is create a role and a database for that role for whatever your username on your local machine is. You'll also probably want to make yourself a [superuser](https://www.postgresql.org/docs/current/role-attributes.html). You may additionally want to set a password for your role for use in database URLS such as `postgres://rossgrat:password@localhost:5432/test-database`. All of those things are taken care of in the below sequence of commands.
```
$ psql postgres
# CREATE ROLE rossgrat;
# CREATE DATABASE rossgrat;
# ALTER ROLE rossgrat WITH SUPERUSER;
# ALTER ROLE rossgrat WITH PASSWORD 'password';
```

### CLI
- You can exeute SQL code directly from the command line using `psql -c 'SELECT ...'`
- Create a dump of a database to your local machine using `pg_dump <database_url> > out.sql`
- Restore a local dump to a database using `psql <database_url> < out.sql`

### SQL
- You can make a copy of a database directly on a postgres server instance using:
    ```sql
        CREATE DATABASE new_database WITH TEMPLATE old_database;
    ```
- You can remove all connections except your own from a database using:
    ```sql
    SELECT pg_terminate_backend(pg_stat_activity.pid)
    FROM pg_stat_activity
    WHERE pg_stat_activity.datname=<database_name>
    AND pid != pg_backend_pid()
    ```

# Programming Languages

## General
- Many languages have getters and setters that allow class variables to execute functions when they are updated. This can be useful for dynamically changing variables or updating. Examples in Typescript and Swift are below.
    ```typescript
    // Typescript
    class Person {
        firstName: string
        lastName: string
        get fullName() {
            return this.firstName + ' ' + this.lastName
        }
        set fullName(fullName: string) {
            let strs = fullName.split(' ')
            if (strs.length > 0) {
                this.firstName = strs[0]
            }
            if (strs.length > 1) {
                this.lastName = strs[1]
            }
        }
    }
    ```
    ```swift
    // Swift
    class Person {
        var firstName: String
        var lastName: String
        var fullName: String {
            get {
                return this.firstName + " " + this.lastName
            }
            set {
                ...
            }
        }
    }
    ```

  ## Go
  - Go is organized by modules, here are some helpful links for organizing Go projects
      - [Creating and using modules](https://go.dev/blog/using-go-modules)
      - [Calling a module from another module](https://go.dev/doc/tutorial/call-module-code)
      - [Module import paths](https://pkg.go.dev/cmd/go#hdr-Remote_import_paths)
      - [Ideas for a Go project structure](https://eli.thegreenplace.net/2019/simple-go-project-layout-with-modules/)

# Cloud Hosting

## AWS

### S3 Buckets
- When creating an S3 bucket, a bucket policy, found under the permissions tab for the bucket, is necessary for defining how the objects in the bucket are accessed. Trying to access data in a bucket without this policy will result in errors, most often 403 "AccessDenied" errors
