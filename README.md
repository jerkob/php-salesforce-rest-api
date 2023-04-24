# Php Salesforce Rest Api

Originally forked from: https://github.com/Cleeng/php-salesforce-rest-api
which was forked from: https://github.com/bjsmasth/php-salesforce-rest-api

## Changelog

Guzzle 7 and PHP >=7.2 are required starting with version 0.7.

Versions 0.6 and below require Guzzle 6 and PHP >=5.6.

## Install

Via Composer

``` bash
composer require jerkob/php-salesforce-rest-api
```

# Getting Started

Setting up a Connected App

1. Log into to your Salesforce org
2. Click on Setup in the upper right-hand menu
3. Under Build click ```Create > Apps ```
4. Scroll to the bottom and click ```New``` under Connected Apps.
5. Enter the following details for the remote application:
    - Connected App Name
    - API Name
    - Contact Email
    - Enable OAuth Settings under the API dropdown
    - Callback URL
    - Select access scope (If you need a refresh token, specify it here)
6. Click Save

After saving, you will now be given a Consumer Key and Consumer Secret. Update your config file with values for ```consumerKey``` and ```consumerSecret```

# Setup

Authentication

```bash
    $options = [
        'grant_type' => 'password',
        'client_id' => 'CONSUMERKEY',
        'client_secret' => 'CONSUMERSECRET',
        'username' => 'SALESFORCE_USERNAME',
        'password' => 'SALESFORCE_PASSWORD' . 'SECURITY_TOKEN'
    ];
    
    $salesforce = new jerkob\Salesforce\Authentication\PasswordAuthentication($options);
    $salesforce->authenticate();
    
    $accessToken = $salesforce->getAccessToken();
    $instanceUrl = $salesforce->getInstanceUrl();
    
    Change Endpoint
    
    $salesforce = new jerkob\Salesforce\Authentication\PasswordAuthentication($options);
    $salesforce->setEndpoint('https://test.salesforce.com/');
    $salesforce->authenticate();
 
    $accessToken = $salesforce->getAccessToken();
    $instanceUrl = $salesforce->getInstanceUrl();
```

Query

```bash
    $query = 'SELECT Id,Name FROM ACCOUNT LIMIT 100';
    
    $crud = new \jerkob\Salesforce\CRUD($instanceUrl, $accessToken);
    $crud->query($query);
```

Create

```bash
    
    $data = [
       'Name' => 'some name',
    ];
    
    $crud->create('Account', $data);  #returns id
```

Update

```bash
    $new_data = [
       'Name' => 'another name',
    ];
    
    $crud->update('Account', $id, $new_data); #returns status_code 204
    
```
Upsert

```bash
    $new_data = [
       'Name' => 'another name',
    ];
    
    $crud->upsert('Account', 'API Name/ Field Name', 'value', $new_data); #returns status_code 204 or 201
    
```

Delete

```bash
    $crud->delete('Account', $id);

```
Describe

```bash
    $crud->describe('Account');

```
