CodeIgniter 리포지토리
########################

.. contents::
    :local:
    :depth: 2

codeigniter4 organization
=========================

CodeIgniter 4 오픈 소스 프로젝트는 `GitHub organization <https://github.com/codeigniter4>`_\ 에 있습니다.

잠재적 기여자를 위한 몇 가지 개발 리포지토리:

+------------------+--------------+-----------------------------------------------------------------+
| Repository       | Audience     | Description                                                     |
+==================+==============+=================================================================+
| CodeIgniter4_    | contributors | Project codebase, including tests & user guide sources          |
+------------------+--------------+-----------------------------------------------------------------+
| translations_    | developers   | System message translations                                     |
+------------------+--------------+-----------------------------------------------------------------+
| coding-standard_ | contributors | Coding style conventions & rules                                |
+------------------+--------------+-----------------------------------------------------------------+
| devkit_          | developers   | Development toolkit for CodeIgniter libraries and projects      |
+------------------+--------------+-----------------------------------------------------------------+
| settings_        | developers   | Settings Library for CodeIgniter 4                              |
+------------------+--------------+-----------------------------------------------------------------+
| shield_          | developers   | Authentication and Authorization Library for CodeIgniter 4      |
+------------------+--------------+-----------------------------------------------------------------+
| tasks_           | developers   | Task Scheduler for CodeIgniter 4                                |
+------------------+--------------+-----------------------------------------------------------------+
| cache_           | developers   | PSR-6 and PSR-16 Cache Adapters for CodeIgniter 4               |
+------------------+--------------+-----------------------------------------------------------------+

.. _CodeIgniter4: https://github.com/codeigniter4/CodeIgniter4
.. _translations: https://github.com/codeigniter4/translations
.. _coding-standard: https://github.com/CodeIgniter/coding-standard
.. _devkit: https://github.com/codeigniter4/devkit
.. _settings: https://github.com/codeigniter4/settings
.. _shield: https://codeigniter4.github.io/shield
.. _tasks: https://github.com/codeigniter4/tasks
.. _cache: https://github.com/codeigniter4/cache

설치 지침에서 참조되는 몇 가지 배포 리포지토리가 있습니다.
배포 리포지토리는 새 버전이 릴리스될 때 자동으로 빌드되며 직접 제공되지 않습니다.

+------------------+--------------+-----------------------------------------------------------------+
| Repository       | Audience     | Description                                                     |
+==================+==============+=================================================================+
| framework_       | developers   | Released versions of the framework                              |
+------------------+--------------+-----------------------------------------------------------------+
| appstarter_      | developers   | Starter project (app/public/writable).                          |
|                  |              | Dependent on "framework"                                        |
+------------------+--------------+-----------------------------------------------------------------+
| userguide_       | anyone       | Pre-built user guide                                            |
+------------------+--------------+-----------------------------------------------------------------+

.. _framework: https://github.com/codeigniter4/framework
.. _appstarter: https://github.com/codeigniter4/appstarter
.. _userguide: https://github.com/codeigniter4/userguide

위의 모든 항목은 GitHub 저장소 페이지의 "Code" 탭에 있는 보조 탐색 모음에서 "releases" 링크를 선택하여 저장소의 최신 버전을 다운로드할 수 있습니다. 
리포지토리 홈페이지인 경우 오른쪽의 "Clone or download" 드롭다운 버튼을 선택하여 현재(개발 중) 버전를 각각 복제하거나 다운로드할 수 있습니다.

Composer Packages
=================

또한 `packagist.org <https://packagist.org/search/?query=codeigniter4>`_\ 에 composer 설치 가능 패키지를 유지 관리합니다.
이는 위에서 언급한 리포지토리에 해당합니다.

- `codeigniter4/framework <https://packagist.org/packages/codeigniter4/framework>`_
- `codeigniter4/appstarter <https://packagist.org/packages/codeigniter4/appstarter>`_
- `codeigniter4/translations <https://packagist.org/packages/codeigniter4/translations>`_
- `codeigniter/coding-standard  <https://packagist.org/packages/codeigniter/coding-standard>`_
- `codeigniter4/devkit <https://packagist.org/packages/codeigniter4/devkit>`_
- `codeigniter4/settings <https://packagist.org/packages/codeigniter4/settings>`_
- `codeigniter4/shield <https://packagist.org/packages/codeigniter4/shield>`_
- `codeigniter4/cache <https://packagist.org/packages/codeigniter4/cache>`_

자세한 내용은 :doc:`Installation </installation/index>` 페이지를 참조하십시오.

CodeIgniter 4 Projects
======================

우리는 프레임워크의 일부는 아니지만 이를 보여 주거나 더 쉽게 사용할 수있는 프로젝트로 GitHub에 `codeigniter4projects <https://github.com/codeigniter4projects>`_\ 을 운영하고 있습니다!

+------------------+--------------+-----------------------------------------------------------------+
| Repository       | Audience     | Description                                                     |
+==================+==============+=================================================================+
| website_         | developers   | The codeigniter.com website, written in CodeIgniter 4           |
+------------------+--------------+-----------------------------------------------------------------+
| playground_      | developers   | Basic code examples in project form. Still growing.             |
+------------------+--------------+-----------------------------------------------------------------+

.. _website: https://github.com/codeigniter4projects/website
.. _playground: https://github.com/codeigniter4projects/playground

이들은 composer 설치 가능 리포지토리가 아닙니다.
