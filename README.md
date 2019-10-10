Emailpy
=======

Emailpy is a basic python library for sending and reading email messages by using the IMAP and SMTP protocols. It's meant to offer a fast and painless access to email services. Supported Python2 and Python3.

# Features
* Send/read messages via IMAP and SMTP   
* Attach and download files to/from messages
* Easily add custom email services
* Ease of use

# Installation
Get `emailpy` source from Github and install it.

## Linux
Execute the following commands:
```sh
git clone git://github.com/ianovir/emailpy.git
cd emailpy/
sudo python emailpy/setup.py install
sudo rm -rf emailpy
```   
## Windows
Download repository or execute the following git command:
```sh
git clone git://github.com/ianovir/emailpy.git
```
Via Windows cmd (or Powershell) navigate to the downloaded folder `emailpy/` and execute the follwing command:
```sh
python emailpy/setup.py install
```   
Finally, delete the folder `emailpy/`
   
   
# How to use emailpy   
In your code, import `emailpy` and get a reference to a Service (e.g. Gmail) :
```python
import emailpy

sm = emailpy.ServiceManager()
mail = sm.get_service("gmail")
```
then setup with user id and password:
```python
mail.setup("user@gmail.com", "password")
```
At the same way, it is possible to get other services:
```python
mail1 = sm.get_service("outlook")
mail2 = sm.get_service("yahoo")
```
Note: at the moment they are handled the following services: `gmail`, `yahoo`, `outlook`. More coming in the future.
To use other services, please read the [Custom Email Services](#custom-email-services) section.

# Send/Read messages

Send message:
```python
mail.send(["john.doe@yahoo.com", "jane.doe@gmail.com"], "Subject", body="Message body" )
```
The body parameter is optional and can be omitted.
Messages can be read by calling the method `MailService.read()` and iterating the returned messages:
```python
messages = gmail.read()
for msg in messages:
    print("Date: " + str(msg.Date))
    print("From: " + msg.From)
    print("To: " + msg.To)
    print("Subject: " + msg.Subject)
```

In the same way, it is possible to iterate the parts of message body:
```python
messages = gmail.read():
for msg in messages:
    for b in msg.Body:
        print("Content part: " + str(b))
```

Function `MailService.read()` can take search criteria as argument. Criteria values can be accessed via the `emailpy.Constants` :
```python
from emailpy import Constants
messages = gmail.read(criteria=Constants.UNSEEN)
```
For example, filtering by 'From' field:
```python
messages = gmail.read(Constants.FROM + ' "joey smith"')
```
Or filtering by date:
```python
messages = gmail.read(Constants.SINCE + ' 10-Aug-2019')
```

For more information about IMAP search criterias, please take a look at [IMAP](https://tools.ietf.org/html/rfc3501).

# Attachments

Sending attachment:
```python
mail.send(["jane.doe@yahoo.com"], "Subject", attachments = ["file1.txt", "file2.bat"] )
```
pass the files' paths into the `attachments` argument.
Attachments of the result messages can be downloaded by using the following argument:
```python
mail.read(download_attachments= True)
```
attachments will be stored under the `./attachment/` subfolder.
It is possible to explore attachments' filenames for each message: 
```python
for msg in yahoo.read(Criteria=Constants.UNSEEN, download_attachments=True):
    for fn in msg.Attachments:
        print("Attachment file name: " + fn)
```
Please note that `EMessage.Attachments` contains only the names of the possible attachments, even if the `download_attachments` is ```False```.

# Custom Email Services

You can configure a custom Email server by creating a custom .xml service file (similar to `services.xml`).
Example:
```xml
<config version="0">
    <services>
        ...
        <service id="custom_service" smtp_address="smtp.custom" smtp_port="123" imap_address="imap.custom" imap_port="321" />
        ...
    </services>
</config>
```
then, pass its path to the ServiceManager:
```python
my_sm = ServiceManager("my_service_file.xml")
custom_service = my_sm.get_service("custom_service")
```
It is important to avoid duplicate Services IDs in the .xml file.

Another (faster) way to use custom email service is to use directly the `EmailService` class:
```python
my_sm = MailService("gmail", "smtp.gmail.com", 587, "imap.gmail.com", 993)
```

# Account security
In order to grant IMAP and SMTP access to your program with services (e.g. Gmail, Yahoo) you may need to lower the security of the account in order to permit access from third-party applications.

# Notices
This project is incomplete and some features may miss. Feel free to make your modifications and improvements by forking the master branch

# Copyright
Copyright(c) 2019 Sebastiano Campisi - [ianovir.com](https://ianovir.com). 
Read LICENSE file for more details.

