# Price Guardian

Automated price protection tracking.

Track prices and notify if price drops so you can get your well deserved refund (store price protection, visa price protection, etc.)

- backend up and running locally
  - Rust, actix, juniper, diesel, postgres
- frontend up and running locally
  - svelte, graphql, a component lib? snow/webpack?
- deploy backend
  - dockerize -> ecs
  - cloudformation to manage infra.. or terraform.. other tools?
  - deploy infra / code changes on merge to master? through CI? circleci?
- deploy frontend
  - cloudfront / s3

Some example apps:

- https://github.com/nemesiscodex/actix-blog-app
- https://github.com/jamesjmeyer210/actix_sqlx_mysql_user_crud
- https://github.com/marcusradell/monadium

General todos:

- logging

# Setup Instructions

Instructions are for a debian linux environment.

## Install Rust (latest stable)

Instructions taken from: https://www.rust-lang.org/tools/install

If you've never installed Rust:

```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

If you've installed Rust in the past:

```
$ rustup update
```

## Database setup

Install Postgresql and the trimmings:

```
$ sudo apt install postgresql postgresql-contrib postgresql-client libpq-dev
```

Install `diesel_cli`:

```
$ cargo install diesel_cli --no-default-features --features postgres
```

Setup up local trust authentication:

```
# find the conf file probably: /etc/postgresql/12/main/pg_hba.conf
$ sudo -u postgres psql
postgres=# SHOW hba_file;
sudo vim /etc/postgresql/12/main/pg_hba.conf
```

Open up the pg_hba.conf file and change every single METHOD to trust. For example:

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# Before:
local   all             all                                     peer

# After:
local   all             all                                     trust
```

`trust` means on your machine any os user can connect as any db user.

Create the price guardian super user:

```
sudo -u postgres createuser -s pg
```

Create the db and migrate to the latest and greatest:

```
diesel setup
diesel migration run
```

## Compile and Run

Install llvm, argonautica requires a c compiler:

```
apt-get install clang llvm-dev libclang-dev
```

Run the app:

```
cargo run
```
