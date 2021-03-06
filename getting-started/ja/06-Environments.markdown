環境
====

`web/`ディレクトリを見てみると、2つのPHPファイル: `index.php`と`frontend_dev.php`が見つかります。
これらのファイルはフロントコントローラーと呼ばれます:
アプリケーションへのすべてのリクエストはこれらを通して行われます。
しかしなぜアプリケーションごとにフロントコントローラーを2つ用意するのでしょうか？

両方のファイルは同じアプリケーションを指し示しますが環境は異なります。
運用サーバーで直接開発する場合を除いて、アプリケーションを開発するとき、複数の**環境**が必要です:

  * **開発環境**: これは**Web開発者**によって使われる環境です。
      アプリケーションに新しい機能を追加したりバグを修正したりします。
  * **テスト環境**: この環境はアプリケーションを自動テストするために使われます。
  * **ステージング環境**: この環境は**顧客**がアプリケーションをテストしてバグもしくは見つからない機能を報告するために使われます。
  * **運用環境**: **エンドユーザー**と情報のやりとりをするための環境です。

環境を使いわける基準は何でしょうか？
たとえば開発環境では、アプリケーションはデバッグ作業を楽にするためにリクエストの詳細をすべてログに記録する必要がありますが、
コードへのすべての変更が即座に反映されるようにするためにキャッシュシステムを無効にしなければなりません。
ですので、開発環境は開発用に最適化しなければなりません。
最良の例は例外が起きるときです。
開発者が問題を速くデバッグするのを手助けするために、symfonyは現在のリクエストに関するすべての情報を持つ例外をブラウザーに表示します:

![開発環境の例外](http://www.symfony-project.org/images/getting-started/1_4/exception_dev.png)

しかし開発環境では、キャッシュレイヤーは有効にしなければならず、もちろんアプリケーションは生の例外の代わりにカスタマイズされたエラーメッセージを表示しなければなりません。
ですので、運用環境ではパフォーマンスとユーザーエクスペリエンスのために最適化しなければなりません。

![運用環境の例外](http://www.symfony-project.org/images/getting-started/1_4/exception_prod.png)

>**TIP**
>フロントコントローラーのファイルを開くと、環境設定以外の内容が同じであることがわかります:
>
>     [php]
>     // web/index.php
>     <?php
>
>     require_once(dirname(__FILE__).'/../config/ProjectConfiguration.class.php');
>
>     $configuration = ProjectConfiguration::getApplicationConfiguration('frontend', 'prod', false);
>     sfContext::createInstance($configuration)->dispatch();

Webデバッグツールバーも環境の使い方のすばらしい例です。
これは開発環境のすべてのページに存在し、異なるタブをクリックすることでたくさんの情報: 現在のアプリケーションの設定、現在のリクエスト用のログ、データベースエンジンで実行されるSQLステートメント、メモリの情報と時間の情報にアクセスできます。
