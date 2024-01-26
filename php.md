
## Тестовое задание PHP-разработчика


1. Представтьте, что это фрагмент кода большого проекта. Напишите, какие есть проблемы у этого кода? 
2. Сделайте рефакторинг данного кода. Можно изменять названия методов, классов, добавлять входящие параметры, создавать новые классы и т.п.. Можно местами использовать псевдокод.

```php
<?php

namespace vBulletin\Search;

class Search {
    public static function doSearch(): void {
      if ($_REQUEST['searchid']){
        $_REQUEST['do'] = 'showresults';
      }elseif(!empty($_REQUEST['q'])){
          $_REQUEST['do'] = 'process';
          $_REQUEST['query'] = &$_REQUEST['q'];
      }
      $db = new \PDO('mysql:dbname=vbforum;host=127.0.0.1', 'forum', '123456');
      if ($_REQUEST['do'] == 'process') {
          $sth = $db->prepare('SELECT * FROM vb_post WHERE text like ?');
          $sth->execute(array($_REQUEST['query']));
          $result = $sth->fetchAll();
          self::render_search_results($result);
          $file = fopen('/var/www/search_log.txt', 'a+');
          fwrite($file, $_REQUEST['query'] . "\n");
      } elseif ($_REQUEST['do'] == 'showresults'){
          $sth = $db->prepare('SELECT * FROM vb_searchresult WHERE searchid = ?');
          $sth->execute(array($_REQUEST['searchid']));
          $result = $sth->fetchAll();
          self::render_search_results($result);
      }
      else {
          echo "<h2>Search in forum</h2><form><input name='q'></form>";
      }
  }

  public static function render_search_results($result){
      global $render;
      foreach($result as $row){
          if ($row['forumid'] != 5){
              $render->render_searh_result($row);
          }
      }
  }
}
```

