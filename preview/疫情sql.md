```mysql
ALTER TABLE `t_photo` 
ADD COLUMN `identify_result` tinyint(2) UNSIGNED NULL DEFAULT 1 COMMENT '是否识别正确' AFTER `is_del`;
```







```sql
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('admin', '疫情管理', 0, 0, 1, 'OutbreakController@getIndex', '', 0, 1, 0, '', '2020-02-09 21:02:35', '2020-02-09 21:02:35');
INSERT INTO `t_admin_auth`( `auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('admin', '申请列表', 111, 0, 2, 'OutbreakController@getIndex', '', 0, 1, 0, '', '2020-02-09 21:03:31', '2020-02-09 21:03:31');

ALTER TABLE `t_outbreak` 
ADD COLUMN `is_del` tinyint(2) UNSIGNED NULL DEFAULT 0 COMMENT '是否删除' AFTER `is_hubei`;

ALTER TABLE `t_outbreak` 
ADD COLUMN `openid` varchar(200) NULL DEFAULT NULL COMMENT 'wx openid' AFTER `id`;

ALTER TABLE `t_outbreak` 
ADD COLUMN `house_type` tinyint(2) UNSIGNED NULL DEFAULT NULL COMMENT '房屋类型：1=自购房屋；2=租住房屋；3=企业宿舍；' AFTER `community`;

ALTER TABLE `t_outbreak` 
ADD COLUMN `option_2_time` datetime(0) NULL DEFAULT NULL COMMENT '隔离时间' AFTER `option_2`;


ALTER TABLE `t_police_station` 
ADD COLUMN `sort` tinyint(3) UNSIGNED NULL COMMENT '排序 desc' AFTER `remark`;



ALTER TABLE `t_outbreak` 
ADD COLUMN `form_type` enum('outbreak','residents') NULL DEFAULT 'outbreak' COMMENT '表单类型：outbreak=防疫；residents=居民' AFTER `id`,
ADD COLUMN `c_option_1` tinyint(2) UNSIGNED NULL COMMENT '近期是否在江津: 1=在江津2个月以上（或一直在重庆）;2=来江津超过14天不超过2个月;3=来或返回江津不超过14日（含14日）;4=目前仍在外地' AFTER `option_3`,
ADD COLUMN `c_option_2` tinyint(2) UNSIGNED NULL COMMENT '是否有新冠肺炎疑似接触情况: 1=途径武汉;2=途径湖北（除武汉外）;3=确诊病例密切接触者;3=疑似病例密切接触者;4=无NCP疑似接触史;' AFTER `c_option_1`,
ADD COLUMN `c_option_3` tinyint(2) UNSIGNED NULL COMMENT '近期有无隔离观察情况: 1=正在江津隔离观察; 2=已在江津隔离观察14日;3=近期无隔离观察室（30日内）;' AFTER `c_option_2`,
ADD COLUMN `c_option_4` tinyint(2) UNSIGNED NULL COMMENT '是否为新冠肺炎确诊病人: 1=是; 2=否; 3=已治愈;' AFTER `c_option_3`;

CREATE TABLE `t_outbreak_clock` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `outbreak_id` int(10) NOT NULL COMMENT '申报id',
  `option_1` char(3) DEFAULT NULL COMMENT '症状：1=发热；2=乏力；3=咳嗽；4=呼吸急促；5=无症状；',
  `option_2` tinyint(2) unsigned DEFAULT NULL COMMENT '是否就诊: 1=是；2=否；',
  `option_3` tinyint(2) unsigned DEFAULT NULL COMMENT '家人有无相同症状: 1=是；2=否；',
  `clock_date` date DEFAULT NULL COMMENT '打卡时间',
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_outbreak_id` (`outbreak_id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


