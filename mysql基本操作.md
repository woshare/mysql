#### mysql基本操作

###根据条件更新数据
>1，UPDATE table1 set column_name1=case 2 when 2 then 'hh'  else column_name1 end, column_name2=case 2 when 2 then 'xx'  else column_name2 end where id=5;    



### INSERT IGNORE INTO wordrecite(uid,word,belong,status,marktime,belong_type) SELECT 1007235,word,belong,status,marktime,belong_type FROM wordrecite WHERE uid='1007168' and marktime BETWEEN '2019-07-17' and '2019-07-18';   

select wordrecite.uid,AES_DECRYPT(from_base64(users.phone_number),"saltsaltsaltsalt") as phone_number,count(*) as recitedWordsCount from wordrecite join users on wordrecite.uid=users.uid group by uid;

select * from users where phone_number=to_base64(AES_ENCRYPT('18612355155',"saltsaltsaltsalt")) ;

cat wordii.txt wordc.txt| sort | uniq -u | sort -n - 比较两个文件不同的行

ALTER TABLE mywordlistbook ADD COLUMN op_status int(8) NOT NULL DEFAULT '0' COMMENT '0表示是逻辑存在的，1表示被逻辑删除了';

#ALTER TABLE mywordlists CHANGE word_list_book_id word_list_book_ids bigint(20) NOT NULL;
#ALTER TABLE mywordlists CHANGE word_list_book_ids word_list_book_id bigint(20) NOT NULL;
#ALTER TABLE mywordlists DROP FOREIGN KEY word_wordbook;
#ALTER TABLE mywordlists DROP INDEX word_list_book_id;
##ALTER TABLE mywordlists CHANGE word_list_book_ids word_list_book_id bigint(20) NOT NULL;
ALTER TABLE mywordlists ADD CONSTRAINT `word_unique` UNIQUE Key(`word_list_book_id`,`words`(50),`word_origin`(10),`word_origin_type`(3),`word_context`(150));
ALTER TABLE mywordlists ADD CONSTRAINT `word_wordbook` FOREIGN KEY (`word_list_book_id`) REFERENCES `mywordlistbook` (`id`) ON DELETE CASCADE;


ALTER TABLE mywordlistbook CHANGE uid uids varchar(20) NOT NULL COMMENT '用户id';
ALTER TABLE mywordlistbook DROP INDEX uid;
ALTER TABLE mywordlistbook CHANGE uids uid varchar(20) NOT NULL COMMENT '用户id';

ALTER TABLE mywordlistbook CHANGE create_time create_time timestamp(3) NOT NULL DEFAULT CURRENT_TIMESTAMP(3) COMMENT '创建时间';

##  008修改 （内侧修改，公测，公正环境没有改）ALTER TABLE longlifediclibs  ADD COLUMN mark_time timestamp(3) NOT NULL DEFAULT CURRENT_TIMESTAMP(3) COMMENT '单词学习时间';

ALTER TABLE longlifediclibs drop COLUMN mark_time;


update longlifediclibs set mark_time=marktime where marktime!='1970-01-01 08:00:00.000';
update longlifediclibs set mark_time=FROM_UNIXTIME(UNIX_TIMESTAMP(marktime)+10) where marktime='1970-01-01 08:00:00.000';
ALTER TABLE longlifediclibs CHANGE maketime maketime_notneed datetime COMMENT '单词学习时间';
ALTER TABLE longlifediclibs CHANGE maketime maketime timestamp(3) NOT NULL DEFAULT CURRENT_TIMESTAMP(3) COMMENT '单词学习时间';

UPDATE dictionary_jianqiao_youdao 
RIGHT JOIN
dictionary_youdao
ON
dictionary_youdao.keyword=dictionary_jianqiao_youdao.keyword
SET dictionary_jianqiao_youdao.mp3_url_us=dictionary_youdao.mp3_url_us
WHERE
dictionary_youdao.mp3_url_us!=''
AND
dictionary_jianqiao_youdao.mp3_url_us is NULL


### 求差集

SELECT a.oname, a.odesc 
FROM
  object_a a 
  LEFT JOIN object_b b 
    ON a.oname = b.oname 
    AND a.odesc = b.odesc 
WHERE b.id IS NULL

ALTER  TABLE mywordlists RENAME TO mywordlists_old

CREATE TABLE `mywordlists_new` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '标识id',
  `word_list_book_id` bigint(20) NOT NULL,
  `words` varchar(100) NOT NULL COMMENT '单词，词组，句子',
  `word_origin` varchar(100) DEFAULT NULL COMMENT '单词来源',
  `word_origin_type` varchar(50) DEFAULT NULL COMMENT '单词来源类型',
  `word_context` varchar(300) DEFAULT NULL COMMENT '单词上下文',
  `cn_explains` text COMMENT '中文释义，json字符串',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `word_status` int(11) NOT NULL DEFAULT '0' COMMENT '单词状态',
  `op_status` int(11) NOT NULL DEFAULT '0' COMMENT '0表示是逻辑存在的，1表示被逻辑删除了',
  `types` int(11) DEFAULT '1' COMMENT '1是收藏接口上传的单词，2是标星上传的单词',
  PRIMARY KEY (`id`),
  UNIQUE KEY `word_unique` (`word_list_book_id`,`words`),
  CONSTRAINT `word_wordbook` FOREIGN KEY (`word_list_book_id`) REFERENCES `mywordlistbook` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COMMENT='词单单词表';

insert into mywordlists(word_list_book_id, words, word_origin,word_origin_type, word_context, cn_explains, create_time,word_status,op_status,types) 
select word_list_book_id, words, word_origin,word_origin_type, word_context, cn_explains, create_time,word_status,op_status,types from mywordlists_old
ON DUPLICATE KEY UPDATE 
word_origin=VALUES(word_origin),
word_origin_type=VALUES(word_origin_type),
word_context=VALUES(word_context),
cn_explains=VALUES(cn_explains),
create_time=VALUES(create_time),
word_status=VALUES(word_status),
op_status=VALUES(op_status),
types=VALUES(types)
          
alter table $tablename modify `word` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NOT NULL COMMENT '单词拼写, 区分大小写';


 update mywordlistbook w1 
INNER  JOIN
(select mywordlistbook.id,count(mywordlists.words) as word_sizes from mywordlistbook LEFT JOIN mywordlists on mywordlistbook.id=mywordlists.word_list_book_id where mywordlistbook.op_status=0 and ((mywordlists.op_status=0 and mywordlists.word_origin>0) or(mywordlists.words is NULL)) GROUP BY mywordlistbook.id) as b
on w1.id=b.id
set w1.word_sizes=b.word_sizes

ALTER TABLE mywordlists DROP FOREIGN KEY `word_wordbook`;       

### INSERT INTO…ON DUPLICATE KEY UPDATE
>1，insert的数据会引起唯一索引（包括主键索引）的冲突，即这个唯一值重复了，则不会执行insert操作，而执行后面的update操作  
>2，insert affect rows=1，update affect rows=2   





-- insert into version_english(id,os,channel_number,new_version_code,min_version_code,bad_version_code,apk_url,update_description,create_time,update_time) VALUES(1,1,1,16,2,'','https://word.ihumand.com/dev.html','test','2019-12-17 17:53:22','2019-12-17 17:53:22');

 alter table version_english modify column id int not null auto_increment;