=====================
허니팟 클래스
=====================

**app\Config\Filters.php** 파일의 허니팟 클래스를 활성화하면 봇(Bot)이 CodeIgniter4 어플리케이션에 요청하는 시점을 결정할 수 있습니다.
이것은 모든 폼(form)에 허니팟 폼 필드를 첨부하여 수행되며, 사람에게 숨겨져 있는 이 필드를 봇이 액세스하여 데이터를 입력한 요청에 대해 봇(Bot)에서 온 것으로 가정하고 ``HoneypotException``\ 을 던집니다.

.. contents::
    :local:
    :depth: 2

***************
허니팟 활성화
***************

허니팟을 활성화하려면 **app/Config/Filters.php**\ 를 변경해야 합니다. 다음과 같이 ``$globals`` 배열의 허니팟의 주석 처리를 제거하십시오.

.. literalinclude:: honeypot/001.php

샘플 Honeypot 필터는 번들로 제공되며, **system/Filters/Honeypot.php**\ 입니다.
직접 만든 필터를 사용하려면 **app/Filters/Honeypot.php**\ 에 만들고 구성(config)의 ``$aliases``\ 를 적절하게 수정하십시오.

********************
허니팟 커스터마이징
********************

허니팟은 직접 만들수 있습니다. 
아래 필드는 **app/Config/Honeypot.php** 또는 ``.env``\ 에서 설정할 수 있습니다.

* ``$hidden`` - 허니팟 필드의 가시성을 제어하기 위해 ``true`` 또는 ``false``\ 를 사용합니다. 기본값은 ``true``\ 입니다.
* ``$label`` - 허니팟 필드에 대한 HTML 레이블입니다. 기본값은 ``'Fill This Field'``\ 입니다.
* ``$name`` - 템플릿에 사용되는 HTML 폼 필드의 이름입니다. 기본값은 ``'honeypot'``\ 입니다.
* ``$template`` - 허니팟에 사용되는 폼 필드 템플릿입니다. 기본값은 ``'<label>{label}</label><input type="text" name="{name}" value="">'``\ 입니다.
* ``$container`` - 템플릿을 위한 컨테이너 태그입니다. 기본값은 ``'<div style="display:none">{template}</div>'``\ 입니다.
  CSP를 사용하도록 설정한 경우 ``style="display:none``\ "을 제거할 수 있습니다.
* ``$containerId`` - [v4.3.0부터] 이 설정은 CSP를 사용하도록 설정한 경우에만 사용됩니다. 컨테이너 태그의 id 속성을 변경할 수 있습니다. 기본값은 ``'hpc'``\ 입니다.
