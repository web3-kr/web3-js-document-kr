.. _eth-subscribe:

=========
web3.eth.subscribe
=========

 ``web3.eth.subscribe`` 함수는 블록체인의 특정 이벤트를 구독할 수 있게 해줍니다.


subscribe
=====================

.. code-block:: javascript

    web3.eth.subscribe(type [, options] [, callback]);

----------
Parameters
----------

1. ``String`` - 구독하고자 하는 이벤트
2. ``Mixed`` - (optional) 구독 유형에 따른 선택적 인자
3. ``Function`` - (optional) 옵셔널 콜백, 첫 번째 매개 변수로 오류 개체를 반환하고 두 번째 매개 변수로 결과를 반환합니다. 또한 들어오는 각각의 구독에 대해 호출되며 구독 자체는 3개의 매개 변수로 호출됩니다


.. _eth-subscription-return:

-------
Returns
-------

``EventEmitter`` - 서브스크립션 개체

    - ``subscription.id``: 구독을 식별하고 구독을 취소하는 데 사용되는 구독 ID.
    - ``subscription.subscribe([callback])``: 동일한 파라미터로 재구독하는 데 사용할 수 있습니다.
    - ``subscription.unsubscribe([callback])``: 구독을 취소하고 성공하면 콜백에 TRUE를 반환합니다.
    - ``subscription.arguments``: 다시 구독할 때 사용되는 구독 인자 입니다.
    - ``on("data")`` returns ``Object``: 로그 개체를 인수로 하여 들어오는 각 로그에서 실행합니다.
    - ``on("changed")`` returns ``Object``: Fires on each log which was removed from the blockchain. The log will have the additional property ``"removed: true"``.
    - ``on("error")`` returns ``Object``: Fires when an error in the subscription occurs.
    - ``on("connected")`` returns ``String``: Fires once after the subscription successfully connected. Returns the subscription id.

----------------
알림 반환값
----------------

- ``Mixed`` - 구독한 이벤트에 따라 다른 결과.

-------
Example
-------

.. code-block:: javascript

    var subscription = web3.eth.subscribe('logs', {
        address: '0x123456..',
        topics: ['0x12345...']
    }, function(error, result){
        if (!error)
            console.log(result);
    });

    // 구독 취소
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });


------------------------------------------------------------------------------


clearSubscriptions
=====================

.. code-block:: javascript

    web3.eth.clearSubscriptions()

구독 초기화

..note:: "web3-ssh"와 같은 다른 패키지의 구독을 리셋하지는 않을 것 입니다. 그런 패키지들은 각각의 요청매니저(RequestManager)를 사용합니다

----------
Parameters
----------

1. ``Boolean``: 구독을 계속 불러오고 있을경우 ``true`` 를 반환합니다.

-------
Returns
-------

``Boolean``

-------
Example
-------

.. code-block:: javascript

    web3.eth.subscribe('logs', {} ,function(){ ... });

    ...

    web3.eth.clearSubscriptions();


------------------------------------------------------------------------------


subscribe("pendingTransactions")
=====================

.. code-block:: javascript

    web3.eth.subscribe('pendingTransactions' [, callback]);

들어오는 보류중인 트렌젝션을 구독하는 함수입니다.
----------
Parameters
----------

1. ``String`` - ``"pendingTransactions"``, 구독의 종류
2. ``Function`` - (optional) 옵셔널 콜백, 첫 번째 매개 변수로 오류 개체를 반환하고 두 번째 매개 변수로 결과를 반환합니다. 또한 들어오는 각 구독에 대해 호출될 것입니다.

-------
Returns
-------

``EventEmitter``: An :ref:`subscription instance <eth-subscription-return>` as an event emitter with the following events:

- ``"data"`` returns ``String``: Fires on each incoming pending transaction and returns the transaction hash.
- ``"error"`` returns ``Object``: Fires when an error in the subscription occurs.

----------------
Notification returns
----------------

1. ``Object|Null`` - First parameter is an error object if the subscription failed.
2. ``String`` - Second parameter is the transaction hash.

-------
Example
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('pendingTransactions', function(error, result){
        if (!error)
            console.log(result);
    })
    .on("data", function(transaction){
        console.log(transaction);
    });

    // unsubscribes the subscription
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });


------------------------------------------------------------------------------


subscribe("newBlockHeaders")
=====================

.. code-block:: javascript

    web3.eth.subscribe('newBlockHeaders' [, callback]);

Subscribes to incoming block headers. This can be used as timer to check for changes on the blockchain.

----------
Parameters
----------

1. ``String`` - ``"newBlockHeaders"``, the type of the subscription.
2. ``Function`` - (optional) Optional callback, returns an error object as first parameter and the result as second. Will be called for each incoming subscription.

-------
Returns
-------

``EventEmitter``: An :ref:`subscription instance <eth-subscription-return>` as an event emitter with the following events:

- ``"data"`` returns ``Object``: Fires on each incoming block header.
- ``"error"`` returns ``Object``: Fires when an error in the subscription occurs.
- ``"connected"`` returns ``Number``: Fires once after the subscription successfully connected. Returns the subscription id.

