################
뷰 셀(View Cell)
################

많은 응용 프로그램에서는 페이지 간이나 페이지의 다른 위치에서 반복해서 사용되는 작은 뷰 조각이 있습니다. 
이러한 것들은 보통 도움말 상자, 탐색 컨트롤, 광고, 로그인 양식 등입니다. 
CodeIgniter는 이러한 표시 블록의 로직을 View Cell 내에 캡슐화할 수 있도록 해줍니다. 
이들은 기본적으로 다른 뷰에 포함될 수 있는 미니 뷰입니다. 
셀별 디스플레이 로직을 처리하는 논리를 내장할 수 있습니다. 
각 셀의 로직을 별도의 클래스로 분리하여 뷰를 더 가독성 있게 만들고 유지보수성을 높일 수 있습니다.

CodeIgniter는 두 가지 유형의 View Cell을 지원합니다. 
Simple View Cell과 Controlled View Cell입니다. 
Simple View Cell은 사용자가 선택한 클래스 및 메서드에서 생성할 수 있으며, 반환하는 문자열을 제외하고는 규칙을 따르지 않아도 됩니다. 
Controlled View Cell은 ``Codeigniter\View\Cells\Cell`` 클래스를 확장한 클래스에서 생성해야 하며, 이를 통해 View Cell을 더 유연하고 빠르게 사용할 수 있도록 기능을 제공합니다.

.. contents::
    :local:
    :depth: 2

.. _app-cells:

*******************
View Cell 호출
*******************

어떤 유형의 View Cell을 사용하더라도 ``view_cell()`` 도우미 메소드를 사용하여 어떤 뷰에서든 호출할 수 있습니다. 
첫 번째 매개 변수는 호출할 클래스 및 메서드의 이름이며, 두 번째 매개 변수는 메서드에 전달할 매개 변수 배열입니다. 
메소드는 문자열을 반환해야 하며, ``view_cell()`` 메소드가 호출된 뷰에 삽입됩니다.

::

    <?= view_cell('App\Cells\MyClass::myMethod', ['param1' => 'value1', 'param2' => 'value2']); ?>

클래스의 이름이 전체 네임스페이스를 포함하지 않으면, ``App\Cells`` 네임스페이스에서 찾는 것으로 가정합니다. 
따라서 다음 예제에서는 ``MyClass`` 클래스를 ``app/Cells/MyClass.php``\ 에서 찾으려고 시도합니다. 
그러나 그곳에서 찾을 수 없으면, 모든 네임스페이스가 스캔되며, 각 네임스페이스의 ``Cells`` 하위 디렉토리에서 검색됩니다.

::

    <?= view_cell('MyClass::myMethod', ['param1' => 'value1', 'param2' => 'value2']); ?>

키/값 문자열로 매개 변수를 전달할 수 있습니다.

::

    <?= view_cell('MyClass::myMethod', 'param1=value1, param2=value2'); ?>

************
Simple 셀
************

Simple 셀은 선택한 메소드에서 문자열을 반환하는 클래스입니다. 
간단한 알림 메시지 셀의 예는 다음과 같습니다.

::

    namespace App\Cells;

    class AlertMessage
    {
        public function show($params): string
        {
            return "<div class="alert alert-{$params['type']}">{$params['message']}</div>";
        }
    }

뷰 안에서 다음과 같이 호출할 수 있습니다.

::

    <?= view_cell('AlertMessage::show', ['type' => 'success', 'message' => 'The user has been updated.']); ?>

또한, 매개변수 이름을 메소드 내의 변수명과 일치시켜 가독성을 높일 수도 있습니다.
이 방식을 사용할 경우, 모든 매개변수는 뷰 셀을 호출할 때 항상 지정되어야 합니다.

::

    <?= view_cell('\App\Libraries\Blog::recentPosts', 'category=codeigniter, limit=5') ?>

    public function recentPosts(string $category, int $limit)
    {
        $posts = $this->blogModel->where('category', $category)
                                 ->orderBy('published_on', 'desc')
                                 ->limit($limit)
                                 ->get();

        return view('recentPosts', ['posts' => $posts]);
    }

.. _controlled-cells:

****************
Controlled 셀
****************

Controlled Cell은 두 가지 주요 목표(셀을 가능한 빠르게 빌드하고, 뷰가 필요할 경우 추가적인 로직과 유연성을 제공)가 있습니다.
클래스는 ``CodeIgniter\View\Cells\Cell``\ 을 확장해야 하고 같은 폴더 안에 뷰 파일이 있어야 합니다.
규칙에 따라 클래스 이름은 PascalCase로 뷰 파일은 클래스 이름의 snake_cased로 만들어야 합니다.
클래스 ``MyCell``\ 이 있다면, 뷰 파일은 ``my_cell.php``\ 여야 합니다.

Controlled 셀 생성
==========================

가장 기본적인 수준에서, 클래스 내에서 구현해야 할 것은 public 속성입니다. 
이러한 속성은 자동으로 뷰 파일에서 사용할 수 있게 됩니다. 
위에서 소개한 AlertMessage를 Controlled Cell로 구현하는 방법은 다음과 같습니다.

