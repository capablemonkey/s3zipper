# s3zipper
Microservice that Servers Streaming Zip file from S3 Securely

## Read the blog here
[Original Blog Post](http://engineroom.teamwork.com/how-to-securely-provide-a-zip-download-of-a-s3-file-bundle/)

## Getting started

Clone this repo.

```
git clone https://github.com/Teamwork/s3zipper.git
cd s3zipper
```

Install dependencies

```
go get
go install
```

Add your S3 and Redis credentials to `conf.json`.

```
cp sample_conf.json conf.json
nano conf.json
```

Start the server

```
go run s3zipper.go
```

Add a new zip file to Redis.  A zip file is represented as a JSON array containing file objects.  File objects have the following structure:

```json
{
  "S3Path": "users/1213/files/2016/baseball_pamphlet.pdf",
  "FileName": "filename_in_zip.pdf",
  "Folder": "optional_directory_within_zip"
}
```

To create a zip file containing just the above file, you'd create a new key of the format `zip:*` and set that to a string containing the JSON array:

```
127.0.0.1:6379> SET zip:testsession123 '[{"S3Path": "users/1213/files/2016/baseball_pamphlet.pdf","FileName": "filename_in_zip.pdf","Folder": "attachments"}]'
OK
```

Now, you can visit `http://localhost:8000/?ref=testsession123` and begin streaming the zip file!