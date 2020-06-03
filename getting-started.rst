
===============
Getting Started
===============

web3.js 라이브러리는 이더리움 환경에서 사용할수 있는 여러 기능을 모듈화 해놓은 라이브러리 입니다.

- ``web3-eth`` 는 이더리움 블록체인과 스마트 컨트렉트를 위한 것 입니다.
- ``web3-shh`` 는 p2p 통신을 하기위한 위스퍼 프로토콜 (whisper protocol)을 위한 것 입니다.
- ``web3-bzz`` 는 탈중앙화된 파일 저장을 위한 스왐 프로토콜(swarm protocol) 을 위한 것 입니다
- ``web3-utils`` 는 Dapp 개발자를 위한 많은 종류에 유용한 기능을 포함하고 있습니다.


.. _adding-web3:

web3.js 추가하기
==============

.. index:: npm
.. index:: yarn

먼저, web3.js 를 프로젝트에 임포트 해야합니다. 그것은 밑에 있는 방법들을 사용해서 할 수 있습니다.
- npm: ``npm install web3``
- yarn: ``yarn add web3``
- pure js: link the ``dist/web3.min.js``

그리고, web3 인스턴스를 만듬과 동시에 web3 provider 를 설정해야합니다.
이더리움을 지원 하는 브라우저 (Mist / MetaMask)는 ``ethereumProvider`` 또는 ``web3.currentProvider``를 사용할 수 있습니다. web3.js에서 사용할 수 있는 것은 ``web3.givenProvider`` 가 있습니다.
만약 property 가 ``null`` 이라면, 원격/로컬 노드에 연결해야만 합니다.

.. code-block:: javascript

    // node.js 에서 사용할 때 : var Web3 = require('web3');

    var web3 = new Web3(Web3.givenProvider || "ws://localhost:8545");

끝입니다! 이제 ``web3`` 오브젝트를 사용할 수 있습니다 !!
