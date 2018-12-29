# Data

## 相关表

几张表之间的关系是：三张基础表，其中除了`k_singer_par`之外都不能使用`singerid`字段。此外还有两张连接关系表和`st_k_mixsong_part`的`albumid`与`mixsongid`字段，揭示三张基础表间的连接关系。另外还有一张备注表，利用mixsongid得到每首歌曲的用途，其中type_id=2表示相关概念的作用。

![](picture/相关表.jpg)

## 抽样研究

**说明不能用`st_k_mixsong_part`中的`singerid`，只是部分id而已。**

```hive
#根据mixsongid直接得到歌曲信息，速度快
select * from common.st_k_mixsong_part where mixsongid = '108783870' and dt='2018-11-18';

mixsongid       songname        other_info      albumid albumname       singerid        singername      language        choric_singer   extname gd_sort hash    filesize       timelength      bitrate publish_time    disc    addtime edittime        is_choric       is_single       is_break_rule   is_file_head    is_copyright    ownercount     playcount       updater upload_time     official_songname       is_recommend    editor  is_finish       is_publish      platform_break_rule     composer      lyrics   company songid  mv_hash high_mv_hash    mv_size high_mv_size    mv_type is_mv_file_head music_trac      hash_320        filesize_320    hash_m4a        m4a_size       hash_128        filesize_128    source  hash_192        filesize_192    hash_ape        filesize_ape    bitrate_ape     hash_flac       filesize_flac   bitrate_flac   scid    vip     mvid    is_search       remark  ori_audio_name  suffix_audio_name       version bpm     dt
108783870       浪人琵琶                8631706 浪人琵琶        766987  胡66    国语    胡66、单色凌    mp3     1       CF995E326627B042250C2E1B6C6809D7        358942224000   128     2018-06-21      1       2018-06-20 17:18:04     2018-11-12 11:25:45     0       1       0       100     0       2741106 469039  WB崔京琳        2018-06-20 17:18:04            0               0       1       0       单色凌  单色凌          27696032                        0       0       0       0       0       3F41EF07F1668D107C32A6852A3BE0BD       8973295         0       CF995E326627B042250C2E1B6C6809D7        3589423 32              0               0       0       B04E1C1C1C5233E4EAD3DA2E838489B0       26380958        941     38857248        0       0       0               浪人琵琶                1       72      2018-11-18
```

```
#根据mixsongid得到独立的歌手，速度一般
select * from common.canal_k_album_audio_author where album_audio_id = '108783870';

id      author_id       album_audio_id  add_time        edit_time       editor
46500161        766987  108783870       2018-06-26 18:34:13     2018-06-26 18:34:13     WB王知新
46500162        2569    108783870       2018-06-26 18:34:13     2018-06-26 18:34:13     WB王知新
```

```
#校验得到的歌手信息
select * from common.k_singer_part where singerid = '2569' and dt='2018-11-18';

singerid        singername      othername       pinyin  cindex  sex     sextype areatype        areaname        country intro   short_intro     img330  img     birthday       addtime edittime        old_id  sorts   updater res_hash        sort_offset     edit_sort       update_time     language        is_publish      hot_sort      all_pinyin       img480_640      img480  bi_sort song_count      album_count     mv_count        source  is_pack is_spread       pack_hash       img_updatetime  style identity dt
2569    单色凌  张阳    DSL     D       男歌手  1       1       大陆    中国    单色凌，酷狗直播房间号：1428573，出生于武汉，创作型歌手。擅长多种乐器，担当自己所有作品的编曲配乐以及幕后制作。@BI_COLUMN_SPLIT@2010年创作《倾城诉》《隔三秋》《蝶变》等多首原创歌曲。@BI_COLUMN_SPLIT@2011年签约伯乐爱乐经纪公司。2011年5月23日，发表首张专辑《还原色》。2012年11月29日发表首支MV《最依赖的温柔》。2013年发表《没死掉就很好》，《最后的皇后》，《两种角色》等专辑。2014年发表EP《孤单格调》，单曲《无人像你》，《执着的形状》，《战中神话》。以及首张实体唱片《小岁月太着急》。2015年发表单曲《亲爱的不能到最后》《不期而遇》。2016年1月16日发行数字专辑《梦是我唯一能找到你的地方》。单色凌，内地创作型男歌手，擅长多种乐器。        20130705160258201556.jpg        20160225174030670260.jpg        1991-11-26      2012-01-06 18:19:30     2018-04-12 11:19:49    24981   334     孟婳莹  6983020acd0af3a1734746d298773e42        -5      397     2017-05-03 19:39:21     华语    1       NULL    DanSeLing       20121122190226304.jpg  20121122190226385.jpg   2567496 114     50      33      0       1       0       d0d740e48d870b73294a145a8016f81a        2018-09-27 16:05:38     5       7     2018-11-18

select * from common.k_singer_part where singerid = '766987' and dt='2018-11-18';

singerid        singername      othername       pinyin  cindex  sex     sextype areatype        areaname        country intro   short_intro     img330  img     birthday       addtime edittime        old_id  sorts   updater res_hash        sort_offset     edit_sort       update_time     language        is_publish      hot_sort      all_pinyin       img480_640      img480  bi_sort song_count      album_count     mv_count        source  is_pack is_spread       pack_hash       img_updatetime  style identity dt
766987  胡66    胡睿    H66     H       女      0       1       大陆    中国    胡66，中国内地女歌手，代表作《空空如也》，2017年10月加入酷狗直播 。@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@从艺历程：2016年，入围超级女声全国100强。@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@2017年10月，入驻酷狗直播，成为酷狗直播签约歌手。@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@2018年1月，翻唱了一首叫《空空如也》的歌走红。@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@2018年6月，发布新歌《浪人琵琶》。    胡66，华语女歌手，代表作《空空如也》          20181115142344232.jpg    1998-05-28      2017-12-14 18:30:37     2018-10-24 10:53:17     NULL    485     WB罗欣如                -9      57              华语    1     NULL                             10618130        26      10      10      32      1       0       a63238600b0a2e1d9de9e322f7a98882        2018-11-15 14:27:59     5     2018-11-18
```

**说明不能使用`k_album_part`中的singerid，只是部分id而已。**

```
#验证album表中的singerid是独立id还是部分id（现在不用复合id了。。。）
select * from common.k_album_part where albumid ='8631706' and dt='2018-11-18';

albumid albumname       other_info      img     singerid        singername      type    company publish_time    intro   language        is_publish      start_time    addtime  edittime        updater grade   grade_count     access_count    is_recommend    platform_recommend      collect_count   source  language_type   editor  is_finish      is_buy  song_count      the_first       quality hot     is_fake push_text       tme_sn  category        dt
8631706 浪人琵琶                20180620160223562693.jpg        766987  胡66、单色凌    单曲专辑        北京亿格艾科技有限公司  2018-06-21      本是花花公子 心无所属@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@为浪迹天涯之客 忠于自由 @BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@繁华世界，青楼酒馆@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@常年孤身一人 乐哉乐哉 非也@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@穿梭时空 得一红颜 悠哉悠哉@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@自知与其不配@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@也愿为其看破红尘 两袖清风 @BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@奈何只是美梦一场@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@亦学会了情为何物@BI_COLUMN_SPLIT@@BI_COLUMN_SPLIT@浪子回头金不换 心痛则动  国语  0000-00-00 00:00:00      2018-06-20 16:00:52     2018-07-09 12:33:51     柯冰梓  8.0     636     0       1       0       0       0       1       姚志辉  0       0     90443    0       {"is_push":0,"push_text_pay":"我的最新专辑【《albumname》】火爆售卖中","push_text_free":"发布专辑：《albumname》"}              1       2018-11-18
```

**结果显示可以用`k_singer_album_part`来显示singer和album之间的关系。**

```
#检查k_singer_album是否能用
select * from common.k_singer_album_part where albumid = '8631706' and dt='2018-11-18';

id      singerid        singername      albumid addtime dt
4445342 766987  胡66    8631706 2018-06-20 17:18:04     2018-11-18
4445343 2569    单色凌  8631706 2018-06-20 17:18:04     2018-11-18
```

查询是否有独立的歌手

