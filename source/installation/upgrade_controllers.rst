컨트롤러 업그레이드
###################

.. contents::
    :local:
    :depth: 2

관련 문서
==============

- `CodeIgniter 3.X 컨트롤러 설명서 <http://codeigniter.com/userguide3/general/controllers.html>`_
- `CodeIgniter 4.X 컨트롤러 설명서 </incoming/controllers.html>`_

무엇이 바뀌었습니까?
=====================

- CodeIgniter4에 네임스페이스가 추가되었으므로 컨트롤러를 지원 네임스페이스로 변경해야 합니다.
- CI4 Controller의 생성자는 코어 클래스를 속성에 자동으로 로드하지 않습니다.
- CI4의 Controller에는 특별한 생성자인 :ref:`initController() <controller-constructor>`\ 가 있습니다.
- CI4는 작업에 사용할 :doc:`Request </incoming/incomingrequest>`\ 와 :doc:`Responses </outgoing/response>` 객체를 제공합니다 - CI3 방식보다 더 강력합니다.
- 기본 컨트롤러를 원하는 경우 (CI3의 ``MY_Controller``\ 와 같은), **app/Controllers/BaseController.php**\ 를 사용하세요."


업그레이드 가이드
=================

1. 먼저 모든 컨트롤러 파일을 **app/Controllers** 폴더로 이동합니다.
2. <?php 태그 바로 뒤에 ``namespace App\Controllers;``\ 를 추가합니다.
3. ``extends CI_Controller``\ 를 ``extends BaseController``\ 로 바꿉니다.
4. ``defined('BASEPATH') OR exit('No direct script access allowed');`` 라인이 있으면 제거합니다.

| 컨트롤러 구조에 하위 디렉터리를 사용한다면 네임스페이스를 변경해야 합니다.
| 예를 들어 **application/controllers/users/auth/Register.php**\ 에 버전 3의 컨트롤러가 있다면 네임스페이스는 ``namespace App\Controllers\Users\Auth;``\ 이고 버전 4의 컨트롤러 경로는 **app/Controllers/Users/Auth/Register.php**\ 와 같이 변경됩니다. 하위 디렉토리의 첫 글자가 대문자로 되어 있는지 확인하십시오.
| ``BaseController``\ 를 확장하려면 네임스페이스 정의 아래에 ``use``\ 문을 삽입해야 합니다 : ``use App\Controllers\BaseController;``

코드 예
============

CodeIgniter Version 3.x
------------------------

Path: **application/controllers**

.. literalinclude:: upgrade_controllers/ci3sample/001.php

CodeIgniter Version 4.x
-----------------------

Path: **app/Controllers**

.. literalinclude:: upgrade_controllers/001.php
