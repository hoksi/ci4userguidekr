뷰 업그레이드
#############

.. contents::
    :local:
    :depth: 2

관련 문서
==============

- `CodeIgniter 3.X 뷰 설명서 <http://codeigniter.com/userguide3/general/views.html>`_
- :doc:`CodeIgniter 4.X 뷰 설명서 </outgoing/views>`

무엇이 바뀌었습니까?
=====================

- CI4의 뷰는 이전과 매우 유사하지만 다르게 해석됩니다. CI3의 ``$this->load->view('x');`` 대신 ``return view('x');``\ 를 사용합니다.
- CI4는 응답을 여러 조각으로 구성하는 :doc:`../outgoing/view_cells`\ 과 페이지 레이아웃을 위한 :doc:`../outgoing/view_layouts`\ 을 지원합니다.
- :doc:`Template Parser <../outgoing/view_parser>`\ 는 그대로 있으며, 상당히 개선되었습니다.

업그레이드 가이드
=================

1. 먼저 모든 뷰를 **app/Views** 폴더로 옮깁니다.
2. 뷰를 로드하는 모든 스크립트의 뷰 로드 구문을 다음과 같이 변경합니다. 
    - ``$this->load->view('directory_name/file_name')`` => ``return view('directory_name/file_name');``
    - ``$content = $this->load->view('file', $data, TRUE);`` => ``$content = view('file', $data);``
3. (선택사항) 뷰의 echo 구문 ``<?php echo $title; ?>``\ 을 ``<?= $title ?>``\ 로 변경할 수 있습니다.
4. 만약 ``defined('BASEPATH') OR exit('No direct script access allowed');`` 라인이 존재한다면 제거합니다.

Code Example
============

CodeIgniter Version 3.ㅌ
------------------------

Path: **application/views**

.. literalinclude:: upgrade_views/ci3sample/001.php

CodeIgniter Version 4.x
-----------------------

Path: **app/Views**

.. literalinclude:: upgrade_views/001.php