```
select * from common.st_k_mixsong_part where songname = '卡路里' and dt='2018-11-18';

112832798       卡路里          9175221 西虹市首富 电影原声     283307  华语群星        国语    火箭少女101     mp3     4       BEDD046FB30A0C443CD6F854574B065E      3713839  232000  128     2018-07-26      1       2018-08-21 19:22:51     2018-11-12 11:18:33     0       1       0       100     0       4756359 1330878 WB崔京琳      2018-08-21 19:22:51              0               0       1       0       Akiyama Sayuri  李聪            29252924                        0       0       0       0     B129D9B4565CB1644C365A07E0F3B342 9284335         0       BEDD046FB30A0C443CD6F854574B065E        3713839 32              0               0       0       0874821E1FFDC05BBA00570F976E9B4A       33346823        1149    40457955        0       0       0       《西虹市首富》电影插曲  卡路里          1       130     2018-11-18
mixsongid       songname        other_info      albumid albumname       singerid        singername      language        choric_singer   extname gd_sort hash    filesize       timelength      bitrate publish_time    disc    addtime edittime        is_choric       is_single       is_break_rule   is_file_head    is_copyright    ownercount     playcount       updater upload_time     official_songname       is_recommend    editor  is_finish       is_publish      platform_break_rule     composer      lyrics   company songid  mv_hash high_mv_hash    mv_size high_mv_size    mv_type is_mv_file_head music_trac      hash_320        filesize_320    hash_m4a        m4a_size       hash_128        filesize_128    source  hash_192        filesize_192    hash_ape        filesize_ape    bitrate_ape     hash_flac       filesize_flac   bitrate_flac   scid    vip     mvid    is_search       remark  ori_audio_name  suffix_audio_name       version bpm     dt
```

```
#得到mixsongid是112832798，尝试得到独立歌手
select * from common.canal_k_album_audio_author where album_audio_id = '112832798';

id      author_id       album_audio_id  add_time        edit_time       editor
49435576        797251  112832798       2018-11-12 11:18:33     2018-11-12 11:18:33     WB崔京琳
```

**很多组合并不能查到其中具体地成员，比如火箭少女赖美云。**

```
#检索得到的歌手信息
select * from common.k_singer_part where singerid = '797251' and dt='2018-11-18';

singerid        singername      othername       pinyin  cindex  sex     sextype areatype        areaname        country intro   short_intro     img330  img     birthday       addtime edittime        old_id  sorts   updater res_hash        sort_offset     edit_sort       update_time     language        is_publish      hot_sort      all_pinyin       img480_640      img480  bi_sort song_count      album_count     mv_count        source  is_pack is_spread       pack_hash       img_updatetime  style identity dt
797251  火箭少女101     Rocket Girls    HJSN101 H       组合    2       1       大陆    中国    火箭少女是第一支从互联网诞生的女团，从诞生的第一天起，都在和青年文化、和音乐、和未来冲撞和对话，每一首作品也都在与市场交流对撞。火箭的发射是依靠反向推进器向地面喷射大量的气体，利用反作用力将火箭喷射向上，火箭少女亦是如此，拥有触底反弹，勇往直前的决心。                        20180816161328931.jpg   2018-06-23      2018-06-23 20:47:33     2018-09-20 09:47:17     NULL    41      wb_WB罗欣如           55       30              华语    1       NULL                            6652183 11      1       16      32      1       0       b35fc8179ce1e6e363a0b08eab18fee0      2018-09-28 00:28:01      5       1       2018-11-18
```

**查找备注，告知我们需要使用这个表的备注，使用canal_kugoumusiclib_k_album_audio_remark，其中type_id等于2（原声带类型），因为st_k_mixsong_part的remark字段已经被废弃。**

```
select * from common.st_k_mixsong_part where mixsongid = '88102868' and dt='2018-11-18';
mixsongid       songname        other_info      albumid albumname       singerid        singername      language        choric_singer   extname gd_sort hash    filesize       timelength      bitrate publish_time    disc    addtime edittime        is_choric       is_single       is_break_rule   is_file_head    is_copyright    ownercount     playcount       updater upload_time     official_songname       is_recommend    editor  is_finish       is_publish      platform_break_rule     composer      lyrics   company songid  mv_hash high_mv_hash    mv_size high_mv_size    mv_type is_mv_file_head music_trac      hash_320        filesize_320    hash_m4a        m4a_size       hash_128        filesize_128    source  hash_192        filesize_192    hash_ape        filesize_ape    bitrate_ape     hash_flac       filesize_flac   bitrate_flac   scid    vip     mvid    is_search       remark  ori_audio_name  suffix_audio_name       version bpm     dt
88102868        一起走过的日子          4017821 暖暖柔情粤语精选        1573    刘德华  粤语    刘德华  mp3     9       2922307E436F1337C0341E8B73F2B5B0        378878236721   128     1991-01-01      1       2017-09-17 08:43:55     2018-10-25 18:35:54     0       1       0       100     0       665817  148061  词曲人传染            0自动化入库      0       1       0       胡伟立  梁美薇          1268330                 0       0       0       0       0       B1AB2DBDBCE0DB078AEC8C1D208D1AF7      9466902          0       E4779B01E1FB3C8EC30723A75B494EB5        3786702 22      8031203B26A1586D2783CEB29172270C        5720943 9455D3FD9644DA659AE0A7F4D5164C09      23297821 787     8820FDFC1FB3698899CD44B1721B7F7C        23931645        809     2561507 0       0       0               一起走过的日子          1       68      2018-11-18

```

```
select * from canal_kugoumusiclib_k_album_audio_remark where album_audio_id = "88102868";
id      album_audio_id  type_id album_audio_remark      add_time        edit_time       editor  is_publish      rel_album_audio_id      audio_id
1803647 88102868        2       《反黑》电视剧插曲      2017-12-11 22:44:53     2018-02-06 16:52:23     陈庚    1       0       0
```



## 代码参考

使用搜索提示时的代码，作为参考：

###获取过去三天搜索词和有效搜播行为

```linux
#!/bin/bash
source $BIPROG_ROOT/bin/shell/common.sh
vDay=${DATA_DATE} #yesterday
yyyy_mm_dd_1=`date -d "$vDay" +%Y-%m-%d` #yesterday
yyyy_mm_dd_3=`date -d "$vDay -2 days" +%Y-%m-%d` #three days
sql="
set mapreduce.job.queuename=${q};
use temp;
create table if not exists temp.search_three_day_full
(
      dt        string,
      kw        string,
      scid_albumid string,
      hot       INT
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile;
insert overwrite table temp.search_three_day_full partition (cdt='${yyyy_mm_dd_1}')
select
        dt,
        kw,
        scid_albumid,
        hot
from (
        select
                dt,
                kw,
                scid_albumid,
                CASE WHEN (status='完整播放' or spt_cnt>=30) THEN 1 ELSE 0 END as hot
        from (
                select 
                        dt,
                        COALESCE(ROUND((CASE WHEN (
                                                    CASE WHEN tv<7900 AND spt<0 THEN 0
                                                         WHEN tv<7900 AND spt>=0 THEN spt
                                                         WHEN tv>=7900 AND ivar2<0 THEN 0
                                                         WHEN tv>=7900 AND ivar2>=0 THEN ivar2
                                                    ELSE 0 END)>50*st THEN st
                                        ELSE (CASE WHEN tv<7900 AND spt<0 THEN 0
                                                   WHEN tv<7900 AND spt>=0 THEN spt
                                                   WHEN tv>=7900 AND ivar2<0 THEN 0
                                                   WHEN tv>=7900 AND ivar2>=0 THEN ivar2
                                                   ELSE 0 END
                                        ) END)/1000,6),
                        '0') as spt_cnt,
                        split(fo,'/')[2] as kw,
                        status,
                        scid_albumid  
                from (
                        select 
                                dt,
                                CASE WHEN CAST(tv1 AS INT) IS NOT NULL THEN tv1 ELSE COALESCE(CAST(tv AS INT),'unknown') END AS tv,
                                COALESCE(TRIM(fo),'unknown') fo,
                                CASE WHEN TRIM(sty)='音频' THEN '音频'
                                WHEN TRIM(sty)='视频' THEN '视频'
                                ELSE 'unknown' END sty,
                                CASE WHEN TRIM(fs)='完整播放' THEN '完整播放'
                                WHEN TRIM(fs)='播放错误' THEN '播放错误'
                                WHEN TRIM(fs) IN ('被终止','播放中退出','暂停时退出','被終止') THEN '未完整播放'
                                ELSE '未知播放状态' END status,
                                case when abs(coalesce(cast(st as decimal(20,0)),0))>=10000000 then abs(coalesce(cast(st as decimal(20,0)),0))/1000 else abs(coalesce(cast(st as decimal(20,0)),0)) end  st,
                                case when coalesce(cast(spt as decimal(20,0)),0)>=10000000 then coalesce(cast(spt as decimal(20,0)),0)/1000 else coalesce(cast(spt as decimal(20,0)),0) end spt,
                                COALESCE(CAST(ivar2 AS decimal(20,0)),0) ivar2,
                                scid_albumid
                        from ddl.dt_list_ard_d
                        WHERE dt BETWEEN '${yyyy_mm_dd_3}' AND '${yyyy_mm_dd_1}'
                                and action='play'
                                and TRIM(fs)<>'播放错误'
                                and sh <>'00000000000000000000000000000000'
                                and st is not null
                                and st<>'null'
                                and ivar10 = '主动播放'
                                and TRIM(scid_albumid)<>'0'
                                and (fo like '%搜索/%' or fo='搜索') and (fo not like '%本地音乐/%')
                )t 
                where sty='音频'
        )u
)v
where hot>0
"
hive -e "$sql"
```

