.. _eth-abi:

=========
web3.eth.abi
=========

``web3.eth.abi '' 를 사용하면 EVM (Ethereum Virtual Machine)에 대한 함수 호출을 위해 매개 변수를 ABI (Application Binary Interface)로 디코딩 및 인코딩 할 수 있습니다.


------------------------------------------------------------------------------


encodeFunctionSignature
=====================

.. code-block:: javascript

    web3.eth.abi.encodeFunctionSignature(functionName);

함수 이름과 함수 타입의 첫 4 바이트를 sha3를 통해 ABI 서명(ABI Signiture)로 인코딩합니다.
----------
인자(Parameters)
----------

1. ``functionName`` - ``String|Object``: 인코딩 할 함수의 이름입니다
or the :ref:`JSON interface <glossary-json-interface>` object of the function. If string it has to be in the form ``function(type,type,...)``, e.g: ``myFunction(uint256,uint32[],bytes10,bytes)``

-------
반환값
-------

``String`` - The ABI signature of the function.

-------
예제
-------

.. code-block:: javascript

    // From a JSON interface object
    web3.eth.abi.encodeFunctionSignature({
        name: 'myMethod',
        type: 'function',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'string',
            name: 'myString'
        }]
    })
    > 0x24ee0097

    // Or string
    web3.eth.abi.encodeFunctionSignature('myMethod(uint256,string)')
    > '0x24ee0097'


------------------------------------------------------------------------------

encodeEventSignature
=====================

.. code-block:: javascript

    web3.eth.abi.encodeEventSignature(eventName);

Encodes the event name to its ABI signature, which are the sha3 hash of the event name including input types.

----------
인자(Parameters)
----------

1. ``eventName`` - ``String|Object``: The event name to encode.
or the :ref:`JSON interface <glossary-json-interface>` object of the event. If string it has to be in the form ``event(type,type,...)``, e.g: ``myEvent(uint256,uint32[],bytes10,bytes)``

-------
반환값
-------

``String`` - 이벤트의 ABI 서명.

-------
예제
-------

.. code-block:: javascript

    web3.eth.abi.encodeEventSignature('myEvent(uint256,bytes32)')
    > 0xf2eeb729e636a8cb783be044acf6b7b1e2c5863735b60d6daae84c366ee87d97

    // json 인터페이스 오브젝트에서
    web3.eth.abi.encodeEventSignature({
        name: 'myEvent',
        type: 'event',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'bytes32',
            name: 'myBytes'
        }]
    })
    > 0xf2eeb729e636a8cb783be044acf6b7b1e2c5863735b60d6daae84c366ee87d97


------------------------------------------------------------------------------

encodeParameter
=====================

.. code-block:: javascript

    web3.eth.abi.encodeParameter(type, parameter);

유형에 따라 매개 변수를 ABI 표현으로 인코딩합니다.

----------
인자(Parameters)
----------

1. ``type`` - ``String|Object``: 매개 변수의 유형은 유형 목록은`solidity 문서 <http://solidity.readthedocs.io/en/develop/types.html>`_ 를 참조하십시오.
2. ``parameter`` - ``Mixed``: 인코딩 할 실제 매개 변수입니다.

-------
반환값
-------

``String`` - ABI 인코딩된 매개 변수입니다.

-------
예제
-------

.. code-block:: javascript

    web3.eth.abi.encodeParameter('uint256', '2345675643');
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b"

    web3.eth.abi.encodeParameter('uint256', '2345675643');
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b"

    web3.eth.abi.encodeParameter('bytes32', '0xdf3234');
    > "0xdf32340000000000000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameter('bytes', '0xdf3234');
    > "0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000003df32340000000000000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameter('bytes32[]', ['0xdf3234', '0xfdfd']);
    > "00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000002df32340000000000000000000000000000000000000000000000000000000000fdfd000000000000000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameter(
        {
            "ParentStruct": {
                "propertyOne": 'uint256',
                "propertyTwo": 'uint256',
                "childStruct": {
                    "propertyOne": 'uint256',
                    "propertyTwo": 'uint256'
                }
            }
        },
        {
            "propertyOne": 42,
            "propertyTwo": 56,
            "childStruct": {
                "propertyOne": 45,
                "propertyTwo": 78
            }
        }
    );
    > "0x000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e"
