---
title: 美人時計の画像を取得するPHPスクリプトを書いてみた2014版
date: 2014-05-25 0:00 UTC
tags:
- 美人時計
---

ぐーてんもーげん！前回のブログから早2ヶ月、重い腰を上げてやっとブログを書く気がでたので久しぶりに更新します。

はるか昔、美人時計の画像収集をひたすらやっていた時期があって、なんだか懐かしくなって美人時計をみてみたんです。

そしたらものすんごい数の美人時計があってびっくりしました。
オフィシャル美人時計とローカル美人時計の棲み分けがいまいちわかりませんが、増えるのはとても良いことですね！ぜひとも全国制覇してほしいです。

さて、過去に収集していた身としてはまた収集してみたくなったのでついうっかりスクリプトを書いてしまいました。
<del>昔はリファラー制御やら時間指定の制御やらいろいろ入ってて難しかったのですが</del>

以前はRubyで書いてたのですが、今回は今さら感たっぷりのPHPで書いてみました。

```php
<?php

date_default_timezone_set('UTC');

class BijintWGetter {

    static public function getBijintAll($type) {

        $dirname = date('Ymd') . '_' . $type;

        shell_exec('mkdir ' . $dirname);

        $commandList = array();

        $filename = '';
        for($hour = 0; $hour < 24; $hour++){
            for($minutes = 0; $minutes < 60; $minutes++){
                if($hour < 10){
                    if($minutes < 10){
                        $filename = '0' . $hour . '0' . $minutes . '.jpg';
                    } else {
                        $filename = '0' . $hour . $minutes . '.jpg';
                    }
                } else {
                    if($minutes < 10){
                        $filename = $hour . '0' . $minutes . '.jpg';
                    } else {
                        $filename = $hour . $minutes . '.jpg';
                    }
                }

                $command = 'wget http://www.bijint.com/' . $type . '/tokei_images/' . $filename . ' -P ' . __DIR__ . '/' . $dirname . '/';
                array_push($commandList, $command);

            }
        }

        foreach($commandList as $value) {
            shell_exec($value);
            sleep(1);
        }

    }

}

BijintWGetter::getBijintAll('jp');
```

使い方はTerminalを開いてこのスクリプトを実行するだけです。

```
% php get-bijint.php
```

特に難しいことはしていないのですが、サーバーに負担をかけないように`sleep(1)`をいれて1秒ごとに取得するようにしています。

もちろん運営している美人時計様に迷惑がかからないようにしましょう。
