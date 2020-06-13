
====
Web3
====

이 것은 web3.js library main(또는 'umbrella') class 이다.

.. code-block:: javascript

    var Web3 = require('web3');

    > Web3.utils
    > Web3.version
    > Web3.givenProvider
    > Web3.providers
    > Web3.modules

------------------------------------------------------------------------------

Web3.modules
=====================

.. code-block:: javascript

    Web3.modules

 object 를 수동으로 인스턴스화 가능하게 object 를 모든 주요 sub modules 의 classes 와 함께 return 한다.

-------
Returns
-------

``Object``: module constructors 의 list:
    - ``Eth`` - ``Constructor``: Ethereum network와 상호작용하기 위한 Eth module 추가정보 :ref:`web3.eth <eth>`
    - ``Net`` - ``Constructor``: network properties 와 상호작용하기 위한 Net module 추가정보 :ref:`web3.eth.net <eth-net>`
    - ``Personal`` - ``Constructor``: Ethereum accounts 와 상호작용하기 위한 Personal module 추가정보 :ref:`web3.eth.personal <personal>` for more.
    - ``Shh`` - ``Constructor``: whisper protocol 과 상호작용하기 위한 Shh module 추가정보 :ref:`web3.shh <shh>`
    - ``Bzz`` - ``Constructor``: swarm network 와 상호작용하기 위한 Bzz module 추가정보 :ref:`web3.bzz <bzz>`

-------
Example
-------

.. code-block:: javascript

    Web3.modules
    > {
        Eth: Eth(provider),
        Net: Net(provider),
        Personal: Personal(provider),
        Shh: Shh(provider),
        Bzz: Bzz(provider),
    }


------------------------------------------------------------------------------

Web3 Instance
=============

Web3 class 는 모든 Ethereum 관련 modules 를 담을 수 있는 umbrella package 이다.

.. code-block:: javascript

    var Web3 = require('web3');

    // "Web3.providers.givenProvider" 는 Ethereum 지원 브라우저일 경우 설정된다.
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    > web3.eth
    > web3.shh
    > web3.bzz
    > web3.utils
    > web3.version


------------------------------------------------------------------------------

version
============

    Web3 class 의 Static accessible property 및 instance 의 property

.. code-block:: javascript

    Web3.version
    web3.version

Web3.js library의 current package version 을 포함한다.

-------
Returns
-------

``String``: The current version.

-------
Example
-------

.. code-block:: javascript

    web3.version;
    > "1.2.3"



------------------------------------------------------------------------------


utils
=====================

    Web3 class 의 Static accessible property 및 instance 의 property

.. code-block:: javascript

    Web3.utils
    web3.utils

Utility functions 도 ``Web3`` class object 에 직접 노출된다.

추가정보 :ref:`web3.utils <utils>`


------------------------------------------------------------------------------


.. include:: include_package-core.rst


------------------------------------------------------------------------------
