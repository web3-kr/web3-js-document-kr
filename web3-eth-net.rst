.. _eth-net:

=========
web3.eth.net
=========


현재 네트워크에 대한 정보를 수신하는 함수를 포함합니다.

------------------------------------------------------------------------------


.. include:: include_package-net.rst


------------------------------------------------------------------------------

getNetworkType
=====================

.. code-block:: javascript

    web3.eth.net.getNetworkType([callback])

제네시스 해시를 비교하여 노드가 연결된 체인을 추측한다.

.. note:: 현재 연결된 체인을 감지하려면 :ref:`web3.eth.getChainId <eth-chainId>` 방법을 사용하는 것이 좋습니다.

-------
반환값 (Returns)
-------

``Promise`` returns ``String``:
    - ``"main"`` 메인 네트워크에 대한 반환
    - ``"morden"`` 모던 테스트 네트워크에 대한 반환
    - ``"ropsten"`` 모던 테스트 네트워크에 대한 반환
    - ``"private"`` 감지할 수 없는 네트워크에 대한 반환


-------
예시 (Example)
-------

.. code-block:: javascript

    web3.eth.net.getNetworkType()
    .then(console.log);
    > "main"


