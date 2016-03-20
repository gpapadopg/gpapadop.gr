---
title: Using magento API to get order details
tags:
    - Magento
---

With the following snippet you can get order details from the magento API:

    <?php
    $proxy = new SoapClient('http://your-magento-site/index.php/api/soap/?wsdl=1');
    $session = $proxy->login('SOAP-API-USER', 'SOAP-API-PASSWORD');
    $orders = $proxy->call($session, 'sales_order.list', [
        [
            'entity_id' => [2595],                 // filter by order id
            //'status' => ['in' => ['Canceled']],  // or by status
            //'state'  => ['in' => ['canceled']],  // or by state, etc
        ]
    ]);
    var_dump($orders);
    foreach ($orders as $order) {
        // fetch information about each order
        $order = $proxy->call($session, 'sales_order.info', $order['increment_id']);
        var_dump($order);
    }

You must have an api user with correct permissions in order for this to work 
(look under "System" > "Web Services").