ALTER TABLE `t_outbreak` 
ADD COLUMN `is_dispose` tinyint(2) UNSIGNED NULL DEFAULT 0 COMMENT '是否处理：0=否；1=是；' AFTER `is_del`;
ALTER TABLE `t_outbreak` 
ADD INDEX `idx_form_type`(`form_type`) USING BTREE;
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('police', '居民申报', 113, 1, 2, 'OutbreakResidentsController@getIndex', '', 0, 1, 0, '', '2020-02-14 13:33:03', '2020-02-14 13:33:03');
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('admin', '居民申报', 111, 0, 2, 'OutbreakResidentsController@getIndex', '', 0, 1, 0, '', '2020-02-14 13:10:50', '2020-02-14 13:10:50');



ALTER TABLE `t_outbreak_clock` 
ADD COLUMN `message` text NULL COMMENT '留言' AFTER `clock_date`;
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('police', '每日打卡', 113, 0, 2, 'OutbreakClockController@getIndex', '', 0, 1, 0, '', '2020-02-15 15:01:08', '2020-02-15 15:01:08');
INSERT INTO `t_admin_auth`( `auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ( 'admin', '每日打卡', 111, 0, 2, 'OutbreakClockController@getIndex', '', 0, 1, 0, '', '2020-02-15 14:48:44', '2020-02-15 14:48:44');




CREATE TABLE `t_outbreak_door` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `openid` char(64) DEFAULT NULL COMMENT 'wx openid',
  `place_name` varchar(255) DEFAULT NULL COMMENT '场所名称',
  `person_name` varchar(50) DEFAULT NULL COMMENT '姓名',
  `person_phone` varchar(20) DEFAULT NULL COMMENT '电话',
  `location` varchar(100) DEFAULT NULL COMMENT '位置id(,分割)',
  `location_address` varchar(255) DEFAULT NULL COMMENT '详情地址',
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
CREATE TABLE `t_outbreak_door_record` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `outbreak_id` int(10) unsigned NOT NULL COMMENT '申报id',
  `outbreak_door_id` int(10) unsigned NOT NULL COMMENT '门id',
  `scan_time` datetime DEFAULT NULL COMMENT '扫码时间',
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_outbreak_id` (`outbreak_id`) USING BTREE,
  KEY `idx_outbreak_door_id_scan_time` (`outbreak_door_id`,`scan_time`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
ALTER TABLE `t_outbreak_door` 
ADD COLUMN `police_station_id` int(10) UNSIGNED NULL COMMENT '派出所id' AFTER `openid`,
ADD COLUMN `community_id` int(10) UNSIGNED NULL COMMENT '社区id' AFTER `police_station_id`;
ALTER TABLE `t_outbreak_door` 
ADD COLUMN `is_del` tinyint(2) UNSIGNED NULL DEFAULT 0 COMMENT '是否删除： 0=否；1=是' AFTER `location_address`;

INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('police', '门二维码列表', 113, 0, 2, 'OutbreakDoorController@getIndex', '', 0, 1, 0, '', '2020-02-16 20:06:55', '2020-02-16 20:06:55');
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('admin', '门二维码列表', 111, 0, 2, 'OutbreakDoorController@getIndex', '', 0, 1, 0, '', '2020-02-16 17:00:15', '2020-02-16 17:00:15');



ALTER TABLE `t_outbreak_clock` 
ADD COLUMN `police_station_id` int(10) UNSIGNED NULL COMMENT '派出所id' AFTER `outbreak_id`,
ADD INDEX `idx_police_station_id`(`police_station_id`) USING BTREE;
UPDATE t_outbreak_clock AS oc
INNER JOIN ( SELECT * FROM t_outbreak ) o ON o.id = oc.outbreak_id 
SET oc.police_station_id = o.police_station_id

ALTER TABLE `t_outbreak_clock` 
ADD COLUMN `community_id` int(10) UNSIGNED NULL DEFAULT NULL COMMENT '社区id' AFTER `police_station_id`,
ADD INDEX `idx_community_id`(`community_id`) USING BTREE;
UPDATE t_outbreak_clock AS oc
INNER JOIN ( SELECT * FROM t_outbreak ) o ON o.id = oc.outbreak_id 
SET oc.community_id = o.community_id;




ALTER TABLE `t_outbreak_door` 
ADD COLUMN `status` tinyint(3) UNSIGNED NULL DEFAULT 10 COMMENT '状态：10=已审核；20=未审核' AFTER `location_address`;


CREATE TABLE `t_outbreak_ride` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `company_name` varchar(255) DEFAULT NULL COMMENT '公司名称',
  `license_plate` varchar(30) DEFAULT NULL COMMENT '车牌',
  `driver_name` varchar(200) DEFAULT NULL COMMENT '司机姓名多个&连接',
  `phone` varchar(20) DEFAULT NULL COMMENT '电话',
  `qrcode_url` varchar(255) DEFAULT NULL COMMENT '二维码链接',
  `is_del` tinyint(2) unsigned DEFAULT '0' COMMENT '是否删除：0=否；1=是',
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='车辆信息';
CREATE TABLE `t_outbreak_ride_member` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `openid` char(64) NOT NULL COMMENT 'wx openid',
  `name` varchar(45) NOT NULL COMMENT '乘车人姓名',
  `phone` varchar(20) NOT NULL COMMENT '乘车人电话',
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_openid` (`openid`) USING BTREE,
  KEY `idx_phone` (`phone`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='乘车用户表';
CREATE TABLE `t_outbreak_ride_record` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `ride_id` int(10) NOT NULL COMMENT '车辆信息id',
  `ride_member_id` int(10) unsigned DEFAULT NULL COMMENT '乘车用户表id',
  `name` varchar(45) DEFAULT NULL COMMENT '乘车人姓名',
  `phone` varchar(20) DEFAULT NULL COMMENT '乘车人电话',
  `scan_time` datetime DEFAULT NULL COMMENT '扫码时间',
  `created_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_ride_id` (`ride_id`) USING BTREE,
  KEY `idx_ride_member_id` (`ride_member_id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='乘车记录';
INSERT INTO `t_admin_auth`( `auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('police', '乘车码-车辆管理', 113, 0, 2, 'OutbreakRideController@getIndex', '', 0, 1, 0, '', '2020-02-20 16:09:44', '2020-02-20 16:09:44');
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('admin', '乘车码-车辆管理', 111, 0, 2, 'OutbreakRideController@getIndex', '', 0, 1, 0, '', '2020-02-20 16:02:23', '2020-02-20 16:02:23');



```



```sql
清理数据sql
UPDATE t_outbreak SET operator_is_update = 1  WHERE operator_is_meet_condition = 20
truncate t_openid_record;
update t_outbreak set openid_record_id = NULL;
UPDATE t_outbreak_ride_member set openid_record_id = NULL;
```



```sql
sql 记录
update t_outbreak set type ='other' where id in (SELECT id FROM 
(SELECT id,openid,id_card FROM t_outbreak where type = 'self' group by openid order by id desc) t2 group by t2.id_card HAVING count(*) > 1);
```



```sql
待执行
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('police', '辅警岗位列表', 38, 1, 2, 'AuxiliaryPolice\\
positionController@getIndex', '', 0, 1, 0, '', '2020-03-06 18:42:35', '2020-03-06 18:42:35');
INSERT INTO `t_admin_auth`(`auth`, `name`, `parent_id`, `type`, `level`, `url`, `params`, `sort`, `enabled`, `is_active`, `fa_class`, `created_at`, `updated_at`) VALUES ('police', '辅警申请列表', 38, 1, 2, 'AuxiliaryPolice\\IndexController@getIndex', '', 0, 1, 0, '', '2020-03-06 17:11:16', '2020-03-06 17:11:16');


    "id" => 640992
    "police_station_id" => 20
    "guardian" => "[{"name":"王仁波","gender":"1","census_register":"蔡家镇鱼市街10000号","phone":"18996264085","card_photo":"","operator_is_update":"","operator_is_meet_condition":"","operator_query_at":"","operator_query_count":"","operator_query_total":""}]"
  ]
```




