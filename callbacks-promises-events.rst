.. _promiEvent:

=========================
Callbacks Promises Events
=========================

이 문서에서는 각자 다른 표준을 가진 모든 유형의 프로젝트에서의 Web3 도입을 위해, 비동기 함수로써 역할을 하는 다양한 방법을 제공합니다.

대부분의 Web3.js 오브젝트들은 마지막 파라미터를 콜백으로써 사용하는것 뿐만 아니라 promises 를 체인함수(chain functions) 에게 반환 할 수 있습니다.

블록체인으로써 이더리움은 다른 단계의 완결성(finality) 를 가지고 있기 때문에 여러 단계(stage)의 행위를 반환해야 합니다

이러한 요구사항에 대처하기 위해, "web3.eth.sendTransaction" 또는 Contract 메소드와 같은 기능에 대해서 "promiEvent"를 반환합니다.

이 "promiEvent" 는 promise와 결합된 이벤트 이미터(event emitter)가 트렌젝션과 같이 블록체인에서의 다른 단계(stage)의 행동을 하는것을 가능하게 하기 위한것입니다.

PromiEvent 는 일반적인 ``on``, ``once`` and ``off`` 함수가 추가된 promise 처럼 작동합니다.
이 방법으로 개발자는, "영수증" 이나 "트렌젝션 해시" 와 같은 추가적인 이벤트를 볼 수 있습니다.

.. code-block:: javascript

    web3.eth.sendTransaction({from: '0x123...', data: '0x432...'})
    .once('transactionHash', function(hash){ ... })
    .once('receipt', function(receipt){ ... })
    .on('confirmation', function(confNumber, receipt){ ... })
    .on('error', function(error){ ... })
    .then(function(receipt){
        // will be fired once the receipt is mined
    });
