# Minio Client Quickstart Guide

Minio Client (mc) provides a modern alternative to UNIX commands like ls, cat, cp, mirror, diff etc. It supports filesystems and Amazon S3 compatible cloud storage service (AWS Signature v2 and v4).

```sh
ls            List files and folders.
mb            Make a bucket or folder.
cat           Display contents of a file.
pipe          Write contents of stdin to target. When no target is specified, it writes to stdout.
share         Generate URL for sharing.
cp            Copy one or more objects to a target.
mirror        Mirror folders recursively from a single source to single destination.
diff          Compute differences between two folders.
rm            Remove file or bucket [WARNING: Use with care].
policy	      Set public policy on bucket or prefix.
session       Manage saved sessions of cp and mirror operations.
config        Manage configuration file.
update        Check for a new software update.
version       Print version.
```

## 1.  Download Minio Client

| Platform | Architecture | URL |
| ---------- | -------- |------|
|GNU/Linux|64-bit Intel|https://dl.minio.io/client/mc/release/linux-amd64/mc|
||32-bit Intel|https://dl.minio.io/client/mc/release/linux-386/mc|
||32-bit ARM|https://dl.minio.io/client/mc/release/linux-arm/mc|
|Apple OS X|64-bit Intel|https://dl.minio.io/client/mc/release/darwin-amd64/mc|
|Microsoft Windows|64-bit|https://dl.minio.io/client/mc/release/windows-amd64/mc.exe|
||32-bit|https://dl.minio.io/server/minio/release/windows-386/minio.exe|
|FreeBSD|64-bit|https://dl.minio.io/client/mc/release/freebsd-amd64/mc|
|Solaris/Illumos|64-bit|https://dl.minio.io/client/mc/release/solaris-amd64/mc|

### Install from Source

Source installation is intended only for developers and advanced users. `mc update` command does not support upgrading from source based installation. Please download official releases from https://minio.io/downloads/#minio-client.

If you do not have a working Golang environment, please follow [How to install Golang](./INSTALLGO.md).

```sh
$ go get -u github.com/minio/mc
```

## 2. Run Minio Client

### 1. GNU/Linux

```sh
$ chmod +x mc
$ ./mc --help
```

### 2. OS X

```sh
$ chmod 755 mc
$ ./mc --help
```

### 3. Microsoft Windows

```sh
C:\Users\Username\Downloads> mc.exe --help
```

### 4. Solaris/Illumos

```sh
$ chmod 755 mc
$ ./mc --help
```

### 5. FreeBSD

```sh
$ chmod 755 mc
$ ./mc --help
```

## 3. Add a Cloud Storage Service
Note: If you are planning to use `mc` only on POSIX compatible filesystems, you may skip this step and proceed to **Step 4**.

To add one or more Amazon S3 compatible hosts, please follow the instructions below. `mc` stores all its configuration information in ``~/.mc/config.json`` file.

#### Usage

```sh
mc config host add <ALIAS> <YOUR-S3-ENDPOINT> <YOUR-ACCESS-KEY> <YOUR-SECRET-KEY> <API-SIGNATURE>
```

Alias is simply a short name to you cloud storage service. S3 end-point, access and secret keys are supplied by your cloud storage provider. API signature is an optional argument. By default, it is set to "S3v4".

### Example - Minio Cloud Storage

Minio server displays URL, access and secret keys.


```sh
$ mc config host add minio http://192.168.1.51 BKIKJAA5BMMU2RHO6IBB V7f1CwQqAcwo80UEIJEjc5gVQUSSx5ohQ9GSrr12 S3v4
```

### Example - Amazon S3 Cloud Storage

Get your AccessKeyID and SecretAccessKey by following [AWS Credentials Guide](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSGettingStartedGuide/AWSCredentials.html).

```sh
$ mc config host add s3 https://s3.amazonaws.com BKIKJAA5BMMU2RHO6IBB V7f1CwQqAcwo80UEIJEjc5gVQUSSx5ohQ9GSrr12 S3v4
```

### Example - Google Cloud Storage

