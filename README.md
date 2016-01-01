# sspopulate

Demonstrate PopulateMergeMatch fails on Member

## Configuration of enclosing project

### composer.json
```
{
	"name": "silverstripe/installer",
	"description": "The SilverStripe Framework Installer",
	"require": {
		"php": ">=5.3.3",
		"silverstripe/cms": "3.2.1",
		"silverstripe/framework": "3.2.1",
		"silverstripe/reports": "3.2.1",
		"silverstripe/siteconfig": "3.2.1",
		"silverstripe-themes/simple": "3.1.*",
		"dnadesign/silverstripe-populate": "dev-master"
	},
	"require-dev": {
		"phpunit/PHPUnit": "~3.7"
	},
	"config": {
		"process-timeout": 600
	},
	"prefer-stable": true,
	"minimum-stability": "dev"
}
```

### _config/config.yml
```
---
Name: mysite
After:
  - 'framework/*'
  - 'cms/*'
---
# YAML configuration for SilverStripe
# See http://doc.silverstripe.org/framework/en/topics/configuration
# Caution: Indentation through two spaces, not tabs
SSViewer:
  theme: 'simple'
Populate:
  include_yaml_fixtures:
   - 'testPopulateMergeMatch/_config/test.yml'
```

### Executed Tasks
* `framework/sake dev/build`
* `framework/sakedev/tasks/PopulateTask`
* `framework/sakedev/tasks/PopulateTask`

Second execution of Populate-Task throws exception:
```
  + Creating test (Member)
ERROR [User Error]: Uncaught ValidationException: Can't overwrite existing member #1 with identical identifier (Email = test@test.com))
IN GET /dev/tasks/PopulateTask
Line 852 in /Users/neo/Sites/sspopulate/framework/security/Member.php

Source
======
  843: 
  844: 			// Note: Same logic as Member_Validator class
  845: 			$filter = array("\"$identifierField\"" => $this->$identifierField);
  846: 			if($this->ID) {
  847: 				$filter[] = array('"Member"."ID" <> ?' => $this->ID);
  848: 			}
  849: 			$existingRecord = DataObject::get_one('Member', $filter);
  850: 
  851: 			if($existingRecord) {
* 852: 				throw new ValidationException(ValidationResult::create(false, _t(
  853: 					'Member.ValidationIdentifierFailed',
  854: 					'Can\'t overwrite existing member #{id} with identical identifier ({name} = {value}))',
  855: 					'Values in brackets show "fieldname = value", usually denoting an existing email address',
  856: 					array(
  857: 						'id' => $existingRecord->ID,
  858: 						'name' => $identifierField,

Trace
=====
Member->onBeforeWrite()
DataObject.php:1217
[truncated]
``
