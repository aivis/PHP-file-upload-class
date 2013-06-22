Simple php upload class with file validation.
=============================================

You need to install (or enable in php.ini) PHP "file info" extension.

Link: http://us2.php.net/manual/en/fileinfo.installation.php

### **Quick start**

```php
<?php
require_once 'upload.php';

if (!empty($_FILES['test'])) {
	
	$upload = Upload::factory('important/files');
	$upload->file($_FILES['test']);
	
	$results = $upload->upload();
	
	var_dump($results);
	
}
```

### **Simple validation**

```php
<?php

require_once 'upload.php';

if (!empty($_FILES['test'])) {
	
	$upload = Upload::factory('important/files');
	$upload->file($_FILES['test']);
	
	//set max. file size (in mb)
	$upload->set_max_file_size(1);
	
	//set allowed mime types
	$upload->set_allowed_mime_types(array('application/pdf'));
	
	$results = $upload->upload();
	
	var_dump($results);
}
```

### **Callbacks**

```php
<?php
require_once 'upload.php';

class validation {
	
	public function check_name_length($object) {
		
		if (mb_strlen($object->file['original_filename']) > 5) {
			
			$object->set_error('File name is too long.');
			
		}

	}
	
}


if (!empty($_FILES['test'])) {
	
	$upload = Upload::factory('important/files');
	$upload->file($_FILES['test']);
	
	$validation = new validation;
	
	$upload->callbacks($validation, array('check_name_length'));
	
	$results = $upload->upload();
	
	var_dump($results);
	
}
```

### **$result dump**

```php 
array
  'status' => boolean false
  'destination' => string 'important/files/' (length=16)
  'size_in_bytes' => int 466028
  'size_in_mb' => float 0.44
  'mime' => string 'application/pdf' (length=15)
  'original_filename' => string 'About Stacks.pdf' (length=16)
  'tmp_name' => string '/private/var/tmp/phpXF2V7o' (length=26)
  'post_data' => 
    array
      'name' => string 'About Stacks.pdf' (length=16)
      'type' => string 'application/pdf' (length=15)
      'tmp_name' => string '/private/var/tmp/phpXF2V7o' (length=26)
      'error' => int 0
      'size' => int 466028
  'errors' => 
    array
      0 => string 'File name is too long.' (length=22)
```

---

```php
$upload->upload();
```

is equivalent
```php
if ($upload->check()) {		
	$upload->save();	
}
$upload->get_state();
```

---
Use this to get validation errors.

```php
$upload->get_errors();
```

---

When upload done you also get new filename and full path

```php
'filename' => string '091755cc57ee634186cd2655c3a0ec990c36f9161322940586' (length=50)
'full_path' => string 'important/files/091755cc57ee634186cd2655c3a0ec990c36f9161322940586' (length=66)
```