###提取三天内有效歌曲的过去十四天有效搜播行为

```linux
source $BIPROG_ROOT/bin/shell/common.sh
vDay=${DATA_DATE} #yesterday
yyyy_mm_dd_1=`date -d "$vDay" +%Y-%m-%d` #yesterday
yyyy_mm_dd_3=`date -d "$vDay -2 days" +%Y-%m-%d` #three days ago
yyyy_mm_dd_4=`date -d "$vDay -3 days" +%Y-%m-%d` #four days ago
yyyy_mm_dd_14=`date -d "$vDay -13 days" +%Y-%m-%d` #fourteen days ago

sql_unique="
set mapreduce.job.queuename=${q};
use temp;
drop table if exists temp.search_three_day_unique;
create table temp.search_three_day_unique
as
select scid_albumid from temp.search_three_day_full where cdt='${yyyy_mm_dd_1}' group by scid_albumid
"

sql_all="
set mapreduce.job.queuename=${q};
use temp;
create table if not exists temp.search_fourteen_day_full
(
      dt        string,
      scid_albumid string,
      hot       BIGINT
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile;
insert overwrite table temp.search_fourteen_day_full partition (cdt='${yyyy_mm_dd_1}')
select 
        dt,
        scid_albumid,
        sum(case when (status='完整播放' or spt_cnt>=30) then 1 else 0 end) as hot
from (
        select 
                dt,
                COALESCE(ROUND((CASE WHEN (
                                            CASE WHEN tv<7900 AND spt<0 THEN 0
                                                 WHEN tv<7900 AND spt>=0 THEN spt
                                                 WHEN tv>=7900 AND ivar2<0 THEN 0
                                                 WHEN tv>=7900 AND ivar2>=0 THEN ivar2
                                            ELSE 0 END)>50*st THEN st
                                ELSE (CASE WHEN tv<7900 AND spt<0 THEN 0
                                           WHEN tv<7900 AND spt>=0 THEN spt
                                           WHEN tv>=7900 AND ivar2<0 THEN 0
                                           WHEN tv>=7900 AND ivar2>=0 THEN ivar2
                                           ELSE 0 END
                                ) END)/1000,6),
                '0') as spt_cnt,
                status,
                scid_albumid  
        from (
                select 
                        dt,
                        CASE WHEN CAST(tv1 AS INT) IS NOT NULL THEN tv1 ELSE COALESCE(CAST(tv AS INT),'unknown') END AS tv,
                        CASE WHEN TRIM(sty)='音频' THEN '音频'
                        WHEN TRIM(sty)='视频' THEN '视频'
                        ELSE 'unknown' END sty,
                        CASE WHEN TRIM(fs)='完整播放' THEN '完整播放'
                        WHEN TRIM(fs)='播放错误' THEN '播放错误'
                        WHEN TRIM(fs) IN ('被终止','播放中退出','暂停时退出','被終止') THEN '未完整播放'
                        ELSE '未知播放状态' END status,
                        case when abs(coalesce(cast(st as decimal(20,0)),0))>=10000000 then abs(coalesce(cast(st as decimal(20,0)),0))/1000 else abs(coalesce(cast(st as decimal(20,0)),0)) end  st,
                        case when coalesce(cast(spt as decimal(20,0)),0)>=10000000 then coalesce(cast(spt as decimal(20,0)),0)/1000 else coalesce(cast(spt as decimal(20,0)),0) end spt,
                        COALESCE(CAST(ivar2 AS decimal(20,0)),0) ivar2,
                        trim(scid_albumid) scid_albumid
                from ddl.dt_list_ard_d e LEFT SEMI JOIN temp.search_three_day_unique f on (e.scid_albumid = f.scid_albumid)
                WHERE dt BETWEEN '${yyyy_mm_dd_14}' AND '${yyyy_mm_dd_1}'
                        and action='play'
                        and TRIM(fs)<>'播放错误'
                        and sh <>'00000000000000000000000000000000'
                        and st is not null
                        and st<>'null'
                        and ivar10 = '主动播放'
                        and TRIM(scid_albumid)<>'0'
                        and (fo like '%搜索/%' or fo='搜索') and (fo not like '%本地音乐/%')
        )t
        where sty='音频'
)a
group by dt, scid_albumid
"
hive -e "$sql_unique"
hive -e "$sql_all"
```

###计算歌曲的分值和热度

