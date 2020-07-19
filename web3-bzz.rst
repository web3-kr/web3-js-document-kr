.. _bzz:

========
web3.bzz
========

.. 노트 ::이 API는 시간이 완전히 완성된 것이 아니므로, 추후 변경 될 수 있습니다.


``web3-bzz`` 는 탈중앙화된 파일 저장을 위해 Swarm을 사용할 수 있게 해줍니다.
자세한 정보는 `Swarm 문서 <http://swarm-guide.readthedocs.io/en/latest/>`_ 를 참고하세요.


.. code-block:: javascript

    var Bzz = require('web3-bzz');

    // "ethereum" 객체(Objcet)가 있는지 자동 감지하여 로컬 swarm 노드 또는 swarm-gateways.net에 연결합니다.    
    // 옵션으로 자신의 프로바이더 URL을 제공 할 수 있습니다. 프로바이더 URL이 제공되지 않으면 기본적으로 "http://swarm-gateways.net"을 사용합니다.
    var bzz = new Bzz(Bzz.givenProvider || 'http://swarm-gateways.net');


    // or using the web3 umbrella package

    var Web3 = require('web3');
    var web3 = new Web3(Web3.givenProvider || 'ws://some.local-or-remote.node:8546');

    // -> web3.bzz.currentProvider // if Web3.givenProvider was an ethereum provider it will set: "http://localhost:8500" otherwise it will set: "http://swarm-gateways.net"

    // 필요한 경우 프로바이더를 수동으로 설정
    web3.bzz.setProvider("http://localhost:8500");


------------------------------------------------------------------------------


setProvider
=====================

.. code-block:: javascript

    web3.bzz.setProvider(myProvider)

Will change the provider for its module.

.. note:: umbrella 패키지``web3 ''에서 호출되면 별도로 제공 해야하는``web3.bzz`` 를 제외한 모든 하위 모듈``web3.eth '',``web3.shh ''에 대한 프로바이더를 설정합니다.


----------
인자(Parameters)
----------

1. ``Object`` - ``myProvider``: :ref:`유효한 프로바이더 <web3-providers>`.

-------
반환값
-------

``Boolean``

-------
예제
-------

.. code-block:: javascript

    var Bzz = require('web3-bzz');
    var bzz = new Bzz('http://localhost:8500');

    // 프로바이더를 변경합니다.
    bzz.setProvider('http://swarm-gateways.net');


------------------------------------------------------------------------------

givenProvider
=====================

.. code-block:: javascript

    web3.bzz.givenProvider

Ethereum 호환 브라우저에서 web3.js를 사용하면 해당 브라우저에서 현재 기본 프로바이더로 설정됩니다.
브라우저 환경에서 지정된 공급자를 반환하지 않으면 ``null`` 을 반환합니다.


-------
반환값
-------

``Object``: 설정된 프로바이더 또는 ``null``;

-------
예제
-------

.. code-block:: javascript

    bzz.givenProvider;
    > {
        send: function(),
        on: function(),
        bzz: "http://localhost:8500",
        shh: true,
        ...
    }

    bzz.setProvider(bzz.givenProvider || "http://swarm-gateways.net");


------------------------------------------------------------------------------


currentProvider
=====================

.. code-block:: javascript

    bzz.currentProvider

위 함수는 현재의 프로바이더 URL을 제공합니다. 프로바이더가 없을 경우 ``null`` 을 반환합니다.

-------
반환값
-------

``Object``: The current provider URL or ``null``;

-------
예제
-------

.. code-block:: javascript

    bzz.currentProvider;
    > "http://localhost:8500"


    if(!bzz.currentProvider) {
        bzz.setProvider("http://swarm-gateways.net");
    }


------------------------------------------------------------------------------


upload
=====================

.. code-block:: javascript

   web3.bzz.upload(mixed)

파일, 폴더 또는 raw 데이터를 swarm에 업로드합니다.

----------
인자(Parameters)
----------

1. ``mixed`` - ``String|Buffer|Uint8Array|Object``: 파일 내용은 Buffer / Uint8Array 로 업로드 가능합니다, 여러 파일 또는 디렉토리 또는 파일에는 다음 유형이 허용됩니다(node.js에서만 가능).
    - ``String|Buffer|Uint8Array``: 업로드 할 파일 내용, Uint8Array 또는 Buffer 입니다.

    - ``Object``:
        1. 파일과 디렉토리에 대한 여러 키 값. 경로는 동일하게 유지됩니다.
                -키는 파일 경로 또는 이름이어야합니다 (예 : `` "/foo.txt"``이며 그 값은 다음과 같은 객체입니다.)
                 -``type '': 파일의 MIME 유형 (예 : `` "text / html"``).
                 -``data '': 업로드 할 파일 내용, 파일 Uint8Array 또는 Buffer.
         2. Node.js의 디스크에서 파일 또는 디렉토리를 업로드하십시오. 다음 속성이 필요합니다..
             -``경로 '': 파일 또는 디렉토리의 경로입니다.
             -``kind '': `` "directory" '',`` "file" ''또는`` "data"`` 3가지 유형으로 나누어집니다..
             -``defaultFile ''(선택 사항) : "directory" 일 때 "defaultFile"의 경로 (예 : `` "/index.html"``.)
         3. 브라우저에서 파일 또는 폴더를 업로드하십시오.
             -``pick '': 시작할 파일 선택기. `` "file"``,`` "directory" ''또는`` "data"``를 사용할 수 있습니다.

