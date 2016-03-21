---
title: Merging product attributes in Magento
tags:
    - Magento
    - PHP
---

Another magento snippet. With this you can merge product attributes:

	// create product collection
	$collection = Mage::getModel('catalog/product')
	    ->getCollection();
	// filter by attribute code
	$collection->addAttributeToFilter($attributeCode, $from);
	// get all product ids...
	$ids = $collection->getAllIds();
	// ...and update them
	Mage::getSingleton('catalog/product_action')
	    ->updateAttributes($ids, [$attributeCode => $to], 0);

Don't forget to initialize the following variables:

- `$attributeCode` - the code of the attribute, eg `color`.
- `$from` - the attribute value id you want to merge from, eg `145` for Cyan.
- `$to` - the attribute value id you want to merge to, eg `146` for Blue.

After that, reindex and drop the old attribute value.