The structure of a returned block header is as follows:

    - ``number`` - ``Number``: The block number. ``null`` when its pending block.
    - ``hash`` 32 Bytes - ``String``: Hash of the block. ``null`` when its pending block.
    - ``parentHash`` 32 Bytes - ``String``: Hash of the parent block.
    - ``nonce`` 8 Bytes - ``String``: Hash of the generated proof-of-work. ``null`` when its pending block.
    - ``sha3Uncles`` 32 Bytes - ``String``: SHA3 of the uncles data in the block.
    - ``logsBloom`` 256 Bytes - ``String``: The bloom filter for the logs of the block. ``null`` when its pending block.
    - ``transactionsRoot`` 32 Bytes - ``String``: The root of the transaction trie of the block
    - ``stateRoot`` 32 Bytes - ``String``: The root of the final state trie of the block.
    - ``receiptsRoot`` 32 Bytes - ``String``: The root of the receipts.
    - ``miner`` - ``String``: The address of the beneficiary to whom the mining rewards were given.
    - ``extraData`` - ``String``: The "extra data" field of this block.
    - ``gasLimit`` - ``Number``: The maximum gas allowed in this block.
    - ``gasUsed`` - ``Number``: The total used gas by all transactions in this block.
    - ``timestamp`` - ``Number``: The unix timestamp for when the block was collated.

----------------
Notification returns
----------------

1. ``Object|Null`` - First parameter is an error object if the subscription failed.
2. ``Object`` - The block header object like above.

-------
Example
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('newBlockHeaders', function(error, result){
        if (!error) {
            console.log(result);

            return;
        }

        console.error(error);
    })
    .on("connected", function(subscriptionId){
        console.log(subscriptionId);
    })
    .on("data", function(blockHeader){
        console.log(blockHeader);
    })
    .on("error", console.error);

    // unsubscribes the subscription
    subscription.unsubscribe(function(error, success){
        if (success) {
            console.log('Successfully unsubscribed!');
        }
    });

------------------------------------------------------------------------------


subscribe("syncing")
=====================

.. code-block:: javascript

    web3.eth.subscribe('syncing' [, callback]);

Subscribe to syncing events. This will return an object when the node is syncing and when its finished syncing will return ``FALSE``.

----------
Parameters
----------

1. ``String`` - ``"syncing"``, the type of the subscription.
2. ``Function`` - (optional) Optional callback, returns an error object as first parameter and the result as second. Will be called for each incoming subscription.

-------
Returns
-------

``EventEmitter``: An :ref:`subscription instance <eth-subscription-return>` as an event emitter with the following events:

- ``"data"`` returns ``Object``: Fires on each incoming sync object as argument.
- ``"changed"`` returns ``Object``: Fires when the synchronisation is started with ``true`` and when finished with ``false``.
- ``"error"`` returns ``Object``: Fires when an error in the subscription occurs.

For the structure of a returned event ``Object`` see :ref:`web3.eth.isSyncing return values <eth-issyncing-return>`.

----------------
Notification returns
----------------

1. ``Object|Null`` - First parameter is an error object if the subscription failed.
2. ``Object|Boolean`` - The syncing object, when started it will return ``true`` once or when finished it will return `false` once.

-------
Example
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('syncing', function(error, sync){
        if (!error)
            console.log(sync);
    })
    .on("data", function(sync){
        // show some syncing stats
    })
    .on("changed", function(isSyncing){
        if(isSyncing) {
            // stop app operation
        } else {
            // regain app operation
        }
    });

    // unsubscribes the subscription
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });

------------------------------------------------------------------------------


subscribe("logs")
=====================

.. code-block:: javascript

    web3.eth.subscribe('logs', options [, callback]);

Subscribes to incoming logs, filtered by the given options.
If a valid numerical ``fromBlock`` options property is set, Web3 will retrieve logs beginning from this point, backfilling the response as necessary.

----------
Parameters
----------

1. ``"logs"`` - ``String``, the type of the subscription.
2. ``Object`` - The subscription options
  - ``fromBlock`` - ``Number``: The number of the earliest block. By default ``null``.
  - ``address`` - ``String|Array``: An address or a list of addresses to only get logs from particular account(s).
  - ``topics`` - ``Array``: An array of values which must each appear in the log entries. The order is important, if you want to leave topics out use ``null``, e.g. ``[null, '0x00...']``. You can also pass another array for each topic with options for that topic e.g. ``[null, ['option1', 'option2']]``
3. ``callback`` - ``Function``: (optional) Optional callback, returns an error object as first parameter and the result as second. Will be called for each incoming subscription.

-------
Returns
-------

``EventEmitter``: An :ref:`subscription instance <eth-subscription-return>` as an event emitter with the following events:

- ``"data"`` returns ``Object``: Fires on each incoming log with the log object as argument.
- ``"changed"`` returns ``Object``: Fires on each log which was removed from the blockchain. The log will have the additional property ``"removed: true"``.
- ``"error"`` returns ``Object``: Fires when an error in the subscription occurs.
- ``"connected"`` returns ``Number``: Fires once after the subscription successfully connected. Returns the subscription id.

For the structure of a returned event ``Object`` see :ref:`web3.eth.getPastEvents return values <eth-getpastlogs-return>`.

----------------
Notification returns
----------------

1. ``Object|Null`` - First parameter is an error object if the subscription failed.
2. ``Object`` - The log object like in :ref:`web3.eth.getPastEvents return values <eth-getpastlogs-return>`.

-------
Example
-------


.. code-block:: javascript

    var subscription = web3.eth.subscribe('logs', {
        address: '0x123456..',
        topics: ['0x12345...']
    }, function(error, result){
        if (!error)
            console.log(result);
    })
    .on("connected", function(subscriptionId){
        console.log(subscriptionId);
    })
    .on("data", function(log){
        console.log(log);
    })
    .on("changed", function(log){
    });

    // unsubscribes the subscription
    subscription.unsubscribe(function(error, success){
        if(success)
            console.log('Successfully unsubscribed!');
    });
