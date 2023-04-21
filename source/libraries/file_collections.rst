################
파일 Collections
################

파일 그룹을 사용하는 작업은 번거로울 수 있으므로 파일 시스템 전체에서 파일 그룹을 쉽게 찾고 작업할 수 있는 ``FileCollection`` 클래스를 제공합니다.

.. contents::
    :local:
    :depth: 2

***********
Basic Usage
***********

``FileCollection``\ 은 가장 기본적으로 설정하거나 빌드하는 파일의 색인입니다.

.. literalinclude:: files/011.php

파일을 입력한 후, 일부 파일을 제거하거나 일치하는 특정 regex 또는 glob-style 패턴을 가진 파일을 제거하거나 유지하는 필터링 명령을 사용할 수 있습니다.

.. literalinclude:: files/012.php

컬렉션이 완성되면 ``get()``\ 을 사용하여 각 ``File``\ 과의 직접 작업을 위한 countable 과 iterable 이라는 장점을 지닌 ``FileCollection``\ 을 활용할 수 있습니다.

.. literalinclude:: files/013.php

다음은 ``FileCollection``\ 으로 작업하는데 사용하는 구체적인 메소드입니다.

*********************
컬렉션으로 시작하기
*********************

__construct(string[] $files = [])
=================================

생성자에 collection으로 사용할 파일 경로를 옵션 배열로 지정 할 수 있습니다. 
지정된 파일은 ``add()`` 메소드에 전달되며 ``$files``\ 의 자식 클래스에서 제공한 파일은 그대로 유지됩니다.

define()
========

자식 클래스가 자신의 초기 파일을 정의할 수 있습니다. 
이 메서드는 생성자에 의해 호출되며 메소드를 호출하지 않아도 미리 정의된 컬렉션을 사용할 수 있습니다.

.. literalinclude:: files/014.php

이제 ``ConfigCollection``\ 을 사용하여 매번 collection 메소드를 다시 호출할 필요 없이 앱의 모든 구성(config) 파일에 액세스할 수 있습니다.

set(array $files)
=================

입력 파일 목록을 제공된 파일 경로 문자열 배열로 설정합니다.
빈 배열을 전달하면 컬렉션에서 기존 파일이 모두 제거되므로 ``$collection->set([])`` 은 기본적으로 하드 리셋입니다.

***************
Inputting Files
***************

add(string[]|string $paths, bool $recursive = true)
===================================================

경로 또는 경로 배열로 표시된 모든 파일을 추가합니다. 
경로가 디렉터리로 확인되면 ``$recursive``\ 에 하위 디렉터리가 포함됩니다.

addFile(string $file) / addFiles(array $files)
==============================================

현재 입력 파일 목록에 파일을 추가합니다. 파일은 실제 파일의 절대 경로입니다.

removeFile(string $file) / removeFiles(array $files)
====================================================

파일 또는 파일을 현재 입력 파일 목록에서 제거합니다.

addDirectory(string $directory, bool $recursive = false)
========================================================
addDirectories(array $directories, bool $recursive = false)
===========================================================

디렉토리 또는 디렉토리에서 모든 파일을 추가하고 선택적으로 하위 디렉토리로 이동합니다.
디렉토리는 실제 디렉토리의 절대 경로입니다.

***************
파일 필터링
***************

removePattern(string $pattern, string $scope = null)
====================================================
retainPattern(string $pattern, string $scope = null)
====================================================

현재 파일 목록을 패턴(과 선택적 범위)을 통해 필터링하여 일치하는 파일을 제거하거나 유지합니다. 
``$pattern``\ 은 완전한 정규식(예: ``'#[A-Za-z]+\.php#'``)이거나 ``glob()``\ 과 유사한 의사-정규식(예: ``*.css``)일 수 있습니다.
``$scope``\ 가 제공되면 해당 디렉토리 내부 또는 하위의 파일만 고려됩니다.(``$scope`` 외부의 파일은 항상 유지됩니다).
범위가 제공되지 않으면 모든 파일이 대상이 됩니다.

.. literalinclude:: files/015.php

****************
파일 검색
****************

get(): string[]
===============

로드된 모든 입력 파일의 배열을 반환합니다.

.. note:: ``FileCollection``\ 은 ``IteratorAggregate`` 이므로 직접 사용할 수 있습니다 (예: ``foreach ($collection as $file)``).
