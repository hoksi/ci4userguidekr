뉴스 아이템 만들기
##################

.. contents::
    :local:
    :depth: 2

CodeIgniter를 사용하여 데이터베이스에서 데이터를 읽는 방법을 알았지만 아직 데이터베이스에 정보를 쓰지는 않았습니다.
이 섹션에서는 이 기능을 포함하기 위해 이전에 작성한 뉴스 컨트롤러 및 모델을 확장합니다.

CSRF 필터 활성화
******************

폼을 만들기 전에 CSRF 보호를 사용하도록 설정합니다.

**app/Config/Filters.php** 파일을 열고 다음과 같이 ``$methods`` 속성을 업데이트합니다.

.. literalinclude:: create_news_items/001.php

모든 **POST** 요청에 대해 CSRF 필터를 사용하도록 구성합니다.
CSRF 보호에 대한 자세한 내용은 :doc:`보안 <../libraries/security>`\ 라이브러리에서 확인할 수 있습니다.

.. Warning:: 일반적으로 :ref:`auto-routing-legacy`\ 가 컨트롤러에 접근할 수 있는 모든 HTTP 메소드를 허용하기 때문에 ``$methods`` 필터를 사용할 경우, :ref:`Auto Routing (Legacy) <use-defined-routes-only>`\ 을 사용하지 않도록 설정해야 합니다.
    활성화되어 있는 상태에서는 예상하지 못한 방법으로 컨트롤러에 액세스하여 필터를 우회할 수 있습니다.

Create a form
*************

View
====

데이터베이스에 데이터를 입력하려면 저장할 정보를 입력 할 수 있는 양식(form)을 작성해야 합니다.
제목과 텍스트 입력을 위해 두 개의 필드가 있는 양식이 필요합니다.
모델에서는 타이틀을 이용하여 슬러그를 만듭니다.
**app/Views/news/create.php**\ 에 새로운 뷰를 만듭니다.

::

    <h2><?= esc($title); ?></h2>

    <?= session()->getFlashdata('error') ?>
    <?= validation_list_errors() ?>

    <form action="/news/create" method="post">
        <?= csrf_field() ?>

        <label for="title">Title</label>
        <input type="input" name="title" value="<?= set_value('title') ?>">
        <br>

        <label for="body">Text</label>
        <textarea name="body" cols="45" rows="4"><?= set_value('body') ?></textarea>
        <br>

        <input type="submit" name="submit" value="Create news item">
    </form>

여기에 낯설게 보이는 것 4가지가 있습니다.

:php:func:`session()` 함수는 세션 객체를 가져오는 데 사용되며, ``session()->getFlashdata('error')``\ 는 CSRF 보호와 관련된 오류를 사용자에게 표시하는 데 사용됩니다.
그러나 기본적으로 CSRF 검증 체크에 실패하면 예외가 throw 됩니다. 따라서 아직 작동하지 않습니다. 자세한 정보는 :ref:`csrf-redirection-on-failure`\ 을 참조하십시오.

:doc:`../helpers/form_helper`\ 에서 제공하는 :php:func:`validation_list_errors()` 함수는 폼 유효성 검사와 관련된 오류를 보고하는 데 사용됩니다.

:php:func:`csrf_field()` 함수는 일부 일반적인 공격에 대한 보호를 위해 CSRF 토큰을 포함하는 숨겨진 입력을 생성합니다.

:doc:`../helpers/form_helper`\ 에서 제공하는 :php:func:`set_value()` 함수는 오류가 발생할 때 이전 입력 데이터를 표시하는 데 사용됩니다.

Controller
==========

**News** 컨트롤러로 돌아갑니다.
여기서 우리는 두 가지 작업, 양식이 제출되었는지와 제출된 데이터가 검증 규칙을 통과했는지 여부를 확인할 겁니다
이를 위해 :ref:`컨트롤러의 검증 메소드 <controller-validatedata>`\ 를 사용합니다.

.. literalinclude:: create_news_items/002.php

위의 코드는 많은 기능을 추가합니다.

먼저, :php:func:`helper()` 함수로 :doc:`Form helper <../helpers/form_helper>`\ 를 로드합니다. 
대부분의 헬퍼 함수는 사용하기 전에 헬퍼를 로드해야 합니다.

다음으로, :doc:`IncomingRequest <../incoming/incomingrequest>` 객체인 ``$this->request``\ 를 사용하여 **POST** 요청을 다루는지 확인합니다.
이 객체는 프레임워크에서 컨트롤러에 설정됩니다. 
:ref:`IncomingRequest::is() <incomingrequest-is>` 메소드는 요청의 유형을 확인합니다.
**create()** 엔드포인트의 라우트가 **GET**\ 과 **POST** 요청을 모두 처리하기 때문에 요청이 POST가 아니면 GET 유형이라고 가정할 수 있습니다.
그러면 폼을 로드하고 표시합니다.