```scala
/**
  *
  * author: jomei
  * date: 2018/5/29 15:41
  */
import scala.math.pow
import scala.math.abs
import java.io.File
import java.text.SimpleDateFormat
import java.util.Calendar
import scala.collection.mutable.ListBuffer
import org.apache.spark.sql.expressions.{Window, WindowSpec}
import org.apache.spark.sql.{Column, SparkSession}
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.IntegerType
import java.util.regex.Matcher
import java.util.regex.Pattern

case class TempRow(label: Int, dt: String)
object Hot extends Serializable{

  def main(args: Array[String]):Unit = {
    //1)connect to cluster
    val warehouseLocation = new File("spark-warehouse").getAbsolutePath
    val spark = SparkSession
      .builder()
      .appName("Generate result for scid_albumid")
      .config("spark.sql.warehouse.dir", warehouseLocation)
      .enableHiveSupport()
      .getOrCreate()
    // For implicit conversions like converting RDDs to DataFrames
    import spark.implicits._
    spark.conf.set("spark.sql.shuffle.partitions", 2001)

    //2)initial variable
    val date_end = args(0) //first argument is yesterday
    val penality = 0.1
    val period = 14 // for burst and hot
    val moving_average_length = 7
    val hot_length = period - 2
    //filter_tag starts from 1, so hot_length will ensure the recent three days
    val category = List("ost", "插曲", "主题曲", "原声带", "配乐", "片尾曲",
      "片头曲", "originalsoundtrack", "原声", "宣传曲", "op",
      "ed", "推广曲", "角色歌", "in", "背景音乐", "tm", "钢琴曲",
      "开场曲", "剧中曲", "bgm", "暖水曲", "主题歌")
    val broadcasted = spark.sparkContext.broadcast(category)
    val alias = Map("^G.E.M.邓紫棋$|^G.E.M.邓紫棋(?=、)|(?<=、)G.E.M.邓紫棋(?=、)|(?<=、)G.E.M.邓紫棋$" -> "邓紫棋")
    val broadcasted_alias = spark.sparkContext.broadcast(alias)

    val date_period = getDaysPeriod(date_end, period - 1)
    //create df_date to fill the missing dt
    var date_list_buffer = new ListBuffer[TempRow]()
    for (dt <- date_period){
      date_list_buffer += TempRow(1, dt)
    }
    val date_list = date_list_buffer.toList
    val df_date = date_list.toDF
    df_date.createOrReplaceTempView("date_table")

    //3)function
    //define strip function
    val mystrip = udf{(a:String) =>
      if(a != null & a != ""){
        a.toLowerCase.replaceAll( "[\\p{P}+~$`^=|<>～｀＄＾＋＝｜＜＞￥×〾〿 ]" , "")
      }
      else {""}
    }
    //define alias function
    val myalias = udf{(a:String) =>
      if(a != null & a != ""){
        var result = ""
        var str = a.trim
        for ((k,v) <- broadcasted_alias.value){
          val pattern = Pattern.compile(k)
          val matcher = pattern.matcher(str)
          if (matcher.find()){
            str = str.substring(0, matcher.start()) + v + str.substring(matcher.end(), str.length)
            result = str
          }
        }
        result
      }
      else {""}
    }
    //define category function
    val myfunc = udf{(a:String) =>
      if(a != null & a != ""){
        var end = -1
        var result = ""
        for (term <- broadcasted.value){
          val position = a.toLowerCase.replace(".", "").replace(" ", "").indexOf(term)
          if (position > -1){
            if (position + term.length > end){
              result = term
              end = position + term.length
            }
            if (position + term.length == end){
              if (term.length > result.length){
                result = term
                end = position + term.length
              }
            }
          }
        }
        result
      }
      else {""}
    }

    //4)read date and create df_period with scid_albumid,hot,dt,label without missing dt
    val sql_scid_period = s"select dt, scid_albumid, hot from temp.search_fourteen_day_full where cdt='$date_end'"
    val df_scid_period = spark.sql(sql_scid_period)
    df_scid_period.persist()

    val df_all = df_scid_period.withColumn("label", lit(1))
    df_all.createOrReplaceTempView("table_all")
    val sql_period  = "select scid_albumid, a.dt, (case when a.dt=b.dt then hot else 0 end) as hot from date_table a RIGHT OUTER JOIN table_all b on a.label=b.label"
    val df_period_temp = spark.sql(sql_period)
    // to eliminate the extra data because of RIGHT OUTER JOIN
    val df_period = df_period_temp.groupBy("dt","scid_albumid").agg(max("hot").as("hot"))
    df_period.persist()

    //5)calculate the burst score and hot score, create df_cal_save with scid_albumid, burst, hot
    //use window function
    val weights = getWeight(moving_average_length)
    val index = List.range(0,moving_average_length)
    val window_ma = Window.partitionBy("scid_albumid").orderBy(asc("dt"))
    val period_ma = df_period.withColumn("weightedmovingAvg", weighted_average(index, weights, window_ma, df_period("hot")))
    //filter the last n rows, cause their weighted moving average we didn't calculate, cause data is missing
    val window_filter = Window.partitionBy("scid_albumid").orderBy(asc("dt"))
    val period_filter_temp = period_ma.withColumn("filter_tag", row_number.over(window_filter))
    period_filter_temp.persist()
    val period_filter = period_filter_temp.filter($"filter_tag" >= moving_average_length)
    //calculate the amp score
    val period_burst = period_filter.groupBy("scid_albumid").agg((last("weightedmovingAvg")-(mean("weightedmovingAvg") + lit(1.5)*stddev_samp("weightedmovingAvg"))).as("weightedamp"))

    //calculate the recent three days total hot
    //filter_tag starts from 1, so hot_length will ensure the recent three days
    val period_filter_hot = period_filter_temp.filter($"filter_tag" >= hot_length)
    val period_hot = period_filter_hot.groupBy("scid_albumid").agg(sum("hot") as "hot")

    //combine both(weightedamp and hot) of them together
    val period_cal = period_burst.as("d1")
                                 .join(period_hot.as("d2"), $"d1.scid_albumid" === $"d2.scid_albumid")
                                 .select($"d1.*",$"d2.hot")

    val df_cal_save = period_cal.withColumn("burst", bround($"weightedamp", 3))
                                .withColumn("hotTmp", $"hot".cast(IntegerType))
                                .drop("hot")
                                .withColumnRenamed("hotTmp", "hot")
                                .select("scid_albumid", "burst", "hot")
    df_cal_save.persist()
    //unpersist to release memory
    df_period.unpersist()
    df_scid_period.unpersist()
    period_filter_temp.unpersist()

    //6)standard the data, (data - mean)/std
    val df_variable = df_cal_save.select(mean(df_cal_save("hot")),stddev_samp(df_cal_save("hot")),mean(df_cal_save("burst")),stddev_samp(df_cal_save("burst")))
    val (hot_mean, hot_std, burst_mean, burst_std) = (df_variable.first.getDouble(0), df_variable.first.getDouble(1), df_variable.first.getDouble(2), df_variable.first.getDouble(3))
    //it will run at once
    val df_cal = df_cal_save.withColumn("burst_standard", ($"burst"-burst_mean)/burst_std)
                            .withColumn("hot_standard", ($"hot"-hot_mean)/hot_std)
    df_cal.persist()
    //calculate the dynamic value(hot_coefficient) to combine burst and hot
    val coefficient_temp = math.abs(df_cal.sort($"hot_standard".desc).limit(20).agg(mean($"hot_standard")).first.getDouble(0) /(df_cal.sort($"burst_standard".desc).limit(20).agg(mean($"burst_standard")).first.getDouble(0)))
    val coefficient_burst = 1
    var coefficient_temp2 = 0.0
    if (coefficient_temp * coefficient_burst < 1){
      coefficient_temp2 = 1.0
    }
    else if(coefficient_temp * coefficient_burst > 20){
      coefficient_temp2 = 20.0
    }
    else{
      coefficient_temp2 = coefficient_temp * coefficient_burst
    }
    val hot_coefficient = coefficient_temp2

    //to move up the minmum of hot-standard to ensure the sum operation below won't trouble in summing negative value
    val temp_min = df_cal.select(min($"hot_standard"))
                         .first
                         .getDouble(0)
    var hot_min = 0.0
    if( temp_min < 0){
      hot_min = math.abs(temp_min)
    }
    //we only use the positive part of burst_standard
    val df_temp = df_cal.withColumn("positive", when($"burst_standard" > lit(0), $"burst_standard").otherwise(lit(0)))
                         .withColumn("positive2", $"hot_standard" + lit(hot_min))
    val df_scid = df_temp.withColumn("result_temp", when($"positive" > lit(2), lit(hot_coefficient) * $"positive" + $"positive2").otherwise($"positive" + $"positive2"))
                         .select($"scid_albumid", bround($"result_temp", 3) as "hot")

    //save values:hot_mean, hot_std, burst_mean, burst_std, hot_coefficient, hot_min
    var save_value_buf = new ListBuffer[(Double, Double, Double, Double, Double, Double)]()
    save_value_buf += ((hot_mean, hot_std, burst_mean, burst_std, hot_coefficient, hot_min))
    val df_save_value = save_value_buf.toList
                                      .toDF("hot_mean", "hot_std", "burst_mean", "burst_std", "hot_coefficient", "hot_min")

    df_save_value.createOrReplaceTempView("savevalue_table")
    val sql_save_value_create = """
create table if not exists temp.jimmy_dt_save_value
(
        hot_mean DOUBLE,
        hot_std DOUBLE,
        burst_mean DOUBLE,
        burst_std DOUBLE,
        hot_coefficient DOUBLE,
        hot_min DOUBLE
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(sql_save_value_create)

    val sql_save_value = s"""
INSERT OVERWRITE TABLE temp.jimmy_dt_save_value PARTITION(cdt='$date_end') select hot_mean, hot_std, burst_mean, burst_std, hot_coefficient, hot_min from savevalue_table
"""
    spark.sql(sql_save_value)

    df_scid.persist()
    df_scid.createOrReplaceTempView("table_scid")
    //it will be used later
    val sql_scid_save_create = """
create table if not exists temp.jimmy_dt_hot_score
(
        scid_albumid string,
        hot DOUBLE
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(sql_scid_save_create)

    val sql_scid_save = s"""
INSERT OVERWRITE TABLE temp.jimmy_dt_hot_score PARTITION(cdt='$date_end') select scid_albumid, hot from table_scid
"""
    spark.sql(sql_scid_save)
    //unpersist to gain memory
    df_cal.unpersist()


    //7)using df_scid to extract name from common.st_k_mixsong_part
    //select singername, songname, albumname
    val sql_sn=s"""
select a.mixsongid, a.choric_singer, a.songname, a.albumname, b.hot
from common.st_k_mixsong_part a
inner join
table_scid b
where a.dt = '$date_end' and a.mixsongid=b.scid_albumid
"""
    val df_sn = spark.sql(sql_sn)
    //filter and format name
    val df_sn_sep = df_sn.withColumn("song", regexp_replace($"songname", "[ ]*\\([^\\(\\)]*\\)$", ""))
                         .withColumn("kind", regexp_extract($"songname", "\\(([^\\(\\)]*)\\)$", 1))
                         .withColumn("singer", regexp_replace($"choric_singer", "[ ]*\\([0-9]*\\)$", "")) //to eliminate the duplicate singer effect
//                         .withColumnRenamed("choric_singer", "singer") //to eliminate the space effect

    df_sn_sep.persist()

    df_sn_sep.createOrReplaceTempView("table_sep")
    //it will be used later
    val sql_sn_sep_save_create = """
create table if not exists temp.jimmy_dt_sn_sep
(
        mixsongid string,
        albumname string,
        song string,
        kind string,
        singer string,
        hot DOUBLE
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(sql_sn_sep_save_create)

    val sql_sn_sep_save = s"""
INSERT OVERWRITE TABLE temp.jimmy_dt_sn_sep PARTITION(cdt='$date_end') select mixsongid, albumname, song, kind, singer, hot from table_sep
"""
    //remember that we should select the item in the hive item order!!!!!!!!!!!!!!!
    spark.sql(sql_sn_sep_save)

    //8)save burst and its songname
    val df_burst_name = df_cal_save.as("d1")
                                   .join(df_sn_sep.as("d2"), $"d1.scid_albumid" === $"d2.mixsongid", "left")
                                   .select($"d1.*", $"d2.song", $"d2.singer", $"d2.kind", $"d2.albumname")

    //it will be used to check
    df_burst_name.createOrReplaceTempView("table_burst_name")
    val df_burst_name_save_create = """
create table if not exists temp.jimmy_dt_burst_name
(
        scid_albumid string,
        burst DOUBLE,
        hot INT,
        song string,
        singer string,
        kind string,
        albumname string
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(df_burst_name_save_create)

    val df_burst_name_save = s"""
INSERT OVERWRITE TABLE temp.jimmy_dt_burst_name PARTITION(cdt='$date_end') select scid_albumid, burst, hot, song, singer, kind, albumname from table_burst_name
"""
    spark.sql(df_burst_name_save)

    spark.stop() //to avoid ERROR LiveListenerBus: SparkListenerBus has already stopped! Dropping event SparkListenerExecutorMetricsUpdate
  }


  def getDaysPeriod(dt: String, interval: Int): List[String] = {
    var period = new ListBuffer[String]() //initialize the return List period
    period += dt
    val cal: Calendar = Calendar.getInstance() //reset the date in Calendar
    cal.set(dt.split("-")(0).toInt, dt.split("-")(1).toInt - 1, dt.split("-")(2).toInt)
    val dateFormat: SimpleDateFormat = new SimpleDateFormat("yyyy-MM-dd") //format the output date
    for (i <- 0 to interval - 1){
      cal.add(Calendar.DATE, - 1)
      period += dateFormat.format(cal.getTime())
    }
    period.toList
  }
  def getWeight(length: Int): List[Double]= {
    var sum = 0.0
    for (i <- 0 to length-1 ){
      sum += pow(0.5, i)
    }
    //    val weights = for (i <- 0 to length-1 ) yield pow(0.5, i)/sum // it will return scala.collection.immutable.IndexedSeq
    val weights = for (i <- List.range(0, length) ) yield pow(0.5, i)/sum
    //    var weights_buffer = new ListBuffer[Double]()
    //    for (i <- 0 to length-1 ){
    //      weights_buffer += pow(0.5, i)/sum
    //    }
    //    val weights = weights_buffer.toList
    weights
  }
  def weighted_average(index: List[Int], weights: List[Double], w: WindowSpec, c: Column): Column= {
    val wma_list = for (i <- index) yield (lag(c, i).over(w))*weights(i) // list comprehension, map also can do some easy thing, return scala.collection.immutable.IndexedSeq
    wma_list.reduceLeft(_ + _)
  }
}