------------------------------------------------------------------------------

encodeParameters
=====================

.. code-block:: javascript

    web3.eth.abi.encodeParameters(typesArray, parameters);

:ref:`JSON interface <glossary-json-interface>` 오브젝트를 기반으로 함수 매개 변수를 인코딩합니다.
----------
인자(Parameters)
----------

1. ``typesArray`` - ``Array<String|Object>|Object``: An array with types or a :ref:`JSON interface <glossary-json-interface>` of a function. See the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``parameters`` - ``Array``: 인코딩 할 매개 변수입니다.

-------
반환값
-------

``String`` - ABI 인코딩 된 매개 변수.

-------
예제
-------

.. code-block:: javascript

    web3.eth.abi.encodeParameters(['uint256','string'], ['2345675643', 'Hello!%']);
    > "0x000000000000000000000000000000000000000000000000000000008bd02b7b0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000748656c6c6f212500000000000000000000000000000000000000000000000000"

    web3.eth.abi.encodeParameters(['uint8[]','bytes32'], [['34','434'], '0x324567fff']);
    > "0x0

    web3.eth.abi.encodeParameters(
        [
            'uint8[]',
            {
                "ParentStruct": {
                    "propertyOne": 'uint256',
                    "propertyTwo": 'uint256',
                    "ChildStruct": {
                        "propertyOne": 'uint256',
                        "propertyTwo": 'uint256'
                    }
                }
            }
        ],
        [
            ['34','434'],
            {
                "propertyOne": '42',
                "propertyTwo": '56',
                "ChildStruct": {
                    "propertyOne": '45',
                    "propertyTwo": '78'
                }
            }
        ]
    );
    > "0x00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000018"

------------------------------------------------------------------------------

encodeFunctionCall
=====================

.. code-block:: javascript

    web3.eth.abi.encodeFunctionCall(jsonInterface, parameters);

:ref:`JSON interface <glossary-json-interface>` 오브젝트 와 주어진 매개 변수를 사용하여 함수 호출을 인코딩합니다.
----------
인자(Parameters)
----------

1. ``jsonInterface`` - ``Object``: The :ref:`JSON interface <glossary-json-interface>` object of a function.
2. ``parameters`` - ``Array``: 인코딩 할 매개 변수입니다.

-------
반환값
-------

``String`` - ABI 인코딩된 함수를 호출합니다. 이는 함수의 서명과 인자를 뜻합니다.
[TODO] => The ABI encoded function call. Means function signature + parameters. 
-------
예제
-------

.. code-block:: javascript

    web3.eth.abi.encodeFunctionCall({
        name: 'myMethod',
        type: 'function',
        inputs: [{
            type: 'uint256',
            name: 'myNumber'
        },{
            type: 'string',
            name: 'myString'
        }]
    }, ['2345675643', 'Hello!%']);
    > "0x24ee0097000000000000000000000000000000000000000000000000000000008bd02b7b0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000748656c6c6f212500000000000000000000000000000000000000000000000000"

------------------------------------------------------------------------------

decodeParameter
=====================

.. code-block:: javascript

    web3.eth.abi.decodeParameter(type, hexString);

ABI 인코딩 매개 변수를 JavaScript에서 사용 가능한 타입으로 디코딩합니다.

----------
인자(Parameters)
----------

1. ``type`` - ``String|Object``: The type of the parameter, see the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``hexString`` - ``String``: 디코딩 할 ABI 바이트 코드입니다.

-------
반환값
-------

``Mixed`` - 디코딩 된 매개 변수.

-------
예제
-------

