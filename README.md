# CakePHP 3.x - RouterOs

## Installation

### Composer

You can install this plugin into your CakePHP application using [composer](http://getcomposer.org).

The recommended way to install composer packages is:

```
composer require ivanamat/cakephp3-routeros
```


### Git submodule

    git submodule add git@github.com:ivanamat/cakephp3-routeros.git plugins/RouterOs
    git submodule init
    git submodule update


### Manual installation

Download the .zip or .tar.gz file, unzip and rename the plugin folder "cakephp3-routeros" to "RouterOs" then copy the folder to your plugins folder.

[Download release](http://git.ivanamat.es/cakephp/cakephp3-routeros/tags)


## Getting started

Include the RouterOs plugin in *app/config/bootstrap.php*

    Plugin::load('RouterOs');

Include the RouterOs component in your controller.

```php
    public $components = [
        'RouterOs' => [
            'className' => 'RouterOs.RouterOs'
        ]
    ];
```

## Options

### debug
Set debug mode true/false
```php
    $this->RouterOs->debug = true;
```

## Methods

### available($host, $port = 8728, $timeout = 1)
Checks connection to the device.

**$host** Host to connect.  
**$port** Port to connect.  
**$timeout** Router timeout.

**return** boolean  
```php
    if($this->RouterOs->available('10.0.0.100')) {
        ...
    }
```    
### comm($com, $arr = array())
Write (send) data to Router OS.

**$com** A string with the command to send.  
**$arr** An array with arguments or queries.

**return** Array with parsed.

```php
    # http://wiki.mikrotik.com/wiki/API_PHP_class
    # example3.php
    
    $this->RouterOs->comm("/ppp/secret/add", array(
        "name"     => "user",
        "password" => "pass",
        "remote-address" => "172.16.1.10",
        "comment"  => "{new VPN user}",
        "service"  => "pptp",
    ));
```
### connect($ip, $login, $password)
Login to RouterOS.

**$ip** Hostname (IP or domain) of the RouterOS server.  
**$login** The RouterOS username  
**$password**  The RouterOS password

**return** boolean

```php
    # http://wiki.mikrotik.com/wiki/API_PHP_class
    if ($this->RouterOs->connect('10.0.0.100', 'username', 'password')) {
        ...
    }
```
### debug($text)
Save text for debug purposes into log file.

**$text** Text to write in log file

**return** void

```php
    # http://wiki.mikrotik.com/wiki/API_PHP_class
    $this->debug('Some text:');
    
    $data = array(
        ...
    );
    $this->debug($data);
```
### delete($com, $options = array())
Delete records.

**$com** A string with the command to send.
**$options** (int/array) '.id' of element or array conditions [key => value].

**return** boolean

```php
    # Delete item with id 1
    $this->RouterOs->delete('/ip/pool',1);
    
    # Delete items with conditions
    $this->RouterOs->delete('/ip/pool',[
        'name' => 'MyPool',
        'ranges' => "10.10.10.2-10.10.10.254"]
    );
```
### disconnect()
Close socket connection.

**return** void

```php
    $this->RouterOs->disconnect();
```
### find($path, $options = array())
Make a quick search.

**$path** A string with the command path to send. Do not include the action (getall, add, set,... ).  
**$options** (int/array) '.id' of element / Array of fields to search as [key => value].

```php
    # Find all interfaces
    $interfaces = $this->RouterOs->find('/interface');
    
    # Get interface ether1
    $interface = $this->RouterOs->find('/interface', ['name' => 'ether1']);
```
### get($path, $id)
Get element by id.

**$path** A string with the command path to send. Do not include the action (getall, add, set,... ).  
**$id** Element id.

```php
    $interface = $this->RouterOs->get('/interface', '*1');
```
### parseResponse($response)
Parse response from Router OS.

**$response** Response data.

**return** Array with parsed data.

```php
    # http://wiki.mikrotik.com/wiki/API_PHP_class
    # example1.php
    
    $this->RouterOs->write('/interface/getall');
    
    $READ = $this->RouterOs->read(false);
    $ARRAY = $this->RouterOs->parseResponse($READ);
    
    debug($ARRAY);
    
    $this->RouterOs->disconnect();
```
### read($parse = true)
Read data from Router OS.

**$read** Parse the data? default: true

**return** Array with parsed or unparsed data.

```php
    # http://wiki.mikrotik.com/wiki/API_PHP_class
    # example6.php
    
    #show me what you got
    $this->RouterOs->write('/ip/dns/static/print');
    $ips = $this->RouterOs->read();
    debug($ips);
```
### save($path, $options = array())
Write (send) data to Router OS.

**$path** A string with the command path to send. Do not include the action (getall, add, set,... ).  
**$options** Array of fields [key => value, key => value, ...]. If id is set in the array, the element will be updated.

**return** On create !trap or new element id. On update empty or !trap.

```php
    # Create new element
    $this->RouterOs->save('/ip/pool',[
        'name' => 'MyRange',
        'ranges' => "10.10.10.2-10.10.10.100"]
    );
    
    # Update de element.
    $this->RouterOs->save('/ip/pool',[
        '.id' => '*1',
        'name' => 'MyRange',
        'ranges' => "10.10.10.2-10.10.10.254"]
    );
```
### write($command, $param2 = true)
Write (send) data to Router OS.

**$command** A string with the command to send.  
**$param2** If we set an integer, the command will send this data as a "tag".  
If we set it to boolean true, the funcion will send the comand and finish.  
If we set it to boolean false, the funcion will send the comand and wait for next command.  
Default: true

**return** boolean Return false if no command especified.

```php
    # http://wiki.mikrotik.com/wiki/API_PHP_class
    # example3.php
    
    $ips = array(
        ...
    );
    
    # delete them all !
    foreach($ips as $num => $ip_data) {
     $this->RouterOs->write('/ip/dns/static/remove', false);
     $this->RouterOs->write("=.id=" . $ip_data[".id"], true);
    }
```

## About CakePHP 3.x - RouterOs

This is an adaptation to the CakePHP component from class originally written by Denis Basta.

### About RouterOS PHP API class v1.6

[MikroTik API PHP](http://wiki.mikrotik.com/wiki/API_PHP_class)

Client API for RouterOS/Mikrotik

This class was originally written by Denis Basta and updated by several contributors.  It aims to give a simple interface to the RouterOS API in PHP.

### Contributors (before moving to Git)
* Nick Barnes
* Ben Menking (ben [at] infotechsc [dot] com)
* Jeremy Jefferson (http://jeremyj.com)
* Cristian Deluxe (djcristiandeluxe [at] gmail [dot] com)
* Mikhail Moskalev (mmv.rus [at] gmail [dot] com)

RouterOS PHP API class v1.6 on [Ben Menking GitHub](https://github.com/BenMenking/routeros-api)  
Mikrotik Wiki page at http://wiki.mikrotik.com/wiki/API_PHP_class

## Author

Iván Amat

[on GitHub](http://github.com/ivanamat)

[www.ivanamat.es](http://www.ivanamat.es/)

## Licensed

[MIT License](https://opensource.org/licenses/MIT)
