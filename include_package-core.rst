

setProvider
=====================

.. code-block:: javascript

    web3.setProvider(myProvider)
    web3.eth.setProvider(myProvider)
    web3.shh.setProvider(myProvider)
    web3.bzz.setProvider(myProvider)
    ...

해당 모듈의 web3 Provider를 변경 또는 설정 합니다.
.. note::
``web3.eth``, ``web3.shh`` 등등과 같은 모든 하위 모듈에 대해 동일한 Provider가 설정됩니다. ``web3.bzz`` 는 분리된 provider가 예외적으로 적용됩니다.

----------
매개변수(Parameters)
----------

1. ``Object`` - ``myProvider``: :ref:Provider를 검증합니다. <web3-providers>.

-------
반환값 (Return)
-------

``Boolean``

-------
예시 (예시 (Example))
-------

.. code-block:: javascript

    var Web3 = require('web3');
    var web3 = new Web3('http://localhost:8545');
    // 또는
    var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

    // provider를 변경합니다.
    web3.setProvider('ws://localhost:8546');

    // 또는

    web3.setProvider(new Web3.providers.WebsocketProvider('ws://localhost:8546'));

    // node.js 에서 IPC Provider를 사용합니다.
    var net = require('net');
    var web3 = new Web3('/Users/myuser/Library/Ethereum/geth.ipc', net); // mac os 의 경로

    // 또는

    var web3 = new Web3(new Web3.providers.IpcProvider('/Users/myuser/Library/Ethereum/geth.ipc', net)); // mac os 경로
    // 윈도우의 경로 : "\\\\.\\pipe\\geth.ipc"
    // 리눅스의 경로: "/users/myuser/.ethereum/geth.ipc"


------------------------------------------------------------------------------

providers(프로바이더)
=====================

.. code-block:: javascript

    web3.providers
    web3.eth.providers
    web3.shh.providers
    web3.bzz.providers
    ...

:ref:`providers <web3-providers>` 를 포함하고 있습니다.
----------
반환값 (Return)
----------

``Object`` with the following providers:

    - ``Object`` - ``HttpProvider``:http 프로바이더는 **더이상 사용되지 않게** 되었습니다. 이것은 구독에 사용할 수 없을 것 입니다.
    - ``Object`` - ``WebsocketProvider``: 웹 소켓 프로바이더는 레거시 브라우저에 대한 표준입니다.
    - ``Object`` - ``IpcProvider``: IPC 프로바이더는 로컬노드를 사용하는 nodejs Dapp에 대한 표준입니다. 가장 안전한 연결을 제공합니다.

-------
예시 (예시 (Example))
-------

.. code-block:: javascript

    var Web3 = require('web3');
    // use the given Provider, e.g in Mist, or instantiate a new websocket provider
    //
    var web3 = new Web3(Web3.givenProvider || 'ws://remotenode.com:8546');
    // or
    var web3 = new Web3(Web3.givenProvider || new Web3.providers.WebsocketProvider('ws://remotenode.com:8546'));

    // Using the IPC provider in node.js
    var net = require('net');

    var web3 = new Web3('/Users/myuser/Library/Ethereum/geth.ipc', net); // mac os path
    // or
    var web3 = new Web3(new Web3.providers.IpcProvider('/Users/myuser/Library/Ethereum/geth.ipc', net)); // mac os path
    // on windows the path is: "\\\\.\\pipe\\geth.ipc"
    // on linux the path is: "/users/myuser/.ethereum/geth.ipc"

-------------
설정하기
-------------

.. code-block:: javascript

    // ====
    // Http
    // ====

    var Web3HttpProvider = require('web3-providers-http');

    var options = {
        keepAlive: true,
        withCredentials: false,
        timeout: 20000, // ms
        headers: [
            {
                name: 'Access-Control-Allow-Origin',
                value: '*'
            },
            {
                ...
            }
        ],
        agent: {
            http: http.Agent(...),
            baseUrl: ''
        }
    };

    var provider = new Web3HttpProvider('http://localhost:8545', options);

    // ==========
    // 웹소켓
    // ==========

    var Web3WsProvider = require('web3-providers-ws');

    var options = {
        timeout: 30000, // ms

        // 크레덴셜을 url에 포함해서 사용할 수 있습니다 ex: ws://username:password@localhost:8546
        headers: {
          authorization: 'Basic username:password'
        },

        // 만약 결과값이 크다면 사용할 수 있습니다.
        clientConfig: {
          maxReceivedFrameSize: 100000000,   // bytes - default: 1MiB
          maxReceivedMessageSize: 100000000, // bytes - default: 8MiB
        },

        // 자동 재연결 활성화 
        reconnect: {
            auto: true,
            delay: 5000, // ms
            maxAttempts: 5,
            onTimeout: false
        }
    };

    var ws = new Web3WsProvider('ws://localhost:8546', options);


