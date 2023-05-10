####################
요청(Request) Class
####################

요청 클래스는 HTTP 요청의 객체 지향적 표현입니다. 이는 브라우저에서 애플리케이션으로의 요청과 같은 들어오는 요청과 애플리케이션에서 제3자 애플리케이션으로 요청을 보내는 데 사용되는 나가는 요청 모두에 대해 작동하도록 설계되었습니다.

이 클래스는 두 경우 모두가 필요로하는 공통 기능을 제공하지만, 각각의 경우 특정 기능을 추가하기 위해 Request 클래스에서 확장된 사용자 정의 클래스가 있습니다. 실제로 이러한 클래스를 사용해야합니다.

더 많은 사용 방법에 대해서는 :doc:`IncomingRequest Class <./incomingrequest>`\ 와 :doc:`CURLRequest Class <../libraries/curlrequest>` 문서를 참조하십시오.

***************
Class Reference
***************

.. php:namespace:: CodeIgniter\HTTP

.. php:class:: Request

    .. php:method:: getIPAddress()

        :returns: 사용자의 IP 주소, IP 주소가 유효하지 않은 경우 ``0.0.0.0``\ 을 반환
        :rtype: string

        현재 사용자의 IP 주소를 반환합니다. IP 주소가 유효하지 않은 경우 메소드는 ``0.0.0.0``\ 을 리턴합니다.
        
        .. literalinclude:: request/001.php

        .. important:: 이 메소드는 ``Config\App::$proxyIPs`` 설정을 고려하여 허용된 IP 주소에 대한 HTTP 헤더로 보고된 클라이언트 IP 주소를 반환합니다.

    .. php:method:: isValidIP($ip[, $which = ''])

        .. deprecated:: 4.0.5
           대신 :doc:`../libraries/validation`\ 을 사용하십시오.

        .. important:: 이 메소드는 더 이상 사용되지 않습니다. 향후 릴리스에서 제거됩니다.

        :param    string    $ip: IP address
        :param    string    $which: IP protocol (``ipv4`` or ``ipv6``)
        :returns:  주소가 유효하면 true, 그렇지 않으면 false
        :rtype:    bool

        IP 주소를 입력으로 사용하고 유효한지 여부에 따라 true 또는 false(부울)를 리턴합니다.

        .. note:: 위의 $request->getIPAddress() 메소드는 IP 주소를 자동으로 검증합니다.

        .. literalinclude:: request/002.php

        IP 형식을 지정하기 위해 ``ipv4`` 또는 ``ipv6``\ 의 선택적 두 번째 매개 변수를 사용합니다. 지정하지 않으면 두 형식을 모두 확인합니다.

    .. php:method:: getMethod([$upper = false])

        .. important:: 매개변수 ``$upper``\ 는 더 이상 사용되지 않습니다. 향후 릴리스에서 제거됩니다.

        :param    bool    $upper: 요청 메소드 이름을 대문자 또는 소문자로 반환할지 여부
        :returns: HTTP 요청 방법(request method)
        :rtype:   string

        설정 옵션에 따라 대문자 또는 소문자로 ``$_SERVER['REQUEST_METHOD']``\ 를 반환합니다.

        .. literalinclude:: request/003.php

    .. php:method:: setMethod($method)

        .. deprecated:: 4.0.5
           대신 :php:meth:`CodeIgniter\\HTTP\\Request::withMethod()` 사용하십시오.

        :param string $method: 요청 방법을 설정합니다. 요청을 위조할 때 사용됩니다.
        :returns: This request
        :rtype: Request

    .. php:method:: withMethod($method)

        .. versionadded:: 4.0.5

        :param string $method: 요청 방법(method)을 설정합니다..
        :returns: New request instance
        :rtype: Request

    .. php:method:: getServer([$index = null[, $filter = null[, $flags = null]]]) 
        :noindex:

        :param  mixed    $index: 값 이름
        :param  int      $filter: 적용할 필터 유형, 필터 목록은 `PHP 메뉴얼 <https://www.php.net/manual/en/filter.filters.php>`__\ 에서 찾을 수 있습니다.
        :param  int      $flags: 적용 할 플래그, 플래그 목록은 `PHP 메뉴얼 <https://www.php.net/manual/en/filter.filters.flags.php>`__\ 에서 찾을 수 있습니다.
        :returns:  발견되면 ``$_SERVER`` 항목 값, 그렇지 않으면 null
        :rtype:    mixed

        이 메소드는 :doc:`IncomingRequest Class <./incomingrequest>`\ 의 ``getPost()``, ``getGet()``, ``getCookie()`` 메소드와 동일하며 server 데이터(``$_SERVER``)만 가져옵니다.
        
        .. literalinclude:: request/004.php

        ``$_SERVER``\ 의 여러개의 값을 배열을 받고싶다면, 필요한 모든 키를 배열로 전달하십시오.
        
        .. literalinclude:: request/005.php

    .. php:method:: getEnv([$index = null[, $filter = null[, $flags = null]]])

        :param    mixed     $index: 값 이름
        :param    int       $filter: 적용할 필터 유형. 필터 목록 `PHP 메뉴얼 <https://www.php.net/manual/en/filter.filters.php>`__.
        :param    int|array $flags: 적용할 플래그. 플래그 목록 `PHP 메뉴얼 <https://www.php.net/manual/en/filter.filters.flags.php>`__.
        :returns: 발견되면 ``$_ENV`` 항목 값, 그렇지 않으면 null
        :rtype:   mixed

        이 메소드는 :doc:`IncomingRequest Class <./incomingrequest>`\ 의 ``getPost()``, ``getGet()``, ``getCookie()`` 메소드와 동일하며 env 데이터(``$_ENV``)만 가져옵니다.
        
        .. literalinclude:: request/006.php

        ``$_ENV``\ 의 여러개의 값을 배열을 받고싶다면, 필요한 모든 키를 배열로 전달하십시오.
        
        .. literalinclude:: request/007.php

    .. php:method:: setGlobal($method, $value)

        :param    string $method: Method명
        :param    mixed  $value:  추가할 데이터
        :returns: This request
        :rtype:   Request

        ``$_GET``, ``$_POST`` 등과 같은 PHP 글로벌 값을 수동으로 설정할 수 있습니다.

    .. php:method:: fetchGlobal($method [, $index = null[, $filter = null[, $flags = null]]])

        :param    string    $method: 입력 필터 상수
        :param    mixed     $index: 값 이름
        :param    int       $filter: 적용할 필터 유형. 필터 목록 `PHP 메뉴얼 <https://www.php.net/manual/en/filter.filters.php>`__.
        :param    int|array $flags: 적용할 플래그. 플래그 목록 `PHP 메뉴얼 <https://www.php.net/manual/en/filter.filters.flags.php>`__.
        :rtype:   mixed

        cookie, get, post 등과 같은 PHP 글로벌에서 하나 이상의 항목을 가져옵니다.
        선택적으로 필터를 전달하여 입력을 검색할 때 입력을 필터링할 수 있습니다.