-------
반환값
-------

``Promise`` returning ``String``: 매니페스트의 콘텐츠 해시를 반환합니다.



-------
예제
-------

.. code-block:: javascript

    var bzz = web3.bzz;

    // raw 데이터
    bzz.upload("test file").then(function(hash) {
        console.log("Uploaded file. Address:", hash);
    })

    // raw 폴더
    var dir = {
        "/foo.txt": {type: "text/plain", data: "sample file"},
        "/bar.txt": {type: "text/plain", data: "another file"}
    };
    bzz.upload(dir).then(function(hash) {
        console.log("Uploaded directory. Address:", hash);
    });

    // 노드에서 디스크 파일 업로드하기
    bzz.upload({
        path: "/path/to/thing",      // 업로드할 경로
        kind: "directory",           // 파일인지, 폴더인지 구분 ('file','directory')
        defaultFile: "/index.html"   // 선택적이며 "directory" 에 대해서만 사용 가능한 인자. 

    })
    .then(console.log)
    .catch(console.log);

    // 브라우저에서 디스크 파일 업로드
    bzz.upload({pick: "file"}) // 파일인지, 폴더인지 구분 ('file','directory')
    .then(console.log);

------------------------------------------------------------------------------

download
=====================

.. code-block:: javascript

   web3.bzz.download(bzzHash [, localpath])

swarm에서 파일 또는 폴더를 버퍼로 또는 디스크로 다운로드합니다 (node.js 에만 해당).
----------
Parameters
----------

1. ``bzzHash`` - ``String``: 다운로드 할 파일 또는 디렉토리입니다. 해시가 원시 파일 인 경우 버퍼를 반환하고 매니페스트 파일 인 경우 디렉토리 구조를 반환합니다. ``localpath ''가 주어지면 파일을 다운로드 한 경로를 반환합니다.
2. ``localpath`` - ``String``: 컨텐츠를 다운로드 할 로컬 폴더입니다. (node.js 만)

-------
반환값
-------

``Promise`` returning ``Buffer|Object|String``: 다운로드 한 파일의 버퍼, 디렉토리 구조의 오브젝트 또는 다운로드 된 경로.


-------
예제
-------

.. code-block:: javascript

    var bzz = web3.bzz;

// raw 파일 다운로드
    var fileHash = "a5c10851ef054c268a2438f10a21f6efe3dc3dcdcc2ea0e6a1a7a38bf8c91e23";
    bzz.download(fileHash).then(function(buffer) {
        console.log("Downloaded file:", buffer.toString());
    });

// 해시가 매니페스트 파일 인 경우 디렉토리를 다운로드
    var dirHash = "7e980476df218c05ecfcb0a2ca73597193a34c5a9d6da84d54e295ecd8e0c641";
    bzz.download(dirHash).then(function(dir) {
        console.log("Downloaded directory:");
        > {
            'bar.txt': { type: 'text/plain', data: <Buffer 61 6e 6f 74 68 65 72 20 66 69 6c 65> },
            'foo.txt': { type: 'text/plain', data: <Buffer 73 61 6d 70 6c 65 20 66 69 6c 65> }
        }
    });

// 파일 / 디렉토리를 디스크로 다운로드 (node.js 에서만 사용가능)
    var dirHash = "a5c10851ef054c268a2438f10a21f6efe3dc3dcdcc2ea0e6a1a7a38bf8c91e23";
    bzz.download(dirHash, "/target/dir")
    .then(path => console.log(`Downloaded directory to ${path}.`))
    .catch(console.log);


------------------------------------------------------------------------------


pick
=====================

.. code-block:: javascript

   web3.bzz.pick.file()
   web3.bzz.pick.directory()
   web3.bzz.pick.data()

브라우저에서 파일 선택기를 열어 파일, 디렉토리 또는 데이터를 선택합니다.

----------
인자(Parameters)
----------

없음

-------
반환값
-------

``Promise`` returning ``Object``: 파일 또는 여러 파일을 반환합니다.

-------
예제
-------

.. code-block:: javascript

    web3.bzz.pick.file()
    .then(console.log);
    > {
        ...
    }
