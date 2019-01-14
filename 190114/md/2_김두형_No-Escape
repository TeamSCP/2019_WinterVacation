## Wechall_No_Escape

------

2019.01.14@Plit00

문제를 먼저 보자

<img width="983" alt="wechall no escape 2019-01-14 02-10-40" src="https://user-images.githubusercontent.com/40850499/51097281-36d04500-1806-11e9-8306-1777a43d9bc4.png">

핵심으로 설명하면 투표(vote for bill)을 할때 111개가 목표이지만 110개가 투표되면 초기화가 되는 형식입니다.

```php
<?php
function noesc_db()
{
        static $noescdb = true;
        if ($noescdb === true)
        {
                $noescdb = gdo_db_instance('localhost', NO_ESCAPE_USER, NO_ESCAPE_PW, NO_ESCAPE_DB);
                $noescdb->setLogging(false);
                $noescdb->setEMailOnError(false);
        }
        return $noescdb;
}
function noesc_createTable()
{
        $db = noesc_db();
        $query =
                "CREATE TABLE IF NOT EXISTS noescvotes ( ".
                "id     INT(11) UNSIGNED PRIMARY KEY, ". # I could have one row per candidate, but currently there is only one global row(id:1). I know it`s a bit unrealistic, but at least it is safe, isn`t it?
                "bill   INT(11) UNSIGNED NOT NULL DEFAULT 0, ". # bill column
                "barack INT(11) UNSIGNED NOT NULL DEFAULT 0, ". # barack column
                "george INT(11) UNSIGNED NOT NULL DEFAULT 0 )"; # george columb
        if (false === $db->queryWrite($query)) {
                return false;
        }
        return noesc_resetVotes();
}
function noesc_resetVotes()
{
        noesc_db()->queryWrite("REPLACE INTO noescvotes VALUES (1, 0, 0, 0)");
        echo GWF_HTML::message('No Escape', 'All votes have been reset', false);
}
function noesc_voteup($who)
{
        if ( (stripos($who, 'id') !== false) || (strpos($who, '/') !== false) ) {
                echo GWF_HTML::error('No Escape', 'Please do not mess with the id. It would break the challenge for others', false);
                return;
        }
        $db = noesc_db();
        $who = GDO::escape($who);
        //--------------------------------------------------------//
        $query = "UPDATE noescvotes SET `$who`=`$who`+1 WHERE id=1";
        //이부분 싱글쿼터와 역쿼터가 있으므로써 취약점으로 이용할 수 있다는 사실을 알 수 있다.
        
        if (false !== $db->queryWrite($query)) {
                echo GWF_HTML::message('No Escape', 'Vote counted for '.GWF_HTML::display($who), false);
        }       
        noesc_stop100();
}
function noesc_getVotes()
{
        return noesc_db()->queryFirst("SELECT * FROM noescvotes WHERE id=1");
}
function noesc_stop100()
{
        $votes = noesc_getVotes();
        foreach ($votes as $who => $count)
        {
                if ($count == 111) {
                        noesc_solved();
                        noesc_resetVotes();
                        break;
                }
                
                if ($count >= 100) {
                        noesc_resetVotes();
                        break;
                }
        }
}
function noesc_displayVotes(WC_Challenge $chall)
{
        $votes = noesc_getVotes();
        echo '<table>';
        echo sprintf('<tr><th>%s</th><th>%s</th><th>%s!</th></tr>', $chall->lang('th_name'), $chall->lang('th_count'), $chall->lang('th_vote'));
        $maxwho = '';
        $max = 0;
        $maxcount = 0;
        foreach ($votes as $who => $count)
        {
                if ($who !== 'id') // Skip ID
                {
                        $count = (int) $count;
                        if ($count > $max) {
                                $max = $count;
                                $maxwho = $who;
                                $maxcount = 1;
                        }
                        elseif ($count === $max) {
                                $maxcount++;
                        }
                        $button = GWF_Button::generic($chall->lang('btn_vote', array($who)), "index.php?vote_for=$who");
                        echo sprintf('<tr><td>%s</td><td class="gwf_num">%s</td><td>%s</td></tr>', $who, $count, $button);
                }
        } 
        echo '</table>';      
        if ($maxcount === 1) {
                echo GWF_Box::box($chall->lang('info_best', array(htmlspecialchars($maxwho))));
        }
}
function noesc_solved()
{
        if (false === ($chall = WC_Challenge::getByTitle('No Escape'))) {
                $chall = WC_Challenge::dummyChallenge('No Escape', 2, '/challenge/no_escape/index.php', false);
        } //여기3
        $chall->onChallengeSolved(GWF_Session::getUserID());
}
 
?>
 
```



<img width="799" alt="wechall no escape 2019-01-14 13-47-36" src="https://user-images.githubusercontent.com/40850499/51097411-35534c80-1807-11e9-9d2e-e7753356d463.png">

이 코드는 작은 따옴표는 걸러내지만 쿼리에서 역 따옴표가 사용되고 있기때문에 이것을 통해서 취약점 공격을 할 수 있게 됩니다.



<img width="510" alt="wechall no escape 2019-01-14 02-14-54" src="https://user-images.githubusercontent.com/40850499/51097433-54ea7500-1807-11e9-8f57-508b15fa5dad.png">



```
vote_for={bill'=111}
OR 
bill'=111# 을 이용해서 보내면 통과가 된다.
```

