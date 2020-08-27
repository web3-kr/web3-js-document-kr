
====
Web3
====


이것은 web3.js 라이브러리의 메인(또는 'umbrella') 클래스 입니다.


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

 object 를 수동으로 인스턴스화 가능하게 object 를 모든 주요 sub modules 의 classes 와 함께 return 합니다.


이것은 모든 주요 하위 모듈의 클래스와 함께 객체를 반환하여 수동으로 인스턴스화 할 수 있게 해줍니다.
-------
반환값
-------


``Object``: Module Constructor 에 대한 리스트
    - ``Eth`` - ``Constructor``: Eth 모듈은 이더리움 네트워크와 통신하는 모듈입니다. :ref:`web3.eth <eth>` 에서 자세한 정보를 확인 할 수 있습니다.
    - ``Net`` - ``Constructor``: Net 모듈은 네트워크 속성과 관련된 모듈입니다. :ref:`web3.eth.net <eth-net>` 에서 자세한 정보를 확인 할 수 있습니다.
    - ``Personal`` - ``Constructor``: Personal 모듈은 이더리움 계정과 관련된 처리를 하는 모듈입니다. :ref:`web3.eth.personal <personal>` 에서 자세한 정보를 확인 할 수 있습니다.
    - ``Shh`` - ``Constructor``: Shh 모듈은 Whisper 프로토콜과 관련된 처리를 하는 모듈입니다. :ref:`web3.shh <shh>` 에서 자세한 정보를 확인 할 수 있습니다.
    - ``Bzz`` - ``Constructor``: Bzz 모듈은 Swarm 네트워크와 관련된 처리를 하는 모듈입니다. :ref:`web3.bzz <bzz>` 에서 자세한 정보를 확인 할 수 있습니다.


-------
예제
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

Web3 인스터스 (Instance)
=============

Web3 class 는 모든 Ethereum 관련 modules 를 담을 수 있는 umbrella package 입니다.

.. code-block:: javascript

    var Web3 = require('web3');

    //"Web3.providers.givenProvider" 는 이더리움 지원 브라우저에 대해 설정 됩니다.
==
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    > web3.eth
    > web3.shh
    > web3.bzz
    > web3.utils
    > web3.version


------------------------------------------------------------------------------

version
============

    Web3 클래스의 정적 액세스(Static Access) 가능한 속성과 인스턴스별 속성 입니다.


.. code-block:: javascript

    Web3.version
    web3.version

web3.js 라이브러리의 현재 패키지 버전을 포함합니다.


-------
반환값
-------

``String``: 현재의 버전 정보

-------
예제
-------

.. code-block:: javascript

    web3.version;
    > "1.2.3"



------------------------------------------------------------------------------


utils(유틸)
=====================


    Web3 클래스의 정적 액세스(Static Access) 가능한 속성과 인스턴스별 속성 입니다.

.. code-block:: javascript

    Web3.utils
    web3.utils

유틸리티 함수는 Web3 클래스 객체에 직접적으로 노출 됩니다.
자세한 정보는 :ref:`web3.utils <utils>`에서 확인 할 수 있습니다.



------------------------------------------------------------------------------


.. include:: include_package-core.rst


------------------------------------------------------------------------------
