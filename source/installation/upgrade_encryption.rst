Upgrade Encryption
##################

.. contents::
    :local:
    :depth: 2

Documentations
**************

- `CodeIgniter 3.X Encryption 문서 <http://codeigniter.com/userguide3/libraries/encryption.html>`_
- :doc:`CodeIgniter 4.X Encryption 문서 </libraries/encryption>`

변경된 사항
*************
- PHP 7.2에서 더 이상 사용되지 않는 ``MCrypt``\ 에 대한 지원이 중단되었습니다.

Upgrade Guide
**************
1. **application/config/config.php**\ 의 ``$config['encryption_key'] = 'abc123';``\ 는  **app/Config/Encryption.php**\ 의 ``public $key = 'abc123';``\ 로  이동하였습니다.
2. CI3의 암호화로 암호화된 데이터를 해독해야 하는 경우, 호환성을 유지하기 위해 설정을 구성하세요. :ref:`encryption-compatible-with-ci3`\ 를 참조하세요.
3. 암호화 라이브러리를 사용한 곳은 ``$this->load->library('encryption');``\ 를 ``$encrypter = service('encrypter');``\ 로 바꾸고 암호화 및 암호 해독 방법을 다음 예제와 같이 변경합니다.

Code Example
*************

CodeIgniter Version 3.x
=======================

.. literalinclude:: upgrade_encryption/ci3sample/001.php

CodeIgniter Version 4.x
=======================

.. literalinclude:: upgrade_encryption/001.php
