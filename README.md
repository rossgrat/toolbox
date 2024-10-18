# Ross' Engineering Toolbox

- TODO: Table of contents here

# Theory

## Client Design
### Filtering
When designing a data driven client, you may want to filter data. Filtering is easiest to think about in terms of sets. Selecting multiple filter fields can either result in the UNION (OR) of two underlying sets of data, or the INTERSECTION (AND).
  - Ex. - Selecting 'Group 1' and 'Football' can either return all users that are in Group 1 AND play Football, or all users that are in Group 1 OR play Football.

# Databases

## [Postgres](https://www.postgresql.org/docs/current/index.html)
### Quick Links
These are some quick links to postgres documentation that I find myself using often.
- [Roles and Privileges](https://www.postgresql.org/docs/17/sql-grant.html)
  [SELECT](https://www.postgresql.org/docs/17/sql-select.html)

### New Install
- Upon installing a new version of postgres the first thing you will need to to is create a role and a database for that role for whatever your username on your local machine is. Y
- You'll also probably want to make yourself a [superuser](https://www.postgresql.org/docs/current/role-attributes.html).
- You may additionally want to set a password for your role for use in database URLS such as `postgres://rossgrat:password@localhost:5432/test-database`
```
$ psql postgres
# CREATE ROLE rossgrat;
# CREATE DATABASE rossgrat;
# ALTER ROLE rossgrat WITH SUPERUSER;
# ALTER ROLE rossgrat WITH PASSWORD 'password';
```
- 

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

# Cloud Hosting

## AWS

### S3 Buckets
- When creating an S3 bucket, a bucket policy, found under the permissions tab for the bucket, is necessary for defining how the objects in the bucket are accessed. Trying to access data in a bucket without this policy will result in errors, most often 403 "AccessDenied" errors
