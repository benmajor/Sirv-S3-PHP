# SirvPHP Client

This library is a Sirv S3 Client written in PHP. It is a port of the official SDK released by Sirv with a few modifications, and rebuilt as a Composer package.

---

### Installation:

The easiest way to install the client is using [Composer](https://www.getcomposer.com/):

```
$ composer require benmajor/sirv-php
```

Since there are no external dependencies (other than PHP >= `5.4` and with the cURL module installed), you can also download the single PHP class from the `src` directory and include it in your code using PHP's `require` statement:

```php
require '/path/to/SirvS3Client.php;
```

---

### Usage:

This client provides some utility functions to easily work with the Sirv S3 buckets and upload content directly from your project using PHP. Usage and function reference is as follows:

#### 1. Creating the client:

We must first create a client before using any of the functions. This is done easily by instantiating the class using your Sirv S3 Bucket, S3 Key and S3 Secret (all of which are located in your [Settings](https://my.sirv.com/#/account/settings/api) page) as follows:

```php
# Include Composer autoloader:
require 'vendor/autoload.php';

use BenMajor\SirvPHP\SirvS3Client;

$client = new SirvS3Client(
  'SIRV_S3_BUCKET',
  'SIRV_S3_KEY',
  'SIRV_S3_SECRET'
);
```

Once the client has been instantiated, you can check the connection is valid and live using the `testConnection()` method:

```php
if( $client->testConnection() ) {
  # Connection is valid and ready.
} else {
  # Something went wrong with the connection.
}
```

#### 2. Check if resource exists:

To check if a folder exists within the Bucket, simply use `checkIfObjectExists()` by passing a relative folder path to the method. The method returns `true` if the folder exists, or `false` if not:

```php
$client->checkIfObjectExists('/example/folder');
```

It's also possible to check if a specific file exists within a folder by passing the basename of the file (including extension) as a second parameter to the `checkIfObjectExists` method:

```php
$client->checkIfObjectExists('example/folder', 'image.jpg');
```

Returns `true` if the resource exists, otherwise `false`.

#### 3. Get a list of resources:

To get a list of resources (i.e. folders and files) that exist, use the `getBucketContents()` method:

```php
$client->getBucketContents('example/folder');
```

Note that we have specified a folder path above to retrieve only resources that exist within the `example/folder` directory. The method can also be called with no parameter to retrieve the contents of the Bucket's root.

This method returns an object as follows:

```json
{
  "bucket": "BUCKET_NAME",
  "current_dir": "CURRENT_WORKING_DIRECTORY",
  "contents": [ ],
  "dirs": [ ]
}
```

#### 4. Creating folders:

To create a new folder, simply use the `createFolder()` method and pass the relative path to the folder to be created as the first argument:

```php
$client->createFolder('example/new-folder');
```

Returns `true` if the folder was successfully created, otherwise `false`.

#### 5. Uploading files:

Files can be easily uploaded using the `uploadFile()` method. The first parameter is the destination folder and filename to which the file should be uploaded. The second parameter must contain a relative path to a file that already exists on the server (it can also be a reference to the temporary file created following a file upload). 

Two examples are given below; one using a file that already exists, and another of uploading a user-uploaded file from PHP's temporary directory.

**Upload existing file on server:**

```php
$client->uploadFile(
  'example/folder/my-picture.jpg',
  '/path/to/local/my-picture.jpg'
);
```

**Uploading a temporary file:**

```php
$client->uploadFile(
  'example/folder/'.basename($_FILES['uploaded']['name']),
  $_FILES['uploaded']['TMP_NAME']
);
```

#### 6. Downloading files:

The `getFile()` method can be used to retrieve the raw contents of a file from the Bucket. The string can then be used to create a file on the local filesystem (using `file_put_contents()` or similar). If the file does not exist on the S3 Bucket, or fails for another reason, the method returns an empty string, so it is advisable to check the returned value before using it:

```php
$file = $client->getFile('/path/to/file.jpg');

if( ! empty($file) ) {
  file_put_contents('file.jpg', $file);
}
```

#### 7. Copying files:

Sometimes, we may wish to copy a file from one directory to another within the Bucket. To do this, use the `copyFile()` method. The first paramater is the path of the source file, and the second parameter is its destination:

```php
$client->copyFile(
  'path/to/image.jpg',
  'another/path/image.jpg'
);
```

Files can also be copied to the same directory, but must be renamed:

```php
$client->copyFile(
	'path/to/image.jpg',
	'path/to/image-copy.jpg'
);
```

#### 8. Renaming files:

It is sometimes necessary to rename a file within the Bucket. This can be done using the `renameFile()` method, and passing the relative path as the first parameter:

```php
$client->renameFile(
  'example/folder/image.jpg',
  'example/folder/renamed.jpg'
);
```

#### 9. Deleting files:

The `deleteFile()` method can be used to delete files from the Bucket instance. The relative path of the resource to be deleted should be passed as the first parameter of this method. 

```php
$client->deleteFile('example/folder/image.jpg');
```

---

### Credits:

The package is heavily based on the official SDK released by [Sirv](https://sirv.com/help/articles/s3-api/php-sdk-for-sirv-s3/), which itself is a simplified version of the [AWS PHP SDK](https://aws.amazon.com/sdk-for-php/).

---

### License:

This Composer package is released under the MIT License as follows:

> Copyright (c) 2020 Ben Major
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
> FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
> AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
