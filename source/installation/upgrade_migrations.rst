마이그레이션 업그레이드
#######################

.. contents::
    :local:
    :depth: 1

관련 문서
==============

- `Codeigniter 3.X 데이터베이스 마이그레이션 설명서 <http://codeigniter.com/userguide3/libraries/migration.html?highlight=migration>`_
- `Codeigniter 4.X 데이터베이스 마이그레이션 설명서 </dbmgmt/migration.html?highlight=migration>`_

무엇이 바뀌었습니까?
=====================

- 우선 순차적 마이그레이션 이름은 (``001_create_users``, ``002_create_posts``)\  더 이상 지원되지 않습니다. CodeIgniter 버전4는 타임스탬프 구성만 (``20121031100537_create_users``, ``20121031500638_create_posts``) 지원합니다. 순차적 이름을 사용한 경우에는 각 마이그레이션 파일의 이름을 변경해야 합니다.
- 마이그레이션 절차도 변경되었습니다. 이제 간단한 CLI 명령을 사용하여 데이터베이스를 마이그레이션할 수 있습니다.

::

    > php spark migrate

업그레이드 가이드
=================

1. v3 프로젝트에서 순차적 마이그레이션 이름을 사용한다면 타임스탬프로 마이그렝션 이름을 변경해야 합니다.
2. 모든 마이그레이션 파일을 ``app/Database/Migration`` 폴더로 옮겨야 합니다.
3. ``defined('BASEPATH') OR exit('No direct script access allowed');``\ 를 제거합니다.
4. <?php 태그 바로 아래 줄에 ``namespace App\Database\Migrations;``\ 를 추가합니다.
5. ``namespace App\Database\Migrations;`` 아래에 ``use CodeIgniter\Database\Migration;``\ 을 추가합니다.
6. ``extends CI_Migration``\ 을 ``extends Migration``\ 으로 대체합니다.
7. ``Forge`` 클래스 내의 메서드 이름이 camelCase를 사용하도록 변경되었습니다.

- ``$this->dbforge->add_field`` to ``$this->forge->addField``
- ``$this->dbforge->add_key`` to ``$this->forge->addKey``
- ``$this->dbforge->create_table`` to ``$this->forge->addTable``
- ``$this->dbforge->drop_table`` to ``$this->forge->addTable``

8. (선택사항) 배열 구문을 ``array(...)``\ 에서 ``[...]``\ 로 변경할 수 있습니다.

코드 예
============

Codeigniter Version 3.11
------------------------

Path: ``application/migrations``

::

    <?php

    defined('BASEPATH') OR exit('No direct script access allowed');

    class Migration_Add_blog extends CI_Migration
    {
        public function up()
        {
            $this->dbforge->add_field(array(
                'blog_id' => array(
                    'type' => 'INT',
                    'constraint' => 5,
                    'unsigned' => TRUE,
                    'auto_increment' => TRUE,
                ),
                'blog_title' => array(
                    'type' => 'VARCHAR',
                    'constraint' => '100',
                ),
                'blog_description' => array(
                    'type' => 'TEXT',
                    'null' => TRUE,
                ),
            ));
            $this->dbforge->add_key('blog_id', TRUE);
            $this->dbforge->create_table('blog');
        }

        public function down()
        {
            $this->dbforge->drop_table('blog');
        }
    }

Codeigniter Version 4.x
-----------------------

Path: ``app/Database/Migrations``

::

    <?php

    namespace App\Database\Migrations;

    use CodeIgniter\Database\Migration;

    class AddBlog extends Migration
    {
        public function up()
        {
            $this->forge->addField([
                'blog_id' => [
                        'type'           => 'INT',
                        'constraint'     => 5,
                        'unsigned'       => true,
                        'auto_increment' => true,
                ],
                'blog_title' => [
                        'type'       => 'VARCHAR',
                        'constraint' => '100',
                ],
                'blog_description' => [
                        'type' => 'TEXT',
                        'null' => true,
                ],
            ]);
            $this->forge->addKey('blog_id', true);
            $this->forge->createTable('blog');
        }

        public function down()
        {
            $this->forge->dropTable('blog');
        }
    }
