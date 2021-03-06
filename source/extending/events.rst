이벤트
#####################################

CodeIgniter의 이벤트 기능은 핵심 파일을 해킹하지 않고 프레임 워크의 내부 작업을 활용하고 수정할 수있는 방법을 제공합니다. 
CodeIgniter가 실행될 때 특정 실행 프로세스를 따릅니다.
그러나 실행 프로세스의 특정 단계에서 일부 작업을 수행하려는 경우가 있을 수 있습니다.
예를 들어 컨트롤러가 로드되기 직전 또는 직후에 스크립트를 실행하거나 다른 위치에서 자체 스크립트 중 하나를 트리거할 수 있습니다.
이벤트는 *publish/subscribe* 패턴에서 작동하며, 여기서 스크립트 실행중 이벤트가 트리거됩니다.
다른 스크립트는 Events 클래스에 등록하여 해당 이벤트가 트리거될 때 조치를 수행할 것임을 알리는 방식으로 해당 이벤트를 "구독"할 수 있습니다.

이벤트 활성화
=================

이벤트는 항상 활성화되어 있으며 전역적으로 사용할 수 있습니다.

이벤트 정의
=================

대부분의 이벤트는 **app/Config/Events.php** 파일에 정의되어 있습니다.
Events 클래스의 ``on()`` 메소드로 이벤트에 액션을 구독할 수 있습니다.
첫 번째 매개 변수는 구독할 이벤트의 이름입니다. 
두 번째 매개 변수는 해당 이벤트가 트리거될 때 실행될 호출 가능(callable)입니다.

::

	use CodeIgniter\Events\Events;

	Events::on('pre_system', ['MyClass', 'MyFunction']);

이 예에서 **pre_controller** 이벤트가 실행될 때마다 ``MyClass`` 인스턴스가 생성되고 ``MyFunction`` 메소드가 실행됩니다.
두 번째 매개 변수는 PHP가 인식하는 `호출 가능(callable) <https://www.php.net/manual/en/function.is-callable.php>`_ 의 *모든* 형식일 수 있습니다.

::

	// Call a standalone function
	Events::on('pre_system', 'some_function');

	// Call on an instance method
	$user = new User();
	Events::on('pre_system', [$user, 'some_method']);

	// Call on a static method
	Events::on('pre_system', 'SomeClass::someMethod');

	// Use a Closure
	Events::on('pre_system', function(...$params)
	{
		. . .
	});

우선 순위 설정
------------------

단일 메소드에 여러 메소드를 등록할 수 있으므로 해당 메소드가 호출되는 순서를 정의하는 방법이 필요합니다.
우선 순위 값은 ``on()`` 메소드의 세 번째 매개 변수로 전달하면 됩니다. 
우선 순위는 낮은 값이 먼저 실행되며 값이 1이면 우선 순위가 가장 높습니다.

::

    Events::on('post_controller_constructor', 'some_function', 25);

우선 순위가 동일한 구독자는 정의된 순서대로 실행됩니다.

값에 유용한 범위를 설정하는 세 개의 상수가 정의되어 있습니다.
이것을 꼭 사용할 필요는 없지만 가독성을 도울 수 있습니다.

::

	define('EVENT_PRIORITY_LOW', 200);
	define('EVENT_PRIORITY_NORMAL', 100);
	define('EVENT_PRIORITY_HIGH', 10);

정렬되면 모든 구독자가 순서대로 실행됩니다.
구독자가 부울(bool) false 값을 반환하면 구독자의 실행이 중지됩니다.


나만의 이벤트 게시
==========================

이벤트 라이브러리를 사용하면 자신의 코드로 이벤트를 간단하게 만들 수 있습니다. 
이 기능을 사용하려면 **Events** 클래스에서 이벤트 이름으로 ``trigger()`` 메소드를 호출하면됩니다.

::

	\CodeIgniter\Events\Events::trigger('some_event');

게시할 때 매개 변수를 추가하여 임의의 인수를 구독자에게 전달할 수 있습니다. 
구독자에게 정의된 것과 동일한 순서로 인수가 지정됩니다

::

	\CodeIgniter\Events\Events::trigger('some_events', $foo, $bar, $baz);

	Events::on('some_event', function($foo, $bar, $baz) {
		...
	});

이벤트 시뮬레이션
====================

테스트할 때 수백 개의 전자 메일을 보내는 것과 같은 느리고 비생산적인 작업을 이벤트로 발생하지 않도록 할 수 있습니다.
``simulate()`` 메소드를 사용하면 Events 클래스에 이벤트 실행만 시뮬레이션하도록 지시할 수 있습니다.
**true**\ 인 경우 트리거 메소드는 모든 이벤트를 건너 뜁니다. 
그러나 다른 모든 것은 정상적으로 작동합니다.

::

    Events::simulate(true);

false를 전달하여 시뮬레이션을 중지할 수 있습니다

::

    Events::simulate(false);

이벤트 포인트
=================

다음은 CodeIgniter 핵심(core) 코드에서 사용 가능한 이벤트 포인트 목록입니다.

* **pre_system** 시스템 실행중에 매우 일찍 호출됩니다. 벤치 마크 및 이벤트 클래스만 로드되고, 라우팅이나 다른 프로세스가 발생하지 않았습니다.
* **post_controller_constructor** 컨트롤러가 인스턴스화 된 직후, 메소드 호출이 발생하기 전에 호출됩니다.
* **post_system** 최종 렌더링된 페이지가 브라우저로 전송된 후, 최종 데이터가 브라우저로 전송된 후 시스템 실행이 끝날 때 호출됩니다.
* **email** ``CodeIgniter\Email\Email``\ 에서 보낸 이메일이 성공적으로 전송된 후 호출됩니다. 매개 변수로 ``Email`` 클래스의 속성 배열을 받습니다.
* **DBQuery** 데이터베이스 쿼리가 성공적으로 완료된 후 호출됩니다. ``Query`` 개체(object)를 받습니다.
* **migrate** ``latest()`` 또는 ``regress()``\ 로 성공적으로 마이그레이션된 후 호출됩니다. ``MigrationRunner``\ 의 현재 속성받습니다.