그런 다음, 사용자가 입력한 POST 데이터에서 필요한 항목을 가져와 ``$post`` 변수에 설정합니다.
이때도 :doc:`IncomingRequest <../incoming/incomingrequest>` 객체 ``$this->request``를 사용합니다.

그 다음 Controller에서 제공하는 :ref:`validateData() <controller-validatedata>` 헬퍼 함수를 사용하여 ``$post`` 데이터를 유효성 검사합니다.
이 경우, 제목과 내용 필드가 필수이며 특정 길이여야 합니다.
CodeIgniter에는 위에서 설명한 강력한 유효성 검사 라이브러리가 있습니다.
자세한 내용은 :doc:`Validation library <../libraries/validation>`\ 를 읽어보세요.

유효성 검사가 실패하면 폼을 로드하고 표시합니다.

모든 규칙을 통과하면 **NewsModel**\ 이 로드되고 호출됩니다.
이것은 뉴스 항목을 모델에 전달하는 것을 처리합니다.
:ref:`model-save` 메소드는 주요 키와 일치하는 배열 키를 찾아서 자동으로 레코드를 삽입하거나 업데이트합니다.

여기에는 새로운 함수 :php:func:`url_title()`\ 이 포함되어 있습니다.
이 함수는 :doc:`URL helper <../helpers/url_helper>`\ 에서 제공하며 전달한 문자열에서 -를 제거하고 모든 공백은 대시(``-``)로 모든 문자는 소문자로 바꾸는 작업을 수행합니다.
이렇게 하면 URI를 생성하는 데 적합한 멋진 슬러그가 생성됩니다.

이후, 뷰 파일이 로드되어 성공 메시지를 표시하도록 반환됩니다. 
뷰 피일 **app/Views/news/success.php**\ 를 만들고 간단하 성공 메시지를 작성하세요.

::

    <p>News item created successfully.</p>

모델 업데이트
**************

이제 데이터를 저장할 수 있도록 모델을 업데이트하겠습니다.
``save()`` 메소드는 기본 키의 존재 여부에 따라 정보를 삽입할지, 업데이트할지를 결정합니다.
``id`` 필드가 전달되지 않을 경우 **news** 테이블에 새 행이 삽입됩니다.

그러나 모델의 insert 및 update 메소드는 기본적으로 업데이트할 안전한 필드를 모르기 때문에 실제로 데이터를 저장하지 않습니다.
업데이트 가능한 필드 목록을 **NewsModel**\ 의 ``$allowedFields`` 속성에 작성합니다.

.. literalinclude:: create_news_items/003.php

이 새 속성에는 이제 데이터베이스에 저장할 수 있는 필드가 포함됩니다.

.. note:: 
    ``id``\ 는 데이터베이스의 자동 증가(auto-incrementing) 필드이기 때문에 $allowdFields에서 생략되었습니다.
    이렇게하면 대량 할당 취약점으로부터 보호할 수 있습니다.
    모델이 타임 스탬프를 처리하는 경우 해당 타임 스탬프도 제외합니다.

라우팅
*******

CodeIgniter 어플리케이션에 뉴스 항목을 추가하기 전에 **app/Config/Routes.php** 파일에 추가 규칙을 추가해야 합니다.
파일에 다음 규칙이 포함되어 있는지 확인하십시오. 
이를 통해 CodeIgniter는 뉴스 항목의 슬러그 대신 ``create()``\ 를 메소드로 인식합니다.
여러분은 :doc:`../incoming/routing`\ 에서 다른 것에 대한 자세한 내용을 읽을 수 있습니다.

.. literalinclude:: create_news_items/004.php

이제 웹 브라우저의 URL에 ``http://localhost:8080/news/create``\ 를 입력하십시오.
몇 가지 뉴스를 추가하고 페이지를 확인해 보세요.

.. image:: ../images/tutorial3.png
    :align: center
    :height: 415px
    :width: 45%

.. image:: ../images/tutorial4.png
    :align: center
    :height: 415px
    :width: 45%

축하합니다!
***************

당신은 첫 번째 CodeIgniter4 어플리케이션을 방금 완료하셨습니다!

아래에 있는 이미지는 프로젝트의 **app** 폴더를 표시하며, 녹색으로 생성한 모든 파일을 표시합니다.
수정된 두 구성 파일(**Config/Routes.php**, **Config/Filters.php**)은 표시되지 않았습니다.

.. image:: ../images/tutorial9.png
    :align: left