::

    // app/Cells/AlertMessageCell.php
    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class AlertMessageCell extends Cell
    {
        public $type;
        public $message;
    }

    // app/Cells/alert_message_cell.php
    <div class="alert alert-<?= $type; ?>">
        <?= $message; ?>
    </div>

.. _generating-cell-via-command:

CLI를 통한 셀 생성
===========================

.. versionadded:: 4.3.0

``php spark make:cell`` 명령을 사용하여 Controlled 셀을 CLI를 통해 생성할 수 있습니다.
생성할 셀의 이름을 인수로 받습니다. 
클래스 이름은 PascalCase로 작성해야 하며, 클래스와 뷰 파일은 ``app/Cells`` 디렉토리에 생성됩니다. 

::

    > php spark make:cell AlertMessage

다른 뷰 사용
======================

클래스의 ``view`` 속성을 설정하여 사용자 지정 뷰 이름을 지정할 수 있습니다. 뷰는 일반적인 뷰처럼 위치해야 합니다.

::

    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class AlertMessage extends Cell
    {
        public $type;
        public $message;

        protected $view = 'my/custom/view';
    }

렌더링 사용자 지정
=======================

HTML 렌더링에 대해 더 많은 제어가 필요한 경우 ``render()`` 메소드를 구현할 수 있습니다.
이 메소드를 사용하면 필요한 경우 추가 논리를 수행하고 뷰에 대한 추가 데이터를 전달할 수 있습니다. ``render()`` 메소드는 문자열을 반환해야 합니다.
Controlled 셀(Cell)의 모든 기능을 활용하려면 일반적인 ``view()`` 헬퍼 함수 대신 ``$this->view()``\ 를 사용해야 합니다.

::

    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class AlertMessage extends Cell
    {
        public $type;
        public $message;

        public function render(): string
        {
            return $this->view('my/custom/view', ['extra' => 'data']);
        }
    }

Computed Properties
===================

하나 이상의 속성에 대해 추가 논리를 수행해야 하는 경우 계산된 속성을 사용할 수 있습니다. 
이러한 경우 속성을 ``protected`` 또는 ``private``\ 로 설정하고 해당 속성 이름을 둘러싼 ``get``\ 과 ``Property``\ 로 구성된 public 메소드를 구현해야 합니다.

::

    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class AlertMessage extends Cell
    {
        protected $type;
        protected $message;

        public function getTypeProperty(): string
        {
            return $this->type;
        }

        public function getMessageProperty(): string
        {
            return $this->message;
        }
    }

프리젠테이션 메소드
====================

때로는 뷰를 위해 추가 논리를 수행해야 하지만 매개변수로 전달하고 싶지 않은 경우 뷰 내부에서 호출되는 메소드를 구현할 수 있습니다.

::

    // app/Cells/RecentPostsCell.php
    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class RecentPosts extends Cell
    {
        protected $posts;

        public function linkPost($post)
        {
            return anchor('posts/' . $post->id, $post->title);
        }
    }

    // app/Cells/recent_posts.php
    <ul>
        <?php foreach ($posts as $post): ?>
            <li><?= $this->linkPost($post) ?></li>
        <?php endforeach; ?>
    </ul>

설정 로직 수행
======================

뷰가 렌더링되기 전에 추가적인 논리를 수행해야 하는 경우, ``mount()`` 메소드를 구현할 수 있습니다.
이 메소드는 클래스가 인스턴스화된 직후에 호출되며, 추가적인 속성을 설정하거나 다른 논리를 수행하는 데 사용할 수 있습니다.

::

    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class RecentPosts extends Cell
    {
        protected $posts;

        public function mount()
        {
            $this->posts = model('PostModel')->getRecent();
        }
    }

``mount()`` 메소드에 추가 매개변수를 전달하려면, ``view_cell()`` 헬퍼 함수에 배열 형태로 전달하면 됩니다.
``mount()`` 메소드의 매개변수 이름과 일치하는 매개변수가 전송된 경우, 해당 매개변수가 전달됩니다.

::

    // app/Cells/RecentPosts.php
    namespace App\Cells;

    use CodeIgniter\View\Cells\Cell;

    class RecentPosts extends Cell
    {
        protected $posts;

        public function mount(?int $categoryId)
        {
            $this->posts = model('PostModel')
                ->when($categoryId, function ($query, $category) {
                    return $query->where('category_id', $categoryId);
                })
                ->getRecent();
        }
    }

    // Called in main View:
    <?= view_cell('RecentPosts::show', ['categoryId' => 5]); ?>

************
Cell Caching
************

뷰 셀의 세 번째 매개 변수로 캐싱 시간(초)을 전달하여 뷰 셀 호출 결과를 캐시(cache)할 수 있습니다.
구성(Config)에 정의된 캐시 엔진을 사용합니다.

::

    // Cache the view for 5 minutes
    <?= view_cell('\App\Libraries\Blog::recentPosts', 'limit=5', 300) ?>

뷰 셀의 네 번째 매개 변수로 자동 생성 이름 대신 사용자 지정 이름을 제공할 수 있습니다.

::

    // Cache the view for 5 minutes
    <?= view_cell('\App\Libraries\Blog::recentPosts', 'limit=5', 300, 'newcacheid') ?>