Get your AccessKeyID and SecretAccessKey by following [Google Credentials Guide](https://cloud.google.com/storage/docs/migrating?hl=en#keys)

```sh
$ mc config host add gcs  https://storage.googleapis.com BKIKJAA5BMMU2RHO6IBB V8f1CwQqAcwo80UEIJEjc5gVQUSSx5ohQ9GSrr12 S3v2
```

NOTE: Google Cloud Storage only supports Legacy Signature Version 2, so you have to pick - S3v2

## 4. Test Your Setup

`mc` is pre-configured with https://play.minio.io:9000, aliased as "play". It is a hosted Minio server for testing and development purpose.  To test Amazon S3, simply replace "play" with "s3" or the alias you used at the time of setup.

*Example:*

List all buckets from https://play.minio.io:9000

```sh
$ mc ls play
[2016-03-22 19:47:48 PDT]     0B my-bucketname/
[2016-03-22 22:01:07 PDT]     0B mytestbucket/
[2016-03-22 20:04:39 PDT]     0B mybucketname/
[2016-01-28 17:23:11 PST]     0B newbucket/
[2016-03-20 09:08:36 PDT]     0B s3git-test/
```

## 5. Everyday Use

You may add shell aliases to override your common Unix tools.

```sh
alias ls='mc ls'
alias cp='mc cp'
alias cat='mc cat'
alias mkdir='mc mb'
alias pipe='mc pipe'
```

## 6. Global Options

### Option [--debug]
Debug option enables debug output to console. 

*Example: Display verbose debug output for `ls` command.* 

```sh
$ mc --debug ls play
mc: <DEBUG> GET / HTTP/1.1
Host: play.minio.io:9000
User-Agent: Minio (darwin; amd64) minio-go/1.0.1 mc/2016-04-01T00:22:11Z
Authorization: AWS4-HMAC-SHA256 Credential=**REDACTED**/20160408/us-east-1/s3/aws4_request, SignedHeaders=expect;host;x-amz-content-sha256;x-amz-date, Signature=**REDACTED**
Expect: 100-continue
X-Amz-Content-Sha256: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
X-Amz-Date: 20160408T145236Z
Accept-Encoding: gzip

mc: <DEBUG> HTTP/1.1 200 OK
Transfer-Encoding: chunked
Accept-Ranges: bytes
Content-Type: text/xml; charset=utf-8
Date: Fri, 08 Apr 2016 14:54:55 GMT
Server: Minio/DEVELOPMENT.2016-04-07T18-53-27Z (linux; amd64)
Vary: Origin
X-Amz-Request-Id: HP30I0W2U49BDBIO

mc: <DEBUG> Response Time:  1.220112837s

[...]

[2016-04-08 03:56:14 IST]     0B albums/
[2016-04-04 16:11:45 IST]     0B backup/
[2016-04-01 20:10:53 IST]     0B deebucket/
[2016-03-28 21:53:49 IST]     0B guestbucket/
```

### Option [--json]

JSON option enables parseable output in JSON format.

*Example: List all buckets from Minio play service.*

```sh
$ mc --json ls play
{"status":"success","type":"folder","lastModified":"2016-04-08T03:56:14.577+05:30","size":0,"key":"albums/"}
{"status":"success","type":"folder","lastModified":"2016-04-04T16:11:45.349+05:30","size":0,"key":"backup/"}
{"status":"success","type":"folder","lastModified":"2016-04-01T20:10:53.941+05:30","size":0,"key":"deebucket/"}
{"status":"success","type":"folder","lastModified":"2016-03-28T21:53:49.217+05:30","size":0,"key":"guestbucket/"}
```

### Option [--no-color]

This option disables the color theme. It useful for dumb terminals.

### Option [--quiet]

Quiet option suppress chatty console output.

### Option [--config-folder]

Use this option to set a custom config path. 

## 7. Commands

|   |   | |
|:---|:---|:---|
|[**ls** - List buckets and objects](#ls)   |[**mb** - Make a bucket](#mb)   | [**cat** - Concatenate an object](#cat)  |
|[**cp** - Copy objects](#cp)   | [**rm** - Remove objects](#rm)  | [**pipe** - Pipe to an object](#pipe)  |
| [**share** - Share access](#share)  |[**mirror** - Mirror buckets](#mirror)   |[**diff** - Diff buckets](#diff)   |
|[**policy** - Set public policy on bucket or prefix](#policy)   |[**session** - Manage saved sessions](#session)   | [**config** - Manage config file](#config)  |
| [**update** - Manage software updates](#update)  | [**version** - Show version](#version)  |   |

<a name="ls"> </a>

###  Command `ls` - List Objects

`ls` command lists files, objects and objects. Use `--incomplete` flag to list partially copied content.

```sh
USAGE:
   mc ls [FLAGS] TARGET [TARGET ...]

FLAGS:
  --help, -h			Help of ls.
  --recursive, -r		List recursively.
  --incomplete, -I		Remove incomplete uploads.
```

*Example: List all buckets on https://play.minio.io:9000.*

```sh
$ mc ls play
[2016-04-08 03:56:14 IST]     0B albums/
[2016-04-04 16:11:45 IST]     0B backup/
[2016-04-01 20:10:53 IST]     0B deebucket/
[2016-03-28 21:53:49 IST]     0B guestbucket/
[2016-04-08 20:58:18 IST]     0B mybucket/
```
<a name="mb"></a>
### Command `mb` - Make a Bucket

`mb` command creates a new bucket on an object storage. On a filesystem, it behaves like `mkdir -p` command. Bucket is equivalent of a drive or mount point in filesystems and should not be treated as folders. Minio does not place any limits on the number of buckets created per user. 
On Amazon S3, each account is limited to 100 buckets. Please refer to [Buckets Restrictions and Limitations on S3](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) for more information.  

```sh
USAGE:
   mc mb [FLAGS] TARGET [TARGET...]

FLAGS:
  --help, -h              Help of mb.
  --region "us-east-1"    Specify bucket region. Defaults to ‘us-east-1’.
```

*Example: Create a new bucket named "mybucket" on https://play.minio.io:9000.*


```sh
$ mc mb play/mybucket
Bucket created successfully ‘play/mybucket’.
```

<a name="cat"></a>

### Command `cat` - Concatenate Objects

`cat` command concatenates contents of a file or object to another. You may also use it to simply display the contents to stdout

```sh
USAGE:
   mc cat [FLAGS] SOURCE [SOURCE...]

FLAGS:
  --help, -h					Help of cat
```

*Example: Display the contents of a text file `myobject.txt`*

```sh
$ mc cat play/mybucket/myobject.txt
Hello Minio!!
```
<a name="pipe"></a>
### Command `pipe` - Pipe to Object
``pipe`` command copies contents of stdin to a target. When no target is specified, it writes to stdout.

```sh
USAGE:
   mc pipe [FLAGS] [TARGET]

FLAGS:
  --help, -h					Help of pipe.
```

*Example: Stream MySQL database dump to Amazon S3 directly.*

```sh
$ mysqldump -u root -p ******* accountsdb | mc pipe s3/ferenginar/backups/accountsdb-oct-9-2015.sql
```

<a name="cp"></a>
### Command `cp` - Copy Objects
`cp` command copies data from one or more sources to a target.  All copy operations to object storage are verified with MD5SUM checksums. Interrupted or failed copy operations can be resumed from the point of failure.

```sh
USAGE:
   mc cp [FLAGS] SOURCE [SOURCE...] TARGET
   
FLAGS:
  --help, -h				Help of cp.
  --recursive, -r			Copy recursively.
```

*Example: Copy a text file to to an object storage.*

```sh
$ mc cp myobject.txt play/mybucket
myobject.txt:    14 B / 14 B  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 41 B/s 0
```
<a name="rm"></a>
### Command `rm` - Remove Buckets and Objects
Use `rm` command to remove file or bucket

```sh
USAGE:
   mc rm [FLAGS] TARGET [TARGET ...]

FLAGS:
  --help, -h			Help of rm.
  --recursive, -r		Remove recursively.
  --force			Force a dangerous remove operation.
  --incomplete, -I		Remove an incomplete upload(s).
  --fake		        Perform a fake remove operation.
```

*Example: Remove a single object.*

```sh
$ mc rm play/mybucket/myobject.txt
Removed ‘play/mybucket/myobject.txt’.
```

*Example: Recursively remove a bucket and all its contents. Since this is a dangerous operation, you must explicitly pass `--force` option.*

```sh
$ mc rm --recursive --force play/myobject
Removed ‘play/myobject/newfile.txt’.
Removed 'play/myobject/otherobject.txt’.
```

*Example: Remove all incompletely uploaded files from `mybucket`.*

```sh
$ mc rm  --incomplete --recursive --force play/mybucket
Removed ‘play/mybucket/mydvd.iso’.
Removed 'play/mybucket/backup.tgz’.
```

<a name="share"></a>
### Command `share` - Share Access

`share` command securely grants upload or download access to object storage. This access is only temporary and it is safe to share with remote users and applications. If you want to grant permanent access, you may look at `mc policy` command instead.

Generated URL has access credentials encoded in it. Any attempt to tamper the URL will invalidate the access. To understand how this mechanism works, please follow [Pre-Signed URL](http://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html) technique.

```sh
USAGE:
   mc share [FLAGS] COMMAND

FLAGS:
  --help, -h					Help of share.
  
COMMANDS:
   download	  Generate URLs for download access.
   upload	  Generate ‘curl’ command to upload objects without requiring access/secret keys.
   list		  List previously shared objects and folders.
```

### Sub-command `share download` - Share Download

`share download` command generates URLs to download objects without requiring access and secret keys. Expiry option sets the maximum validity period (no more than 7 days), beyond which the access is revoked automatically.

```sh
USAGE:
   mc share download [OPTIONS] TARGET [TARGET...]

OPTIONS:
  --help, -h				Help of share download
  --recursive, -r			Share all objects recursively.
  --expire, -E "168h"			Set expiry in NN[h|m|s].
```

*Example: Grant temporary access to an object with 4 hours expiry limit.*

```sh
$ mc share download --expire 4h play/mybucket/myobject.txt
URL: https://play.minio.io:9000/mybucket/myobject.txt
Expire: 0 days 4 hours 0 minutes 0 seconds
Share: https://play.minio.io:9000/mybucket/myobject.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=Q3AM3UQ867SPQQA43P2F%2F20160408%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20160408T182008Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=1527fc8f21a3a7e39ce3c456907a10b389125047adc552bcd86630b9d459b634
```

#### Sub-command `share upload` - Share Upload

`share upload` command generates a ‘curl’ command to upload objects without requiring access/secret keys. Expiry option sets the maximum validity period (no more than 7 days), beyond which the access is revoked automatically. Content-type option restricts uploads to only certain type of files.

```sh
USAGE:
   mc share upload [OPTIONS] TARGET [TARGET...]

OPTIONS:
  --help, -h				Help of share download.
  --recursive, -r			Recursively upload any object matching the prefix.
  --expire, -E "168h"			Set expiry in NN[h|m|s].
  --content-type, -T 			Speific content-type to allow.
```

*Example: Generate a `curl` command to enable upload access to `play/mybucket/myotherobject.txt`. User replaces `<FILE>` with the actual filename to upload*

```sh
$ mc share upload play/mybucket/myotherobject.txt
URL: https://play.minio.io:9000/mybucket/myotherobject.txt
Expire: 7 days 0 hours 0 minutes 0 seconds
Share: curl https://play.minio.io:9000/mybucket -F x-amz-date=20160408T182356Z -F x-amz-signature=de343934bd0ba38bda0903813b5738f23dde67b4065ea2ec2e4e52f6389e51e1 -F bucket=mybucket -F policy=eyJleHBpcmF0aW9uIjoiMjAxNi0wNC0xNVQxODoyMzo1NS4wMDdaIiwiY29uZGl0aW9ucyI6W1siZXEiLCIkYnVja2V0IiwibXlidWNrZXQiXSxbImVxIiwiJGtleSIsIm15b3RoZXJvYmplY3QudHh0Il0sWyJlcSIsIiR4LWFtei1kYXRlIiwiMjAxNjA0MDhUMTgyMzU2WiJdLFsiZXEiLCIkeC1hbXotYWxnb3JpdGhtIiwiQVdTNC1ITUFDLVNIQTI1NiJdLFsiZXEiLCIkeC1hbXotY3JlZGVudGlhbCIsIlEzQU0zVVE4NjdTUFFRQTQzUDJGLzIwMTYwNDA4L3VzLWVhc3QtMS9zMy9hd3M0X3JlcXVlc3QiXV19 -F x-amz-algorithm=AWS4-HMAC-SHA256 -F x-amz-credential=Q3AM3UQ867SPQQA43P2F/20160408/us-east-1/s3/aws4_request -F key=myotherobject.txt -F file=@<FILE>
```

#### Sub-command `share list` - Share List

`share list` command lists unexpired URLs that were previously shared

```sh
USAGE:
   mc share list COMMAND

COMMAND:
   upload:   list previously shared access to uploads.
   download: list previously shared access to downloads.
```

<a name="mirror"></a>
### Command `mirror` - Mirror Buckets

`mirror` command is similar to `rsync`, except it synchronizes contents between filesystems and object storage.

```sh
USAGE:
   mc mirror [FLAGS] SOURCE TARGET

FLAGS:
  --help, -h					 Help of mirror.
  --force				         Force overwrite of an existing target(s).
  --fake					 Perform a fake mirror operation.
``` 

*Example: Mirror a local directory to 'mybucket' on https://play.minio.io:9000.*

```sh
$ mc mirror localdir/ play/mybucket
localdir/b.txt:  40 B / 40 B  ┃▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓┃  100.00 % 73 B/s 0
```

<a name="diff"></a>
### Command `diff` - Show Difference

``diff`` command computes the differences between the two directories. It only lists the contents which are missing or which differ in size.

It *DOES NOT* compare the contents, so it is possible that the objects which are of same name and of the same size, but have difference in contents are not detected. This way, it can perform high speed comparison on large volumes or between sites

```sh
USAGE:
   mc diff [FLAGS] FIRST SECOND

FLAGS:
  --help, -h					Help of diff.
```

*Example: Compare a local directory and a remote object storage.*

```sh
$  mc diff localdir play/mybucket
‘localdir/notes.txt’ and ‘https://play.minio.io:9000/mybucket/notes.txt’ - only in first.
```

<a name="policy"></a>
### Command `policy` - Manage bucket policies
Manage anonymous bucket policies to a bucket and its contents

```sh
USAGE:
   mc policy [FLAGS] PERMISSION TARGET
   mc policy [FLAGS] TARGET

PERMISSION:
   Allowed policies are: [none, download, upload, both].

FLAGS:
  --help, -h				Help of policy.
```   

*Example: Show current anonymous bucket policy*
Show current anonymous bucket policy for *andoria/myphotos/2020/bots/* sub-directory

```sh
$ mc policy s3/andoria/myphotos/2020/bots/
Access permission for ‘s3/andoria/myphotos/2020/bots/’ is ‘none’
```

*Example : Set anonymous bucket policy to download only*

Set anonymous bucket policy  for *andoria/myphotos/2020/bots/* sub-directory to download only

```sh
$ mc policy download s3/andoria/myphotos/2020/bots/
Access permission for ‘s3/andoria/myphotos/2020/bots/’ is set to 'download'
```

*Example : Remove current anonymous bucket policy*

Remove any bucket policy for *andoria/myphotos/2020/bots/* sub-directory.

```sh
$ mc policy none s3/andoria/myphotos/2020/bots/
Access permission for ‘s3/andoria/myphotos/2020/bots/’ is set to 'none'
```

<a name="session"></a>
### Command `session` - Manage Sessions

``session`` command manages previously saved sessions for `cp` and `mirror` operations

```sh
USAGE:
   mc session [FLAGS] OPERATION [ARG]

OPERATION:
   resume   		  Resume a previously saved session.
   clear       		  Clear a previously saved session.
   list           	  List all previously saved sessions.

SESSION-ID:
   SESSION - Session can either be $SESSION-ID or "all".

FLAGS:
  --help, -h					Help of session.
```

*Example: List all previously saved sessions.*

```sh
$ mc session list
IXWKjpQM -> [2016-04-08 19:11:14 IST] cp assets.go play/mybucket
ApwAxSwa -> [2016-04-08 01:49:19 IST] mirror miniodoc/ play/mybucket
```

*Example: Resume a previously saved session.*

```sh
$ mc session resume IXWKjpQM 
...assets.go: 1.68 KB / 1.68 KB  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  100.00 % 784 B/s 2s
```

*Example: Drop a previously saved session.*

```sh
$mc session clear ApwAxSwa
Session ‘ApwAxSwa’ cleared successfully.
```

<a name="config"></a>
### Command `config` - Manage Config File

`config host` command provides a convenient way to manage host entries in your config file `~/.mc/config.json`. It is also OK to edit the config file manually using a text editor.  

```sh
USAGE:
   mc config host OPERATION

OPERATION:
   add ALIAS URL ACCESS-KEY SECRET-KEY [API]
   remove ALIAS
   list

FLAGS:
  --help, -h				Help of config host
```

*Example: Manage Config File*

Add Minio server access and secret keys to config file host entry. Note that, the history feature of your shell may record these keys and pose a security risk. On `bash` shell, use `set -o` and `set +o` to disable and enable history feature momentarily.

```sh
$ set +o history
$ mc config host add myminio http://localhost:9000 OMQAGGOL63D7UNVQFY8X GcY5RHNmnEWvD/1QxD3spEIGj+Vt9L7eHaAaBTkJ
$ set +o history
```

<a name="update"></a>
### Command `update` - Software Updates

Check for new software updates from [https://dl.minio.io](https://dl.minio.io). Experimental flag checks for unstable experimental releases primarily meant for testing purposes.

```sh
USAGE:
   mc update [FLAGS]

FLAGS:
  --help, -h				Help for update.
  --experimental, -E			Check experimental update.
```

*Example: Check for an update.*

```sh
$ mc update
You are already running the most recent version of ‘mc’.
```

<a name="version"></a>
### Command `version` - Display Version

Display the current version of `mc` installed

```sh
USAGE:
   mc version [FLAGS]

FLAGS:
  --help, -h					Help for version.
```
 
 *Example: Print version of mc.*
 
```sh
$ mc version
Version: 2016-04-01T00:22:11Z
Release-tag: RELEASE.2016-04-01T00-22-11Z
Commit-id: 12adf3be326f5b6610cdd1438f72dfd861597fce
```
