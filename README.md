DC_Multilingual
===============

This is a standalone DC driver for contao that allows you to easily make your data translatable.

Usage
-----
```php
	<?php
	/* DCA configuration */
	
	// set the driver
	$GLOBALS['TL_DCA']['table']['config']['dataContainer'] = 'Multilingual';
	
	// languages you want to provide for translation
	$GLOBALS['TL_DCA']['table']['config']['languages'] = array('de', 'en');
	
	// database column that contains the language keys (default: "language")
	$GLOBALS['TL_DCA']['table']['config']['langColumn'] = 'language_dc';
	
	// database column that contains the reference id (default: "langPid")
	$GLOBALS['TL_DCA']['table']['config'][pidColumn'] = 'pid';
	
	// fallback language - if none is given then there will be another language "fallback" selectable from the dropdown
	$GLOBALS['TL_DCA']['table']['config']['fallbackLang'] = 'de';
	
	
	/* Field configuration */
	// if you don't use ['eval']['translatableFor'] and the user is not editing the fallback language, then the field will be hidden for all the languages
	
	// use '*' to make a field translatable for all languages
	$GLOBALS['TL_DCA']['table']['fields']['username']['eval']['translatableFor'] = '*';
	
	// use an array of language keys to specify for which languages the field is translatable
	$GLOBALS['TL_DCA']['table']['fields']['name']['eval']['translatableFor'] = array('de');
	
```

	
Querying using the DC_Multilingual_Query builder
-----
```php
	<?php
	$objQuery = new DC_Multilingual_Query('table');
	
	// the base query as string
	// you see the table-alias t1 for the base-fields
	// and t2 for the language-fields
	echo $objQuery->getQuery();
	
	// probably you would change the language, default is $GLOBALS[TL_LANGUAGE]
	$objQuery->language = 'en';
	
	// add some conditions
	$objQuery->addField('CONCAT(t2.firstname," ",t2.lastname") AS fullname')
	         ->addField('joinedTable.anotherField')
			 ->addJoin('LEFT JOIN joinedTable ON (t1.joinedID = joinedTable.id)');
	
	// some WHERE and ORDER conditions
	$objQuery->addWhere('t1.published="1"')->addOrder('t2.lastname');
	
	echo $objQuery->getQuery();
	
	// or get the Database_Statement instance
	$objStatement = $objQuery->getStatement();
	
	$objResult = $objQuery->getStatement()->execute();
	
```