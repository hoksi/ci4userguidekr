##############################
트랜잭션(Transactiion)
##############################

.. contents::
    :local:
    :depth: 2

CodeIgniter의 데이터베이스 추상화를 통해 트랜잭션 안전 테이블 유형을 지원하는 데이터베이스와 트랜잭션을 사용할 수 있습니다.
MySQL에서는 일반적인 MyISAM 대신 InnoDB 또는 BDB 테이블 유형을 실행해야합니다.
대부분의 다른 데이터베이스 플랫폼은 트랜잭션을 기본적으로 지원합니다.

트랜잭션에 익숙하지 않은 경우 특정 데이터베이스에 대한 온라인 리소스를 찾아 보는 것이 좋습니다.
아래 정보는 당신이 트랜잭션에 대한 기본적인 이해가 있다고 가정합니다.

CodeIgniter의 트랜잭션 접근 방식
======================================

CodeIgniter는 널리 사용되는 데이터베이스 클래스 ADODB에서 사용하는 프로세스와 매우 유사한 트랜잭션 접근 방식을 사용합니다.
이러한 접근 방식은 트랜잭션 실행 프로세스를 크게 단순화하기 때문에 선택했습니다.
대부분의 경우 두 줄의 코드만 필요합니다.

일반적으로 트랜잭션은 쿼리를 추적하고 쿼리의 성공 또는 실패를 기반으로 커밋(commit) 또는 롤백(rollback) 여부를 결정해야 하므로 상당한 양의 작업이 필요합니다.
중첩 쿼리에서는 특히 번거롭습니다.
우리는 당신을 위해 이 모든 것을 자동으로 수행하는 스마트 트랜잭션 시스템을 구현했습니다. (원한다면 수동으로 트랜잭션을 관리할 수 있지만, 실제로는 아무런 이점이 없습니다)

트랜잭션 실행
====================

트랜잭션을 사용하여 쿼리를 실행하려면 ``$this->db->transStart()``\ 과 ``$this->db->transComplete()`` 메소드를 사용하십시오.

.. literalinclude:: transactions/001.php

``transStart()``/``transComplete()`` 메소드 사이에 원하는 쿼리를 실행할 수 있으며 주어진 쿼리의 성공 또는 실패에 따라 모두 커밋되거나 롤백됩니다.

엄격한 모드 (Strict Mode)
==============================

CodeIgniter는 기본적으로 모든 트랜잭션을 엄격 모드로 실행합니다.
엄격 모드가 활성화되면 여러 트랜잭션 그룹을 실행중 한 그룹이 실패하면 모든 후속 그룹이 롤백됩니다.
엄격 모드를 사용하지 않으면 각 그룹이 독립적으로 처리되므로 한 그룹의 장애가 다른 그룹에 영향을 미치지 않습니다.

다음과 같이 엄격한 모드를 비활성화 할 수 있습니다

.. literalinclude:: transactions/002.php

오류 관리
===============

*app/Config/Database.php** 파일의 ``DBDebug`` 값을 true로 설정하면 쿼리 오류가 발생했을때 모든 쿼리가 롤백되고 예외가 발생하며 표준 오류 페이지가 표시됩니다.
``DBDebug``\ 를 false로 설정하면 다음과 같이 자신의 오류를 관리할 수 있습니다.

.. literalinclude:: transactions/003.php

트랜잭션 비활성화
======================

트랜잭션은 기본적으로 활성화되어 있습니다. 트랜잭션을 비활성화하려면 ``$this->db->transOff()``\ 를 사용하면 됩니다.

.. literalinclude:: transactions/004.php

트랜잭션이 비활성화되면 트랜잭션없이 쿼리를 실행할 때와 마찬가지로 쿼리가 자동 커밋됩니다.

테스트 모드
==================

선택적으로 트랜잭션 시스템을 "테스트 모드"\ 로 설정하면 쿼리가 유효한 결과를 생성하더라도 쿼리가 롤백됩니다.
테스트 모드를 사용하려면 ``$this->db->transStart()`` 메소드의 첫 번째 매개 변수를 true로 설정하십시오.

.. literalinclude:: transactions/005.php

수동으로 트랜잭션 실행
=============================

트랜잭션을 수동으로 실행하려면 **app/Config/Database.php** 파일의 ``DBDebug`` 값을 false로 설정하고 다음과 같이 할 수 있습니다.

.. literalinclude:: transactions/006.php

.. note:: 수동 트랜잭션을 실행할 때는 ``$this->db->transStart()``\ 가 아니라 ``$this->db->transBegin()``\ 을 사용해야합니다.

중첩 트랜잭션
===================

CodeIgniter에서 트랜잭션은 최상위 또는 최상위 트랜잭션 명령만 실행되는 방식으로 중첩될 수 있습니다.
내부에 원하는 만큼 트랜잭션 블록을 ``transStart()``/``transComplete()`` 또는 ``transBegin()``/``transCommit()``/``transRollback()`` 쌍으로 포함할 수 있습니다.
CodeIgniter는 트랜잭션 "깊이(depth)"를 추적하고 가장 바깥쪽 레이어(zero depth)에서 조치를 취합니다.

.. literalinclude:: transactions/007.php

.. note:: 구조가 훨씬 더 복잡한 경우 내부 트랜잭션이 데이터베이스의 가장 바깥쪽 계층에서 완전히 실행되도록 하여 의도하지 않은 커밋/롤백(commits/rollback)을 방지하는 것은 사용자의 책임입니다.
