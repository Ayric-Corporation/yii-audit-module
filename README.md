# Yii Audit Module

Track and store usage information including page requests, database field changes and system errors.

Yii's built-in error handling is amazing... when you are in debug mode. However as soon as you set YII_DEBUG to false you stop getting those pretty error messages, which can make it tricky to find a problem in a live environment.


### Contents

[Features](#features)  
[Installation](#installation)  
[Configuration](#configuration)  
[License](#license)  
[Links](#links)  


## Features

- Track site activity including everything you need to know about the request.
- Track data changes to your model data and relate those changes to an AuditRequest.
- Full error stack dump is saved, even in live mode.
- Catches all errors, including fatal errors.
- View all the collected data from the module interface.


## Installation

Please download using ONE of the following methods:


### Composer Installation

```
curl http://getcomposer.org/installer | php
php composer.phar require mrphp/yii-audit-module
```


### Manual Installation

Download the [latest version](https://github.com/cornernote/yii-audit-module/archive/master.zip) and move the `audit` folder into your `protected/modules` folder.


## Configuration

Add yii-audit-module to the `modules` in your yii configuration:

```php
return array(
	'modules' => array(
		'audit' => array(
			// path to the AuditModule class
			'class' => 'vendor.mrphp.yii-audit-module.audit.AuditModule',
			// if you downloaded into modules
			//'class' => 'application.modules.audit.AuditModule',

			// add a list of users who can access the audit module
			'adminUsers' => array('admin'),

			// set this to your user view url, 
			// AuditModule will replace --user_id-- with the actual user_id
			'userViewUrl' => array('/user/view', 'id' => '--user_id--'),

			// set this to false in production to improve performance
			'autoCreateTables' => true,
		),
	),
);
```

Use `AuditErrorHandler` as your application error handler by updating the `components` section in your yii configuration:

```php
return array(
	'preload' => array('log', 'errorHandler'), // preload to handle fatal errors
	'components' => array(
		'errorHandler' => array(
			// path to the AuditErrorHandler class
			'class' => 'audit.components.AuditErrorHandler',

			// set this as you normally would for CErrorHandler
			'errorAction' => 'site/error',

			// set this to true to track all requests
			'trackAllRequests' => true,
		),
	),
);
```

To handle fatal errors we have add the error handler to the `preload` section in your yii configuration:

```php
return array(
	'preload' => array(
		'log', 
		'errorHandler', // handle fatal errors
	),
);
```

To track field changes add `AuditFieldBehavior` to your CActiveRecord `behaviors()` functions.

```php
class Post extends CActiveRecord
{
	public function behaviors()
	{
		return array(
			'AuditFieldBehavior' => 'audit.components.AuditFieldBehavior',
		);
	}
}
```

Add information to your footer (optional):
```php
$this->renderPartial('audit.views.request.__footer');
```

included partials for your app ... docs coming soon...


## License

- Author: Brett O'Donnell <cornernote@gmail.com>
- Author: Zain Ul abidin <zainengineer@gmail.com>
- Source Code: https://github.com/cornernote/yii-audit-module
- Copyright � 2013 Mr PHP <info@mrphp.com.au>
- License: BSD-3-Clause https://raw.github.com/cornernote/yii-audit-module/master/LICENSE


## Links

- [Yii Extension](http://www.yiiframework.com/extension/yii-audit-module)
- [Composer Package](https://packagist.org/packages/mrphp/yii-audit-module)
- [MrPHP](http://mrphp.com.au)