```

###获取歌曲相关备注（外部曲库表）

```linux
source $BIPROG_ROOT/bin/shell/common.sh
vDay=${DATA_DATE} #yesterday
yyyy_mm_dd_1=`date -d "$vDay" +%Y-%m-%d` #yesterday

sql_remark="
set mapreduce.job.queuename=${q};
use temp;
create table if not exists temp.search_remark
(
      scid_albumid string,
      remark    string,
      hot       double,
      type      string,
      rel_album_audio_id string
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile;
insert overwrite table temp.search_remark partition (cdt='${yyyy_mm_dd_1}')
select 
        scid_albumid, 
        album_audio_remark as remark, 
        hot, 
        type_id as type, 
        rel_album_audio_id 
from (
        select 
                scid_albumid, 
                album_audio_remark, 
                hot, 
                type_id, 
                rel_album_audio_id, 
                row_number() over(partition by scid_albumid, type_id order by add_time desc) as rank 
        from (
                select 
                        b.scid_albumid, 
                        a.album_audio_remark, 
                        b.hot, a.add_time, 
                        a.type_id, 
                        a.rel_album_audio_id
                from common.canal_kugoumusiclib_k_album_audio_remark a 
                inner join temp.jimmy_dt_hot_score b 
                where a.album_audio_id=b.scid_albumid 
                    and b.cdt='${yyyy_mm_dd_1}'
                    and a.type_id in ('2', '4', '5', '7', '8') 
        )c
)d where rank = 1
"
hive -e "$sql_remark"
```

###信息融合生成最终搜索提示

```scala
/**
  *
  * author: jomei
  * date: 2018/5/29 15:41
  */
import scala.math.pow
import scala.math.abs
import java.io.File
import java.text.SimpleDateFormat
import java.util.Calendar
import scala.collection.mutable.ListBuffer
import org.apache.spark.sql.expressions.{Window, WindowSpec}
import org.apache.spark.sql.{Column, SparkSession}
import org.apache.spark.sql.functions._
import org.apache.spark.sql.types.IntegerType
import java.util.regex.Matcher
import java.util.regex.Pattern

case class TempRow(label: Int, dt: String)
object Suggest extends Serializable{

  def main(args: Array[String]):Unit = {
    //1)connect to cluster
    val warehouseLocation = new File("spark-warehouse").getAbsolutePath
    val spark = SparkSession
      .builder()
      .appName("Generate search suggest for yesterday")
      .config("spark.sql.warehouse.dir", warehouseLocation)
      .enableHiveSupport()
      .getOrCreate()
    // For implicit conversions like converting RDDs to DataFrames
    import spark.implicits._
    spark.conf.set("spark.sql.shuffle.partitions", 2001)

    //2)initial variable
    val date_end = args(0) //first argument is yesterday
    val hot_threshold = 30
    val penality = 0.1
    val period = 14 // for burst and hot
    val moving_average_length = 7
    val hot_length = period - 2
    //filter_tag starts from 1, so hot_length will ensure the recent three days
    val category = List("ost", "插曲", "主题曲", "原声带", "配乐", "片尾曲",
      "片头曲", "originalsoundtrack", "原声", "宣传曲", "op",
      "ed", "推广曲", "角色歌", "in", "背景音乐", "tm", "钢琴曲",
      "开场曲", "剧中曲", "bgm", "暖水曲", "主题歌")
    val broadcasted = spark.sparkContext.broadcast(category)
    val alias = Map("^G.E.M.邓紫棋$|^G.E.M.邓紫棋(?=、)|(?<=、)G.E.M.邓紫棋(?=、)|(?<=、)G.E.M.邓紫棋$" -> "邓紫棋")
    val broadcasted_alias = spark.sparkContext.broadcast(alias)

    val date_period = getDaysPeriod(date_end, period - 1)
    //create df_date to fill the missing dt
    var date_list_buffer = new ListBuffer[TempRow]()
    for (dt <- date_period){
      date_list_buffer += TempRow(1, dt)
    }
    val date_list = date_list_buffer.toList
    val df_date = date_list.toDF
    df_date.createOrReplaceTempView("date_table")

    //3)function
    //define strip function
    val mystrip = udf{(a:String) =>
      if(a != null & a != ""){
        a.toLowerCase.replaceAll( "[\\p{P}+~$`^=|<>～｀＄＾＋＝｜＜＞￥×〾〿 ]" , "")
      }
      else {""}
    }
    //define alias function
    val myalias = udf{(a:String) =>
      if(a != null & a != ""){
        var result = ""
        var str = a.trim
        for ((k,v) <- broadcasted_alias.value){
          val pattern = Pattern.compile(k)
          val matcher = pattern.matcher(str)
          if (matcher.find()){
            str = str.substring(0, matcher.start()) + v + str.substring(matcher.end(), str.length)
            result = str
          }
        }
        result
      }
      else {""}
    }
    //define category function
    val myfunc = udf{(a:String) =>
      if(a != null & a != ""){
        var end = -1
        var result = ""
        for (term <- broadcasted.value){
          val position = a.toLowerCase.replace(".", "").replace(" ", "").indexOf(term)
          if (position > -1){
            if (position + term.length > end){
              result = term
              end = position + term.length
            }
            if (position + term.length == end){
              if (term.length > result.length){
                result = term
                end = position + term.length
              }
            }
          }
        }
        result
      }
      else {""}
    }

    //4)read date
    //read remark data
    val sql_remark = s"select scid_albumid, remark, hot, type, rel_album_audio_id from temp.search_remark where cdt='$date_end'"
    val df_remark = spark.sql(sql_remark)
    df_remark.persist()
    //read seperated song data
    val sql_sn_sep_read = s"select mixsongid, albumname, song, kind, singer, hot from temp.jimmy_dt_sn_sep where cdt='$date_end'"
    val df_sn_sep = spark.sql(sql_sn_sep_read)

    //5)create related dataframe
    //generate remark related dataframe
    //===================================================
    //remark field without "", but we do it for safe purpose
    val df_remark_music = df_remark.filter($"type" === "2")
                                   .filter($"remark" =!= "")
                                   .select("scid_albumid", "remark", "hot")
    val df_remark_language = df_remark.filter($"type" === "8")
                                      .filter($"remark" =!= "")
                                      .select("scid_albumid", "remark", "hot")

    val df_remark_ref = df_remark.filter($"type" === "4" || $"type" === "5")
                                 .filter($"remark" =!= "")
                                 .filter($"rel_album_audio_id" =!= "0")
                                 .select("scid_albumid", "remark", "hot", "rel_album_audio_id")
                                 .withColumnRenamed("hot", "cover_hot")

    val df_remark_translate = df_remark.filter($"type" === "7")
                                       .filter($"remark".isNotNull && $"remark" =!= "")
                                       .select("scid_albumid", "remark", "hot")

    val df_remark_notion = df_remark_music.filter(not($"remark".contains("@BI_ROW_SPLIT@")))
                                          .withColumn("notion", regexp_extract($"remark", "《(.*)》", 1))
                                          .withColumn("category", myfunc(regexp_replace($"remark", "《.*》", "")))

    val df_remark_multiple = df_remark_music.filter($"remark".contains("@BI_ROW_SPLIT@"))
                                            .withColumn("remark_new", explode(split($"remark", "@BI_ROW_SPLIT@")))
                                            .select("scid_albumid", "hot", "remark_new")
                                            .withColumnRenamed("remark_new", "remark")
                                            .withColumn("notion", regexp_extract($"remark", "《(.*)》", 1))
                                            .withColumn("category", myfunc(regexp_replace($"remark", "《.*》", "")))

    val df_remak_final = df_remark_notion.select("scid_albumid", "hot", "remark", "notion", "category")
                                         .union(df_remark_multiple.select("scid_albumid", "hot", "remark", "notion", "category"))

    val df_remark_language_final = df_remark_language.filter(not($"remark".contains("@BI_ROW_SPLIT@")))
                                                     .withColumn("notion", regexp_extract($"remark", "《(.*)》", 1))
                                                     .withColumn("category", regexp_replace($"remark", "《.*》", ""))

    val df_remark_ref_cover = df_remark_ref.join(df_sn_sep, df_remark_ref("scid_albumid") === df_sn_sep("mixsongid"), "left")
                                           .select("scid_albumid", "cover_hot", "rel_album_audio_id", "song")
                                           .withColumnRenamed("song", "cover_song")
    val df_remark_ref_final = df_remark_ref_cover.join(df_sn_sep, df_remark_ref_cover("rel_album_audio_id") === df_sn_sep("mixsongid"), "left")
                                                 .filter($"hot".isNotNull)
                                                 .select("scid_albumid", "cover_hot", "rel_album_audio_id", "cover_song", "song", "hot")
    //===================================================

    //generate seperated song related dataframe
    //===================================================
    val df_singersong_alias = df_sn_sep.filter($"singer".isNotNull && $"singer" =!= "" && $"song".isNotNull && $"song" =!= "")
                                       .groupBy("singer", "song").agg(sum("hot")*penality as "result")
                                       .withColumn("term",concat($"singer", lit(" "), $"song"))
                                       .withColumn("alias_singer", myalias($"singer"))
                                       .withColumn("alias", when($"alias_singer" =!= "", concat($"alias_singer", lit(" "), $"song")).otherwise(""))

    val df_singersong = df_singersong_alias.select("term", "result", "alias")

    val df_songsinger_alias = df_sn_sep.filter($"singer".isNotNull && $"singer" =!= "" && $"song".isNotNull && $"song" =!= "")
                                       .groupBy("singer", "song").agg(sum("hot")*penality as "result")
                                       .withColumn("term",concat($"song", lit(" "), $"singer"))
                                       .withColumn("alias_singer", myalias($"singer"))
                                       .withColumn("alias", when($"alias_singer" =!= "", concat($"song", lit(" "), $"alias_singer")).otherwise(""))

    val df_songsinger = df_songsinger_alias.select("term", "result", "alias")

    val df_singer_single_pre = df_sn_sep.filter($"singer".isNotNull && $"singer" =!= "")
                                        .filter(not($"singer".contains("、")))
                                        .groupBy("singer").agg(sum("hot") as "result")
                                        .withColumnRenamed("singer", "term")

    val df_singer_multiple_pre = df_sn_sep.filter($"singer".isNotNull && $"singer" =!= "")
                                          .filter($"singer".contains("、"))
                                          .groupBy("singer").agg(sum("hot") as "pre_result")
                                          .withColumnRenamed("singer", "term")

    val df_singer_multiple = df_singer_multiple_pre.withColumn("result", $"pre_result"*penality*penality) //to avoid same with multiple singer and singer/song
                                                   .select("term", "result")

    val df_singer_multiple_sep = df_singer_multiple_pre.withColumn("singer_sep", explode(split($"term", "、")))
                                                       .select("pre_result", "singer_sep")
                                                       .groupBy("singer_sep").agg(sum("pre_result") as "result") //sum("pre_result")*penality*5:to overcome the some singer too high according to hechang
                                                       .withColumnRenamed("singer_sep", "term")

    val df_singer_single = df_singer_single_pre.select("term", "result")
                                               .union(df_singer_multiple_sep.select("term", "result"))
                                               .groupBy("term").agg(sum("result") as "result")

    val df_singer = df_singer_single.select("term", "result")
                                    .union(df_singer_multiple)
                                    .withColumn("alias", myalias($"term"))
                                    .select("term", "result", "alias")

    val df_song = df_sn_sep.filter($"song".isNotNull && $"song" =!= "")
                           .groupBy("song").agg(sum("hot") as "result")
                           .withColumnRenamed("song", "term")
                           .withColumn("alias", lit(""))
                           .select("term", "result", "alias")

    val df_songkind = df_sn_sep.filter($"kind".isNotNull && $"kind" =!= "")
                               .withColumn("kindfilter", lower($"kind"))
                               .filter($"kindfilter" =!= "live")
                               .withColumn("term",concat($"song", lit(" "), $"kind"))
                               .groupBy("term").agg(sum("hot")*penality as "result")
                               .withColumn("alias", lit(""))
                               .select("term", "result", "alias")

    val df_album = df_sn_sep.filter($"albumname".isNotNull && $"albumname" =!= "")
                            .filter(not($"albumname" rlike ".*原声[带]?$"))
                            .groupBy("albumname").agg(sum("hot")*penality*penality as "result")
                            .withColumnRenamed("albumname", "term")
                            .withColumn("alias", lit(""))
                            .select("term", "result", "alias")

    val df_notion = df_remak_final.filter($"notion".isNotNull && $"notion" =!= "")
                                  .groupBy("notion").agg(sum("hot") as "result")
                                  .withColumnRenamed("notion", "term")
                                  .withColumn("alias", lit(""))
                                  .select("term", "result", "alias")

    val df_notioncategory = df_remak_final.filter($"notion".isNotNull && $"notion" =!= "" && $"category".isNotNull && $"category" =!= "")
                                          .filter($"category" =!= "原声带" && $"category" =!= "原声")
                                          .withColumn("term",concat($"notion", lit(" "), $"category"))
                                          .groupBy("term").agg(sum("hot")*penality as "result")
                                          .withColumn("alias", lit(""))
                                          .select("term", "result", "alias")

    val df_language = df_remark_language_final.filter($"notion".isNotNull && $"notion" =!= "" && $"category".isNotNull && $"category" =!= "")
                                              .withColumn("term",concat($"notion", lit(" "), $"category"))
                                              .groupBy("term").agg(sum("hot")*penality as "result")
                                              .withColumn("alias", lit(""))
                                              .select("term", "result", "alias")

    val df_translate = df_remark_translate.groupBy("remark").agg(sum("hot") as "result")
                                          .withColumnRenamed("remark", "term")
                                          .withColumn("alias", lit(""))
                                          .select("term", "result", "alias")

    val window_cover_song = Window.partitionBy("cover_song")
    val window_song = Window.partitionBy("song")

    val df_ref_diff = df_remark_ref_final.filter($"song" =!= $"cover_song")
                                         .withColumn("cover_value", sum($"cover_hot").over(window_cover_song))
                                         .withColumn("value", max($"hot").over(window_song)) //change avg to max, cause it will exists many-many relation between song and hot, some hots are so small, it will lower the sum value, eg:凉凉
                                         .withColumn("term",concat($"cover_song", lit(" 原唱")))
                                         .withColumn("penality", $"value"*penality)
                                         .groupBy("term").agg(max("penality") as "result")
                                         .select("term", "result")

    val df_ref_eql = df_remark_ref_final.filter($"song" === $"cover_song")
                                        .withColumn("cover_value", sum($"cover_hot").over(window_cover_song))
                                        .withColumn("value", max($"hot").over(window_song)) //change avg to max, cause it will exists many-many relation between song and hot, some hots are so small, it will lower the sum value, eg:凉凉
                                        .withColumn("term",concat($"cover_song", lit(" 原唱")))
                                        .withColumn("penality", when($"cover_value" > $"value", $"value"*penality as "result").otherwise($"value"*penality*penality*penality as "result"))
                                        .groupBy("term").agg(max("penality") as "result")
                                        .select("term", "result")

    val df_ref = df_ref_eql.union(df_ref_diff)
                           .groupBy("term").agg(max("result") as "result")
                           .withColumn("alias", lit(""))
                           .select("term", "result","alias")//change the order otherwise it will change the column result from double to string
    //===================================================

    //6) union all related dataframe to df_final(created by song self)
    val df_term = df_singersong.union(df_songsinger)
                               .union(df_singer)
                               .union(df_song)
                               .union(df_songkind)
                               .union(df_album)
                               .union(df_notion)
                               .union(df_notioncategory)
                               .union(df_language)
                               .union(df_translate)
                               .union(df_ref)
                               .groupBy("term", "alias").agg(max("result") as "result")

    df_term.persist()

    //7)deal with manual input
    //read saved value
    //read kw and hot within three days
    val sql_kw_read= s"select dt, kw, scid_albumid, hot from temp.search_three_day_full where cdt = '$date_end'"
    val df_kw_dt = spark.sql(sql_kw_read)
    val df_kw_all = df_kw_dt.groupBy("kw").agg(sum("hot") as "hot")
                            .filter($"hot" > hot_threshold) //add the minimum threshold

    //standard the data, (data - mean)/std
    // use save value from yesterday
    val sql_read_value = s"select hot_mean, hot_std, burst_mean, burst_std, hot_coefficient, hot_min from temp.jimmy_dt_save_value where cdt='$date_end'"
    val df_save_value = spark.sql(sql_read_value)
    val (hot_mean, hot_std, burst_mean, burst_std, hot_coefficient, hot_min) = (df_save_value.first.getDouble(0), df_save_value.first.getDouble(1), df_save_value.first.getDouble(2), df_save_value.first.getDouble(3), df_save_value.first.getDouble(4), df_save_value.first.getDouble(5))

    //use saved values:hot_mean, hot_std, burst_mean, burst_std, hot_coefficient, hot_min
    val df_kw_cal = df_kw_all.withColumn("hot_standard", ($"hot"-hot_mean)/hot_std)

    //we only use the positive part of hot_standard
    val df_kw_manual = df_kw_cal.withColumn("positive2", $"hot_standard" + lit(hot_min))
                                .filter($"positive2" > 0)
                                .select($"kw", bround($"positive2", 3) as "hot")

    //8) merge kw and df_final to df_mix with term, result, alias
    val df_kw_manual_strip = df_kw_manual.withColumn("origin2", mystrip($"kw"))
                                         .groupBy("origin2").agg(max("hot") as "hot")
    val df_final_strip = df_term.withColumn("origin", mystrip($"term"))

    val df_kw_manual_final = df_kw_manual_strip.join(df_final_strip, df_kw_manual_strip("origin2") === df_final_strip("origin"), "left")
    //list only manual exist term
    val df_kw_manual_unique = df_kw_manual_final.filter($"origin".isNull)
                                                .groupBy("origin2").agg(sum("hot") as "result") //to sum all the input by person
                                                .withColumnRenamed("origin2", "term")
                                                .withColumn("alias", myalias($"term"))
                                                .select("term", "result", "alias")
    //list both exist but manual is bigger than df_final
    val df_anti = df_kw_manual_final.filter($"hot" > $"result") //to overcome some small thing override
                                    .select("origin2", "hot", "term")
                                    .withColumnRenamed("term", "term_out")

    val df_final_exist = df_term.join(df_anti, df_term("term") === df_anti("term_out"), "leftanti")
                                 .select("term", "result", "alias")

    val df_kw_manual_exist = df_anti.groupBy("origin2").agg(max("hot") as "result")
                                    .withColumnRenamed("origin2", "term")
                                    .withColumn("alias", myalias($"term"))
                                    .select("term", "result", "alias")

    val df_mix = df_final_exist.union(df_kw_manual_exist)
                               .union(df_kw_manual_unique)
                               .groupBy("term", "alias").agg(max("result") as "result")
    df_mix.persist()


    //9)add the tag of variety to df_search with term, result, alias
    // for example chuangzao101
    val sql_kw_variety_album = s"""
select a.singername, a.albumid
from common.k_singer_album_part a
LEFT SEMI JOIN
(
select singerid
from common.k_singer_part
where dt='$date_end' and sextype = '12'
group by singerid
)b
on
(a.singerid = b.singerid
AND a.dt='$date_end')
"""
    val df_variety_album = spark.sql(sql_kw_variety_album)
    df_variety_album.createOrReplaceTempView("varitey_albumtable")


    val sql_kw_variety_song = s"""
select a.mixsongid, a.albumid
from common.st_k_mixsong_part a
LEFT SEMI JOIN
varitey_albumtable b
on
(a.albumid = b.albumid
AND a.dt='$date_end')
"""
    val df_variety_song = spark.sql(sql_kw_variety_song)
    //read
    val sql_scid_read=s"select scid_albumid, hot from temp.jimmy_dt_hot_score where cdt = '$date_end'"
    val df_scid = spark.sql(sql_scid_read)
    df_scid.createOrReplaceTempView("table_scid")

    val df_variety_temp = df_variety_album.join(df_variety_song, df_variety_album("albumid") === df_variety_song("albumid"))
                                          .drop("albumid") //it will remove two columns
    val df_variety = df_variety_temp.join(df_scid, df_variety_temp("mixsongid") === df_scid("scid_albumid"))
                                    .drop("mixsongid","scid_albumid","cdt")
                                    .groupBy("singername").agg(sum("hot")*penality as "result")
                                    .withColumnRenamed("singername", "term")
                                    .withColumn("alias", myalias($"term"))
                                    .select("term", "result", "alias")

    val df_search = df_mix.select("term", "result", "alias")
                          .union(df_variety)
                          .groupBy("term", "alias").agg(max("result") as "result")

    df_search.persist()

    //10) read from specified tag
    val sql_scid_hot = s"""
select a.scid_albumid, b.scid, a.hot
from
table_scid a
left join
common.st_k_mixsong_part b
on
(a.scid_albumid = b.mixsongid
AND b.dt='$date_end')
"""
    val df_scid_album_hot = spark.sql(sql_scid_hot)
    // there are some missing scid, it doesn't matter

    val df_scid_hot = df_scid_album_hot.filter($"scid".isNotNull)
                                       .groupBy("scid").agg(sum("hot") as "hot")

    df_scid_hot.createOrReplaceTempView("table_scid_hot")
    //there exist some missing for scid with tagid, remove the tagid is Null
    val sql_scid_tag_hot = s"""
select
        c.scid,
        c.tagid,
        d.tagname,
        c.hot
from (
        select
                a.scid,
                a.hot,
                b.tagid
        from table_scid_hot a
        left join common.k_sc_tag_part b
        on (
                a.scid = b.scid
                AND b.dt='$date_end'
        )
        where b.tagid IS NOT NULL
) c
left join common.k_tag_sc_part d
on (
        c.tagid = d.tagid
        AND d.dt='$date_end'
)
where d.tagname IS NOT NULL
"""
    val df_scid_tag_hot = spark.sql(sql_scid_tag_hot)
                               .groupBy("scid","hot").agg(concat_ws("-", collect_set("tagname")) as "tag")
    df_scid_tag_hot.createOrReplaceTempView("table_scid_tag_hot")
    df_scid_tag_hot.persist() //to save execute time by line 461

    //to avoid these table be removed by system
    val sql_tag_keyword_create = """
create table if not exists temp.jimmy_tag
(
tag string,
kw string
)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(sql_tag_keyword_create)

    val sql_tag_keyword = """
select
        tag,
        kw
from temp.jimmy_tag
"""
    val df_tag_keyword = spark.sql(sql_tag_keyword)
    val tag_map = df_tag_keyword.select("tag","kw").as[(String, String)].collect.toMap

    var tag_hot_buf = new ListBuffer[(String, Double)]() //result is also double type
    for ((k,v) <- tag_map){ // k is the key, v is the value
      if(v != null & v != ""){ //to avoid null and empty string
        tag_hot_buf += ((k, df_scid_tag_hot.filter(v.split("-").toList.foldLeft(lit(true))((acc, x) => (acc && col("tag").contains(x)))).agg(sum("hot")).first.getAs[Double](0)))
      }
      else {
        tag_hot_buf += ((k, 0))
      }
    }
    val tag_hot = tag_hot_buf.toList
                             .toDF("term","result")
                             .withColumn("alias", lit(""))
                             .select("term", "result", "alias")

    val df_triple = df_search.select("term", "result", "alias")
                             .union(tag_hot)
                             .groupBy("term", "alias").agg(max("result") as "result")

    df_triple.createOrReplaceTempView("search_savetable")
    df_triple.persist()
    val sql_search_create= """
create table if not exists temp.jimmy_dt_hot_result_mix
(
term string,
result DOUBLE,
alias string
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(sql_search_create)

    val sql_search= s"""
INSERT OVERWRITE TABLE temp.jimmy_dt_hot_result_mix PARTITION(cdt='$date_end') select term, result, alias from search_savetable
"""
    spark.sql(sql_search)

    //11) add num to df_search, then create df_search_num with term, result, resultnum, alias
    val sql_search_num = """
select
term,
result,
resultnum,
alias
from search_savetable a
left join
(
select keyword, resultnum
from
(
select keyword, resultnum, row_number() over(partition by keyword order by ren desc) as rank
from
(
select kw as keyword, ivar5 resultnum, count(distinct mid) as ren
from
ddl.dt_list_ard_d
where dt="""+s"""'$date_end'"""+"""
and action='search'
and b like '搜索页-%'
and ivar5 is not null
and ivar5 <>'null'
and ivar5 <>''
group by kw, ivar5
)b
)b where rank=1
)b on a.term = b.keyword
"""
    val df_search_num = spark.sql(sql_search_num)
    df_search_num.createOrReplaceTempView("num_search_savetable")
    val sql_result_num_create= """
create table if not exists temp.jimmy_dt_hot_result_num_mix
(
term string,
result DOUBLE,
resultnum INT,
alias string
)
partitioned by (cdt string)
row format delimited fields terminated by '|' lines terminated by '\n' stored as textfile
"""
    spark.sql(sql_result_num_create)

    val sql_result_num= s"""
INSERT OVERWRITE TABLE temp.jimmy_dt_hot_result_num_mix PARTITION(cdt='$date_end') select term, result, resultnum, alias from num_search_savetable
"""
    spark.sql(sql_result_num)

    spark.stop() //to avoid ERROR LiveListenerBus: SparkListenerBus has already stopped! Dropping event SparkListenerExecutorMetricsUpdate
  }


  def getDaysPeriod(dt: String, interval: Int): List[String] = {
    var period = new ListBuffer[String]() //initialize the return List period
    period += dt
    val cal: Calendar = Calendar.getInstance() //reset the date in Calendar
    cal.set(dt.split("-")(0).toInt, dt.split("-")(1).toInt - 1, dt.split("-")(2).toInt)
    val dateFormat: SimpleDateFormat = new SimpleDateFormat("yyyy-MM-dd") //format the output date
    for (i <- 0 to interval - 1){
      cal.add(Calendar.DATE, - 1)
      period += dateFormat.format(cal.getTime())
    }
    period.toList
  }
  def getWeight(length: Int): List[Double]= {
    var sum = 0.0
    for (i <- 0 to length-1 ){
      sum += pow(0.5, i)
    }
    //    val weights = for (i <- 0 to length-1 ) yield pow(0.5, i)/sum // it will return scala.collection.immutable.IndexedSeq
    val weights = for (i <- List.range(0, length) ) yield pow(0.5, i)/sum
    //    var weights_buffer = new ListBuffer[Double]()
    //    for (i <- 0 to length-1 ){
    //      weights_buffer += pow(0.5, i)/sum
    //    }
    //    val weights = weights_buffer.toList
    weights
  }
  def weighted_average(index: List[Int], weights: List[Double], w: WindowSpec, c: Column): Column= {
    val wma_list = for (i <- index) yield (lag(c, i).over(w))*weights(i) // list comprehension, map also can do some easy thing, return scala.collection.immutable.IndexedSeq
    wma_list.reduceLeft(_ + _)
  }
}

```

## 数据提取

经过验证，java程序内的数据都是10月1号的，因而我们需要在这一天构建知识图谱。

###java中数据的提取代码

```
hive -e"set hive.cli.print.header=true;
set mapreduce.job.queuename=root.baseDepSarchQueue;
select songname, max(ownercount), max(playcount) from (
select songname, ownercount, playcount from common.st_k_mixsong_part where dt='2018-10-01' and ownercount>0 and playcount>0 )a group by songname;">song_info.csv

hive -e"set hive.cli.print.header=true;
set mapreduce.job.queuename=root.baseDepSarchQueue;
select singername, sextype, min(edit_sort) from (
select edit_sort, singername, sextype from common.k_singer_part where dt='2018-10-01'
)a group by singername, sextype;">singer_info.csv

hive -e"set hive.cli.print.header=true;
set mapreduce.job.queuename=root.baseDepSarchQueue;
select albumname, max(hot) from (
select albumname, hot from common.k_album_part where dt='2018-10-01' and hot > 0
)a group by albumname;">album_info.csv
```

### 改进后的数据提取代码

#### 先创建临时表
以应对`common.canal_k_album_audio_author`表不在hive库中的问题。

```
hive -e"
set mapreduce.job.queuename=root.baseDepSarchQueue;
use temp;
CREATE TABLE IF NOT EXISTS temp.jimmy_filter_mixsong_singerid(
  author_id string COMMENT '歌手ID', 
  album_audio_id string COMMENT '歌曲ID')
PARTITIONED BY (cdt string)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY '|' 
  LINES TERMINATED BY '\n' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  'hdfs://kgdc/user/hive/warehouse/temp.db/jimmy_filter_mixsong_singerid'
;">createsampletable.txt

hive -e"
set mapreduce.job.queuename=root.baseDepSarchQueue;
use temp;
CREATE TABLE IF NOT EXISTS temp.jimmy_mixsong_basic(
  mixsongid string COMMENT '歌曲ID',
  songname string COMMENT '歌曲名',
  albumname string COMMENT '专辑名',
  singername string COMMENT '歌手名', 
  sextype int COMMENT '歌手性别类型')
PARTITIONED BY (cdt string)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY '|' 
  LINES TERMINATED BY '\n' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  'hdfs://kgdc/user/hive/warehouse/temp.db/jimmy_mixsong_basic'
;">createsampletable.txt
```

~~先得到符合条件的mixsongid~~，算了直接并在下面计算。

```scala
val date_end = "2018-10-01"
val sql_song = s"select mixsongid, songname, albumname, max(ownercount) as ownercount, max(playcount) as playcount from (select mixsongid, songname, albumname, ownercount, playcount from common.st_k_mixsong_part where dt='$date_end' and ownercount>0 and playcount>0 )a group by mixsongid, songname, albumname"
val df_song = spark.sql(sql_song)
df_song.persist()
df_song.createOrReplaceTempView("song_table")
```

####然后利用hive得到相关的singerid

```linux
hive -e"set hive.cli.print.header=true;
set mapreduce.job.queuename=root.baseDepSarchQueue;
insert overwrite table temp.jimmy_filter_mixsong_singerid PARTITION (cdt='2018-10-01')
select author_id, album_audio_id from common.canal_k_album_audio_author as a LEFT SEMI JOIN (select mixsongid, songname, albumname, max(ownercount) as ownercount, max(playcount) as playcount from (select mixsongid, songname, albumname, ownercount, playcount from common.st_k_mixsong_part where dt='2018-10-01' and ownercount>0 and playcount>0 )a group by mixsongid, songname, albumname) b on (a.album_audio_id = b.mixsongid);"
```

####接着，得到各自singer和song的信息，将其join得到basic信息

```scala
val date_end = "2018-10-01"
val sql_relation = s"select author_id, album_audio_id from temp.jimmy_filter_mixsong_singerid where cdt='$date_end'"
val df_relation = spark.sql(sql_relation)
df_relation.persist()
df_relation.createOrReplaceTempView("relation_table")
//scala> df_relation.count()
//res2: Long = 1728830

val sql_song = s"select mixsongid, songname, albumname, max(ownercount) as ownercount, max(playcount) as playcount from (select mixsongid, songname, albumname, ownercount, playcount from common.st_k_mixsong_part where dt='$date_end' and ownercount>0 and playcount>0 )a group by mixsongid, songname, albumname"
val df_song = spark.sql(sql_song)
df_song.persist()
df_song.createOrReplaceTempView("song_table")
//scala> df_song.count()
//res11: Long = 1923657

//otherwise it will raise errors, it's a bug unless 2.1.1 version
spark.conf.set("spark.sql.crossJoin.enabled", "true")
val sql_singer = s"select singername, sextype, singerid, album_audio_id from (select d.singername, d.sextype, d.singerid, c.album_audio_id from relation_table as c LEFT JOIN (select singername, sextype, singerid from common.k_singer_part as a LEFT SEMI JOIN relation_table b on (a.dt='$date_end' and a.singerid = b.author_id))d on (c.author_id = d.singerid))e group by singername, sextype, singerid, album_audio_id"
val df_singer = spark.sql(sql_singer)
df_singer.persist()
df_singer.createOrReplaceTempView("singer_table")
//scala> df_singer.count()
//res6: Long = 1728805

val df_basic = df_song.filter($"ownercount" > 10).as("d1").join(df_singer.as("d2"),$"d1.mixsongid" === $"d2.album_audio_id").select($"d1.mixsongid", $"d1.songname", $"d1.albumname", $"d2.singername", $"d2.sextype")
//scala> df_basic.count()
//res23: Long = 1086276  
//scala> df_basic.dropDuplicates().count()
//res24: Long = 1086276  
df_basic.persist()
df_basic.createOrReplaceTempView("basic_table")

val sql_save_value = s"""
INSERT OVERWRITE TABLE temp.jimmy_mixsong_basic PARTITION(cdt='$date_end') select mixsongid, songname, albumname, singername, sextype from basic_table
"""
spark.sql(sql_save_value)
```

####最后，通过hive得到remark信息

```linux
hive -e"set hive.cli.print.header=true;
set mapreduce.job.queuename=root.baseDepSarchQueue;
select  mixsongid, songname, albumname, singername, sextype, album_audio_remark from (select mixsongid, songname, albumname, singername, sextype from temp.jimmy_mixsong_basic where cdt='2018-10-01')c LEFT JOIN (select album_audio_id, album_audio_remark from common.canal_kugoumusiclib_k_album_audio_remark as a LEFT SEMI JOIN (select mixsongid, songname, albumname, singername, sextype from temp.jimmy_mixsong_basic where cdt='2018-10-01')b on (a.album_audio_id = b.mixsongid and a.type_id = '2'))d on (c.mixsongid = d.album_audio_id);">kg_info.csv

//1134296 rows
```