HTTP 와 Websocket 프로바이더에 대한 정보를 더 얻으려면 밑에 링크를 참고할 수 있습니다.
    - `HttpProvider <https://github.com/ethereum/web3.js/tree/1.x/packages/web3-providers-http#usage>`_
    - `WebsocketProvider <https://github.com/ethereum/web3.js/tree/1.x/packages/web3-providers-ws#usage>`_

------------------------------------------------------------------------------

givenProvider
=====================

.. code-block:: javascript

    web3.givenProvider
    web3.eth.   
    web3.shh.givenProvider
    web3.bzz.givenProvider
    ...

이더리움 호환 브라우저에서 web3.js 를 사용하면, 이 함수는 해당 브라우저의 네이티브 프로바이더를 반환합니다. 
호환 브라우저가 아닐 경우, ``null`` 을 반환합니다.

-------
반환값 (Returns)
-------

``Object``: 설정된 givenProvider 또는 ``null``.;

-------
예시 (Example)
-------

.. code-block:: javascript
    web3.setProvider(web3.givenProvider || "ws://remotenode.com:8546");

------------------------------------------------------------------------------


currentProvider
=====================

.. code-block:: javascript

    web3.currentProvider
    web3.eth.currentProvider
    web3.shh.currentProvider
    web3.bzz.currentProvider
    ...

현재 provider 또는 ``null`` 을 반환합니다

-------
반환값 (Returns)
-------

``Object``: 현재 설정된 프로바이더 또는 ``null``;

-------
예시 (Example)
-------

.. code-block:: javascript
    if(!web3.currentProvider) {
        web3.setProvider("http://localhost:8545");
    }

------------------------------------------------------------------------------

BatchRequest
=====================

.. code-block:: javascript

    new web3.BatchRequest()
    new web3.eth.BatchRequest()
    new web3.shh.BatchRequest()
    new web3.bzz.BatchRequest()

Batch 요청을 만들고 실행하는 클래스 입니다.
----------
인자 (Parameters)
----------

없음

-------
반환값 (Returns)
-------

``Object``: 밑에 두 메소드로 이루어져 있습니다:

    - ``add(request)``: batch call에 요청 오브젝트를 추가합니다.
    - ``execute()``: batch 요청을 실행합니다.

-------
예시 (Example)
-------

.. code-block:: javascript

    var contract = new web3.eth.Contract(abi, address);

    var batch = new web3.BatchRequest();
    batch.add(web3.eth.getBalance.request('0x0000000000000000000000000000000000000000', 'latest', callback));
    batch.add(contract.methods.balance(address).call.request({from: '0x0000000000000000000000000000000000000000'}, callback2));
    batch.execute();


------------------------------------------------------------------------------

extend
=====================

.. code-block:: javascript

    web3.extend(methods)
    web3.eth.extend(methods)
    web3.shh.extend(methods)
    web3.bzz.extend(methods)
    ...

web3 모듈을 확장할 수 있게 합니다.

----------
인자
----------

1. ``methods`` - ``Object``: 메서드 배열이 있는 확장 개체에서는 개체를 아래와 같이 설명한다:
    - ``property`` - ``String``: (optional) 모듈에 추가할 속성의 이름. 설정된 속성이 없는 경우 모듈에 직접 추가됨.
    - ``methods`` - ``Array``: 메서드 설명 배열
        - ``name`` - ``String``: 추가할 메서드의 이름.
        - ``call`` - ``String``: RPC 메서드의 이름.
        - ``params`` - ``Number``: (optional) 함수에 대한 파라미터의 갯수, 기본값은 0.
        - ``inputFormatter`` - ``Array``: (optional) 입력 포맷터 함수 배열. 각 어레이 항목은 함수 매개 변수에 응답하므로 일부 매개 변수를 포맷하지 않으려면 대신 ``null`` 을 추가하세요.
        - ``outputFormatter - ``Function``: (optional) 메서드의 출력을 포맷하는 데 사용할 수 있다.


----------
반환값 (Returns)
----------

``Object``: 확장 모듈을 반환합니다.

-------
예시 (Example)
-------

.. code-block:: javascript

    web3.extend({
        property: 'myModule',
        methods: [{
            name: 'getBalance',
            call: 'eth_getBalance',
            params: 2,
            inputFormatter: [web3.extend.formatters.inputAddressFormatter, web3.extend.formatters.inputDefaultBlockNumberFormatter],
            outputFormatter: web3.utils.hexToNumberString
        },{
            name: 'getGasPriceSuperFunction',
            call: 'eth_gasPriceSuper',
            params: 2,
            inputFormatter: [null, web3.utils.numberToHex]
        }]
    });

    web3.extend({
        methods: [{
            name: 'directCall',
            call: 'eth_callForFun',
        }]
    });

    console.log(web3);
    > Web3 {
        myModule: {
            getBalance: function(){},
            getGasPriceSuperFunction: function(){}
        },
        directCall: function(){},
        eth: Eth {...},
        bzz: Bzz {...},
        ...
    }


------------------------------------------------------------------------------
