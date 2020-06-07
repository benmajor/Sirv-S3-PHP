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

#### 3. Get a list of resources:

To get a list of resources (i.e. folders and files) that exist, use the `getBucketContents()` method:

```php
$client->getBucketContents('example/folder');
```

Note that we have specified a folder path above to retrieve only resources that exist within the `example/folder` directory. The method can also be called with no parameter to retrieve the contents of the Bucket's root.

#### 4. Uploading files:



---

### Credits:

The package is heavily based on the official SDK released by [Sirv](https://sirv.com/help/articles/s3-api/php-sdk-for-sirv-s3/), which itself is a simplified version of the [AWS PHP SDK](https://aws.amazon.com/sdk-for-php/).

---

### License:

This Composer package is released under the MIT License as follows:

> Copyright (c) 2020 Ben Major

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
