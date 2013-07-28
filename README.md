Description
===========

Installs and configures [ownCloud](http://owncloud.org/), an open source personal cloud for data and file sync, share and view.

Requirements
============

## Platform:

* CentOS
* Debian
* Ubuntu

## Cookbooks:

* apache2
* database
* mysql
* openssl
* php
* postfix

Attributes
==========

<table>
  <tr>
    <td>Attribute</td>
    <td>Description</td>
    <td>Default</td>
  </tr>
  <tr>
    <td><code>node['owncloud']['version']</code></td>
    <td>Version of ownCloud to install</td>
    <td><code>"5.0.7"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['download_url']</code></td>
    <td>Url from where ownCloud will be downloaded</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['www_dir']</code></td>
    <td>Root directory defined in Apache where web documents are stored</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['dir']</code></td>
    <td>Directory where ownCloud will be installed</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['data_dir']</code></td>
    <td>Directory where ownCloud data will be stored</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['server_name']</code></td>
    <td>Sets the server name for the ownCloud virtual host</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['ssl']</code></td>
    <td>Whether ownCloud should accept requests through SSL</td>
    <td><code>true</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['admin']['user']</code></td>
    <td>Administrator username</td>
    <td><code>"admin"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['admin']['pass']</code></td>
    <td>Administrator password</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['dbtype']</code></td>
    <td>Type of database, only mysql supported for now</td>
    <td><code>"mysql"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['dbname']</code></td>
    <td>Name of the ownCloud database</td>
    <td><code>"owncloud"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['dbuser']</code></td>
    <td>User to access the ownCloud database</td>
    <td><code>"owncloud"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['dbpassword']</code></td>
    <td>Password to access the ownCloud database</td>
    <td><em>calculated</em></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['dbhost']</code></td>
    <td>Host running the ownCloud database</td>
    <td><code>"localhost"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['dbtableprefix']</code></td>
    <td>Prefix for the ownCloud tables in the database</td>
    <td><code>""</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtpmode']</code></td>
    <td>Mode to use for sending mail, can be sendmail, smtp, qmail or php</td>
    <td><code>"sendmail"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtphost']</code></td>
    <td>Host to use for sending mail, depends on mail_smtpmode if this is used</td>
    <td><code>"127.0.0.1"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtpport']</code></td>
    <td>Port to use for sending mail, depends on mail_smtpmode if this is used</td>
    <td><code>25</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtptimeout']</code></td>
    <td>SMTP server timeout in seconds for sending mail, depends on mail_smtpmode if this is used</td>
    <td><code>10</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtpsecure']</code></td>
    <td>SMTP connection prefix or sending mail, depends on mail_smtpmode if this is used. Can be "", ssl or tls</td>
    <td><code>""</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtpauth']</code></td>
    <td>Whether authentication is needed to send mail, depends on mail_smtpmode if this is used</td>
    <td><code>false</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtpauthtype']</code></td>
    <td>authentication type needed to send mail, depends on mail_smtpmode if this is used. Can be LOGIN, PLAIN or NTLM</td>
    <td><code>"LOGIN"</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtpname']</code></td>
    <td>Username to use for sendmail mail, depends on mail_smtpauth if this is used</td>
    <td><code>""</code></td>
  </tr>
  <tr>
    <td><code>node['owncloud']['config']['mail_smtppassword']</code></td>
    <td>Password to use for sendmail mail, depends on mail_smtpauth if this is used</td>
    <td><code>""</code></td>
  </tr>
</table>

Recipes
=======

## owncloud::default

Installs and configures ownCloud.

Usage
=====

Add `recipe[owncloud]` to your node's run list or role, or include it in another cookbook.

On the first run, several passwords will be automatically generated and stored in the node:

* `node['owncloud']['admin']['pass']`
* `node['owncloud']['config']['dbpassword']`
* `node['mysql']['server_root_password']`
* `node['mysql']['server_repl_password']`
* `node['mysql']['server_debian_password']`

When using Chef Solo, these passwords need to be set manually.

The attribute `node['owncloud']['server_name']` should be set to the domain name used by the ownCloud installation. If not set, `node['fqdn']` will be used instead.

By default ownCloud cookbook relies on a local *Postfix* installation to send emails. But a remote SMTP server can be used changing `node['owncloud']['config']['mail_smtpmode']` to `smtp` and setting up the rest of the mail configuration attributes (see example below).

## Examples

### Basic owncloud role

    name "owncloud"
    description "Install ownCloud"
    default_attributes(
      "owncloud" => {
        "server_name" => "cloud.mysite.com"
      }
    )
    run_list(
      "recipe[owncloud]"
    )

### Using remote SMTP server

In this example an Amazon Simple Email Service account is used to send emails.

    name "owncloud_ses"
    description "Install ownCloud and use an AWS SES account to send emails"
    default_attributes(
      "owncloud" => {
        "server_name" => "cloud.mysite.com",
        "config" => {
          "mail_smtpmode" => "smtp",
          "mail_smtphost" => "email-smtp.us-east-1.amazonaws.com",
          "mail_smtpport" => 465,
          "mail_smtpsecure" => "tls",
          "mail_smtpauth" => true,
          "mail_smtpauthtype" => "LOGIN",
          "mail_smtpname" => "amazon-ses-username",
          "mail_smtppassword" => "amazon-ses-password",
        }
      }
    )
    run_list(
      "recipe[owncloud]"
    )

Testing
=======

## Requirements

* Install [Vagrant](http://www.vagrantup.com/)
* Install gem dependencies with bundler:

```bash
$ gem install bundler
$ bundle install
```

## Running the tests

```bash
$ bundle exec kitchen test
```

Contributing
============

1. Fork the repository on Github
2. Create a named feature branch (like `add_component_x`)
3. Write you change
4. Write tests for your change (if applicable)
5. Run the tests, ensuring they all pass
6. Submit a Pull Request using Github

License and Author
==================

|                      |                                          |
|:---------------------|:-----------------------------------------|
| **Author:**          | Raúl Rodríguez (<raul@onddo.com>)
| **Copyright:**       | Copyright (c) 2013 Onddo Labs, SL. (www.onddo.com)
| **License:**         | Apache License, Version 2.0

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
