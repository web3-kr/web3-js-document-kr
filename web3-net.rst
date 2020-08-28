.. _net:

========
web3.*.net
========


``web3-net`` 패키지는 이더리움 노드 네트워크 속성과 상호작용을 할 수 있게 합니다.


.. code-block:: javascript

    var Net = require('web3-net');

    // "Personal.providers.givenProvider" 는 이더리움 지원 브라우저에 의해 설정됩니다.
    var net = new Net(Net.givenProvider || 'ws://some.local-or-remote.node:8546');


    // web3 umbrella Package 를 사용하는 방법
    
    var Web3 = require('web3');
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    // -> web3.eth.net
    // -> web3.bzz.net
    // -> web3.shh.net



------------------------------------------------------------------------------


.. include:: include_package-net.rst


------------------------------------------------------------------------------
