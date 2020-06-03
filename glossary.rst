
========
Glossary
========



.. _glossary-json-interface:

------------------------------------------------------------------------------

json 인터페이스
=====================

json 인터페이스는 json 오브젝트는 이더리움 스마트 계약을 위한 애플리케이션 바이너리 인터페이스(ABI) https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI를 기술합니다.
web3.js 에서 json 인터페이스를 사용하기 위해서는, 스마트 컨트렉트에 대한 오브젝트(:ref:`web3.eth.Contract object <eth-contract>`)와, 이에 대한 Method 와 Event 가 필요합니다 
-------
상세
-------

Functions:

- ``type``: ``"function"``, ``"constructor"`` (can be omitted, defaulting to ``"function"``; ``"fallback"`` also possible but not relevant in web3.js);
- ``name``: the name of the function (only present for function types);
- ``constant``: ``true`` if function is specified to not modify the blockchain state;
- ``payable``: ``true`` if function accepts ether, defaults to ``false``;
- ``stateMutability``: a string with one of the following values: ``pure`` (specified to not read blockchain state), ``view`` (same as ``constant`` above), ``nonpayable`` and ``payable`` (same as ``payable`` above);
- ``inputs``: an array of objects, each of which contains:

  - ``name``: the name of the parameter;
  - ``type``: the canonical type of the parameter.
- ``outputs``: an array of objects same as ``inputs``, can be omitted if no outputs exist.

Events:

- ``type``: 항상 ``"event"`` 입니다;
- ``name``: event의 이름입니다;
- ``inputs``: 오브젝트의 배열입니다, 각각의 요소는:
  - ``name``: 파라미터의 이름입니다;
  - ``type``: 파라미터의 타입입니다;
  - ``indexed``: ``true`` 일때는 필드가 로그 주제의 일부인 경우이고, ``false`` 일때는 로그의 데이터 세그먼트일 때 입니다.
- ``anonymous``: 익명(``anonymous``)으로 선언되어있을때 ``true`` 입니다.


-------
예제
-------

.. code-block:: javascript

    contract Test {
        uint a;
        address d = 0x12345678901234567890123456789012;

        function Test(uint testInt)  { a = testInt;}

        event Event(uint indexed b, bytes32 c);

        event Event2(uint indexed b, bytes32 c);

        function foo(uint b, bytes32 c) returns(address) {
            Event(b, c);
            return d;
        }
    }

    // JSON 결과값
    [{
        "type":"constructor",
        "payable":false,
        "stateMutability":"nonpayable"
        "inputs":[{"name":"testInt","type":"uint256"}],
      },{
        "type":"function",
        "name":"foo",
        "constant":false,
        "payable":false,
        "stateMutability":"nonpayable",
        "inputs":[{"name":"b","type":"uint256"}, {"name":"c","type":"bytes32"}],
        "outputs":[{"name":"","type":"address"}]
      },{
        "type":"event",
        "name":"Event",
        "inputs":[{"indexed":true,"name":"b","type":"uint256"}, {"indexed":false,"name":"c","type":"bytes32"}],
        "anonymous":false
      },{
        "type":"event",
        "name":"Event2",
        "inputs":[{"indexed":true,"name":"b","type":"uint256"},{"indexed":false,"name":"c","type":"bytes32"}],
        "anonymous":false
    }]


------------------------------------------------------------------------------
