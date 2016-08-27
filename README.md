# crates.io

Source code for the default registry for Cargo users. Can be found online at
[crates.io][crates-io]

[crates-io]: https://crates.io

## Installation

* `git clone` this repository
* `npm install`
* `npm run bower -- install`

## Making UI tweaks or changes

This website is built using [Ember.js](http://emberjs.com/) for the frontend,
which enables tweaking the UI of the site without actually having the server
running locally. To get up and running with just the UI, run:

```
npm run start:staging
```

This will give you a local server to browse while using the staging backend
(hosted on heroku at https://staging-crates-io.herokuapp.com).

If you'd like to run the server with a specific different backend, you can specify specific arguments to `npm start`. For example you can set the proxy to `https://crates.io/` to use the live instance, but do be aware that any modifications made here will be permanent! To do this, run:

```
npm start -- --proxy https://crates.io
```

The same is also available as:

```
npm run start:live
```

This requires NPM 2.0.

## Working on the backend

If you'd like to change the API server (the Rust backend), then the setup is a
little more complicated.

1. Copy the `.env.sample` file to `.env` and change any applicable values as
    directed by the comments in the file. Make sure the values in your new
    `.env` are exported in the shell you use for the following commands.

2. Set up the git index

    ```
    ./script/init-local-index.sh
    ```

    But *do not* modify your `~/.cargo/config` yet. Do that after step 3.

3. Build the server.

    ```
    cargo build
    ```

    On OS X 10.11, you will need to install the openssl headers first, and tell
    cargo where to find them. See https://github.com/sfackler/rust-openssl#osx.

4. Run the migrations

    ```
    ./target/debug/migrate
    ```

5. Run the servers

    ```
    # In one window, run the api server
    ./target/debug/server

    # In another window run the ember-cli server
    npm run start:local
    ```

## Running Tests

1. Configure the location of the test database. Note that this should just be a
   blank database, the test harness will ensure that migrations are run.

    ```
    export TEST_DATABASE_URL=...
    ```

2. Set the s3 bucket to `alexcrichton-test`. No actual requests to s3 will be
   made; the requests and responses are recorded in files in
   `tests/http-data` and the s3 bucket name needs to match the requests in the
   files.

    ```
    export S3_BUCKET=alexcrichton-test
    ```

3. Run the API server tests

    ```
    cargo test
    ```

4. Install [phantomjs](http://phantomjs.org/)

5. Run frontend tests

    ```
    ember test
    ember test --server
    ```

## Tools

For more information on using ember-cli, visit
[http://iamstef.net/ember-cli/](http://ember-cli.com/).

For more information on using cargo, visit
[doc.crates.io](http://doc.crates.io/).