.. code-block:: javascript

    web3.eth.abi.decodeParameter('uint256', '0x0000000000000000000000000000000000000000000000000000000000000010');
    > "16"

    web3.eth.abi.decodeParameter('string', '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > "Hello!%!"

    web3.eth.abi.decodeParameter('string', '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > "Hello!%!"

    web3.eth.abi.decodeParameter(
        {
            "ParentStruct": {
              "propertyOne": 'uint256',
              "propertyTwo": 'uint256',
              "childStruct": {
                "propertyOne": 'uint256',
                "propertyTwo": 'uint256'
              }
            }
        },

    , '0x000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e');
    > {
        '0': {
            '0': '42',
            '1': '56',
            '2': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'childStruct': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'propertyOne': '42',
            'propertyTwo': '56'
        },
        'ParentStruct': {
            '0': '42',
            '1': '56',
            '2': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'childStruct': {
                '0': '45',
                '1': '78',
                'propertyOne': '45',
                'propertyTwo': '78'
            },
            'propertyOne': '42',
            'propertyTwo': '56'
        }
    }

------------------------------------------------------------------------------

decodeParameters
=====================

.. code-block:: javascript

    web3.eth.abi.decodeParameters(typesArray, hexString);

ABI 인코딩 매개 변수를 JavaScript에서 사용 가능한 타입으로 디코딩합니다.
----------
인자(Parameters)
----------

1. ``typesArray`` - ``Array<String|Object>|Object``: An array with types or a :ref:`JSON interface <glossary-json-interface>` outputs array. See the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``hexString`` - ``String``: 디코딩 할 ABI 바이트 코드입니다.

-------
반환값
-------

``Object`` - 디코딩 된 매개 변수를 포함하는 결과 오브젝트입니다.

-------
예제
-------

.. code-block:: javascript

    web3.eth.abi.decodeParameters(['string', 'uint256'], '0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000ea000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > Result { '0': 'Hello!%!', '1': '234' }

    web3.eth.abi.decodeParameters([{
        type: 'string',
        name: 'myString'
    },{
        type: 'uint256',
        name: 'myNumber'
    }], '0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000ea000000000000000000000000000000000000000000000000000000000000000848656c6c6f212521000000000000000000000000000000000000000000000000');
    > Result {
        '0': 'Hello!%!',
        '1': '234',
        myString: 'Hello!%!',
        myNumber: '234'
    }

    web3.eth.abi.decodeParameters([
      'uint8[]',
      {
        "ParentStruct": {
          "propertyOne": 'uint256',
          "propertyTwo": 'uint256',
          "childStruct": {
            "propertyOne": 'uint256',
            "propertyTwo": 'uint256'
          }
        }
      }
    ], '0x00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000038000000000000000000000000000000000000000000000000000000000000002d000000000000000000000000000000000000000000000000000000000000004e0000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000000018');
    > Result {
        '0': ['42', '24'],
        '1': {
            '0': '42',
            '1': '56',
            '2':
                {
                    '0': '45',
                    '1': '78',
                    'propertyOne': '45',
                    'propertyTwo': '78'
                },
            'childStruct':
                {
                    '0': '45',
                    '1': '78',
                    'propertyOne': '45',
                    'propertyTwo': '78'
                },
            'propertyOne': '42',
            'propertyTwo': '56'
        }
    }

------------------------------------------------------------------------------


decodeLog
=====================

.. code-block:: javascript

    web3.eth.abi.decodeLog(inputs, hexString, topics);

ABI 인코딩 된 로그 데이터 및 인덱싱 된 토픽 데이터를 디코딩합니다.

----------
인자(Parameters)
----------

1. ``inputs`` - ``Object``: A :ref:`JSON interface <glossary-json-interface>` inputs array. See the `solidity documentation <http://solidity.readthedocs.io/en/develop/types.html>`_  for a list of types.
2. ``hexString`` - ``String``: The ABI byte code in the ``data`` field of a log.
3. ``topics`` - ``Array``: An array with the index parameter topics of the log, without the topic[0] if its a non-anonymous event, otherwise with topic[0].

-------
반환값
-------

``Object`` - 디코딩 된 매개 변수를 포함하는 결과 오브젝트 입니다.

-------
예제
-------

.. code-block:: javascript


    web3.eth.abi.decodeLog([{
        type: 'string',
        name: 'myString'
    },{
        type: 'uint256',
        name: 'myNumber',
        indexed: true
    },{
        type: 'uint8',
        name: 'mySmallNumber',
        indexed: true
    }],
    '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000748656c6c6f252100000000000000000000000000000000000000000000000000',
    ['0x000000000000000000000000000000000000000000000000000000000000f310', '0x0000000000000000000000000000000000000000000000000000000000000010']);
    > Result {
        '0': 'Hello%!',
        '1': '62224',
        '2': '16',
        myString: 'Hello%!',
        myNumber: '62224',
        mySmallNumber: '16'
    }
