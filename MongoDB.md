## WAL-G for MongoDB

**Interface of Mongo now is unstable**

You can use wal-g as a tool for encrypting, compressing Mongo backups and push/fetch them to/from storage without saving it on your filesystem.

Development
-----------
### Installing
To compile and build the binary for Mongo:

```
go get github.com/wal-g/wal-g
cd $GOPATH/src/github.com/wal-g/wal-g
make install
make deps
make mongo_build
```
Users can also install WAL-G by using `make install`. Specifying the GOBIN environment variable before installing allows the user to specify the installation location. On default, `make install` puts the compiled binary in `go/bin`.
```
export GOBIN=/usr/local/bin
cd $GOPATH/src/github.com/wal-g/wal-g
make install
make deps
make mongo_install
```

Configuration
-------------

* `WALG_MONGO_OPLOG_END_TS`

To set time [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) for recovery point.

* `WALG_MONGO_OPLOG_DST`

To place oplogs in the specified directory during stream-fetch.

Usage
-----

WAL-G mongo extension currently supports these commands:

* ``stream-fetch``

When fetching backup's stream, the user should pass in the name of the backup. It returns an encrypted data stream to stdout, you should pass it to a backup tool that you used to create this backup.
```
wal-g stream-fetch example_backup | mongorestore --archive --oplogReplay
```
WAL-G can also fetch the latest backup using:

```
wal-g stream-fetch LATEST | mongorestore --archive --oplogReplay
```

* ``stream-push``

Command for compressing, encrypting and sending backup from stream to storage.

```
wal-g stream-push
```

Variable _WALG_STREAM_CREATE_COMMAND_ is required for use stream-push 
(eg. ```mongodump --archive --oplog```)

* ``oplog-push``

Command for sending oplogs to storage by CRON.

```
wal-g oplog-push
```
