
# Email

a module for Emvicy2 (2.x) PHP Framework: https://github.com/emvicy/Emvicy/tree/2.x

Emails to be sent are processed via Queue.   
Data type classes are available for composing emails and attachments, which simplify the declaration.

---

## Installation

_cd into the modules folder of your `Emvicy` copy; e.g.:_
~~~bash
cd /var/www/html/modules/;
~~~

_clone `Email`_
~~~bash
git clone --branch 2.x https://github.com/emvicy/Email.git Email;
~~~


## Config

add this config to the config of your primary working module.

~~~php
//######################################################################################################################
// Module Email

$aConfig['MODULE']['Email'] = array(

    // callback function
    'oCallback' => function($oEmail) {

        // send e-mail via SMTP
        return \Email\Model\Smtp::sendViaPhpMailer($oEmail);
    },
    
    'sSenderEmailAddress' => getenv('email.sSenderEmailAddress'),

    /**
     * SMTP account settings
     * get from .env file
     */
    'sHost' => getenv('email.sHost'),
    'iPort' => getenv('email.iPort'),       # ssl=465 | tls=587
    'sSecure' => getenv('email.sSecure'),   # ssl | tls
    'bAuth' => getenv('email.bAuth'),
    'sUsername' => getenv('email.sUsername'),
    'sPassword' => getenv('email.sPassword'),
);
~~~

---

## Usage

**somewhere in your Emvicy2 Controller or Model**

_create an Email Object and add job to Queue_
~~~php
// email
$oDTEmail = \Email\DataType\DTEmail::create()
    ->set_subject('Example Subject')
    ->set_recipientMailAdresses(array('foo@example.com'))
    ->set_senderMail(Config::MODULE('Email')['sSenderEmailAddress'])
    ->set_senderName('foo')
    ->set_text("Foo\nbar\n")
    ->set_html('<h1>Foo</h1><p>bar</p>')
    // 1. attachment
    ->add_aAttachment(DTEmailAttachment::create()->set_file('/var/www/html/public/robots.txt')->set_name('robots.txt'))
    ;    

// add job to queue
Queue::push(
    oDTAppTableQueue: \App\DataType\DTAppTableQueue::create()
        ->set_key('Email::new')
        ->set_value(json_encode(Convert::objectToArray($oDTEmail))),
    bPreventMultipleCreation: true
);
~~~

---

**Queue / Worker**

add worker to your primary module queue/worker config  
_(replace `'Foo'` by your primary module name)_

~~~php
<?php

$aConfig['MODULE']['Foo']['queue']['worker'] = [

    // queue key
    //                                      responsible worker class
    'Email::new'                        => '\Email\Model\Worker\EmailNew',

];
~~~

---

## Events

- `email.model.worker.EmailNew.send.before`: DTEmail
- `email.model.smtp.sendViaPhpMailer.before`: DTEmail
- `email.model.smtp.sendViaPhpMailer.error`: Exception
- `email.model.smtp.sendViaPhpMailer.after`: DTEmailResponse
- `email.model.worker.EmailNew.send.after`: DTEmailResponse
