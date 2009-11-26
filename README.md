Cakephp Skipjack Component
====

Cakephp component for connecting to Skipjack API and submit credit card orders.

@version 0.1

<http://jpablobr.com/>


Installation
------------

* Cakephp 1.2.+
* PHP 5.+

Install:

    * Create a new file in your app/controllers/components directory named 'skipjack.php'.

Then to be able to use the component you'll need to declare it in your controller.:

    class MyController extends AppController {

    var $name = 'MyController';

    $components = array('Skipjack');

    ...

    }

And call the component from your controller.

        function payment() {
        // Set development env
        $this->Skipjack->setDeveloper(true);

        // Sample data retrievement
        $order = $this->Session->read('Order');
        $ccPostInfo = $this->data['Payment'];
        $cart = $this->Cart->get_contents();
        $order_id = $this->Order->id;

        // Creates CC donor info that will be post to skipjack
        $ccfields = array();
        $ccfields['OrderNumber'] = $order_id;
        $ccfields['ItemNumber']  = $ccPostInfo['ItemNumber'];
        $ccfields['ItemDescription'] = $ccPostInfo['ItemDescription'];
        $ccfields['ItemCost'] = $ccPostInfo['ItemCost'];
        $ccfields['Quantity'] = $ccPostInfo['Quantity'];
        $ccfields['Taxable'] = $ccPostInfo['Taxable'];
        $ccfields['AccountNumber'] = $ccPostInfo['AccountNumber']; // '4111111111111111';
        $ccfields['Month'] = $ccPostInfo['month']; //'08';
        $ccfields['Year'] = $ccPostInfo['year']; // '09'; cake form helper can only display in 4 digit format
        $ccfields['TransactionAmount'] = $ccPostInfo['TransactionAmount']; // < than $100 for test env

        // Sets recommended dummy values
        $this->Skipjack->dummyVals = array(
            'SJName' => $order['Order']['billing_first_name'],
            'Email'  => $order['Order']['email'],
            'StreetAddress' => $order['Order']['billing_address_1'],
            'City' => $order['Order']['billing_city'],
            'State' => $order['Order']['billing_state'],
            'ZipCode' => $order['Order']['billing_zip'],
            'ShipToPhone' => $order['Order']['billing_phone'],
            'OrderString' => $order['Order']['OrderString']
        );

        // Array to be sent to object at once
        $this->Skipjack->addFields($ccfields);

        // Runs process and verify if it’s successful
        if($this->Skipjack->process() && $this->Skipjack->isApproved()) {
            $this->Session->setFlash('Transaction approved!');
        } else {
            $this->Session->setFlash('There was an error in the transaction...');
        }
    }


Get Cakephp Skipjack Component
--------

Browse the source on GitHub: <http://github.com/jpablobr/SkipjackComponent>

Clone with Git:

    $ git clone git://github.com/jpablobr/SkipjackComponent

Or download in either
[zip](http://github.com/jpablobr/SkipjackComponent/zipball/master) or
[tar](http://github.com/jpablobr/SkipjackComponent/tarball/master) formats.

Future enhancements:
--------

Migrate it from cURL to cake’s HttpSocket:
<http://api.cakephp.org/view_source/http-socket/#line-111>

Issues
------

Find a bug? Want a feature? Submit an [issue
here](http://github.com/jpablobr/SkipjackComponent/issues). Patches welcome!

Author
-------

Jose Pablo Barrantes <jpablobr@jpablobr.com>
license <http://www.opensource.org/licenses/mit-license.php> MIT License

Based on Vinay Yadav SkipJack.php original class
<http://www.phpclasses.org/browse/file/4751.html>

* [Jose Pablo Barrantes][1]

[1]: http://jpablobr.com/
[1]: http://github.com/jpablobr

CakePHP README file
--------

CakePHP is a rapid development framework for PHP which uses commonly known design patterns like Active Record,
Association Data Mapping, Front Controller and MVC. Our primary goal is to provide a structured framework that enables
PHP users at all levels to rapidly develop robust web applications, without any loss to flexibility.

The Cake Software Foundation - promoting development related to CakePHP
<http://www.cakefoundation.org/>

CakePHP - the rapid development PHP framework
<http://www.cakephp.org>

Cookbook - user documentation for learning about CakePHP
<http://book.cakephp.org>

API - quick reference to CakePHP
<http://api.cakephp.org>

The Bakery - everything CakePHP
<http://bakery.cakephp.org>

The Show - live and archived podcasts about CakePHP and more
<http://live.cakephp.org>

CakePHP Google Group - community mailing list and forum
<http://groups.google.com/group/cake-php>

#cakephp on irc.freenode.net - chat with CakePHP developers
<irc://irc.freenode.net/cakephp>

CakeForge - open development for CakePHP
<http://cakeforge.org>

CakePHP gear
<http://www.cafepress.com/cakefoundation>

Recommended Reading
<http://astore.amazon.com/cakesoftwaref-20/>