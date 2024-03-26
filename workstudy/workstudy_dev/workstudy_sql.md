# 1、学生表

```sql
-- 学生表
create table if not exists `student`
(
`id` bigint(20) UNSIGNED not null auto_increment primary key,
`student_no` varchar(100) null comment '学生学号，要唯一，不能重复',
`student_name` varchar(50) null comment '学生姓名',
`student_sex` varchar(10) default '男' null comment '学生性别',
`student_start_year` varchar(10) default '2018' null comment '入学年份',
`student_finish_year` varchar(10) default '2022' null comment '毕业年份',
`student_nation` varchar(20) default '汉' null comment '民族',
`student_birthday` datetime null comment '学生生日',
`student_college` varchar(50) default '软件学院' null comment '学院',
`student_major` varchar(50) default '计算机科学与技术' null comment '专业',
`student_type` varchar(20) null comment '学生类型，例如：中专/大专/本科/研究生'
) comment '学生表';


INSERT INTO `student` (`student_no`, `student_name`, `student_sex`, `student_start_year`, `student_finish_year`, `student_nation`, `student_birthday`, `student_college`, `student_major`, `student_type`)  
VALUES  
('20230001', '张三', '男', '2023', '2027', '汉', '2003-05-10 00:00:00', '软件学院', '计算机科学与技术', '本科'),  
('20230002', '李四', '女', '2023', '2027', '汉', '2003-06-15 00:00:00', '软件学院', '软件工程', '本科'),  
('20230003', '王五', '男', '2023', '2027', '回', '2003-07-01 00:00:00', '商学院', '工商管理', '本科'),  
('20230004', '赵六', '女', '2023', '2027', '藏', '2003-08-20 00:00:00', '艺术学院', '艺术设计', '本科'),  
('20230005', '孙七', '男', '2023', '2027', '壮', '2003-09-10 00:00:00', '外国语学院', '英语', '本科'),
('20230006', '周八', '男', '2023', '2027', '汉', '2003-10-15 00:00:00', '信息工程学院', '电子信息工程', '本科'),  
('20230007', '吴九', '女', '2023', '2027', '满', '2003-11-05 00:00:00', '物理学院', '应用物理学', '本科'),  
('20230008', '郑十', '男', '2023', '2027', '苗', '2003-12-01 00:00:00', '法学院', '法学', '本科'),  
('20230009', '陈十一', '女', '2023', '2027', '蒙古', '2003-12-20 00:00:00', '教育学院', '教育学', '本科'),  
('20230010', '冯十二', '男', '2023', '2027', '朝鲜', '2004-01-10 00:00:00', '数学学院', '数学与应用数学', '本科'),  
('20230011', '褚十三', '女', '2023', '2027', '锡伯', '2004-02-15 00:00:00', '环境科学与工程学院', '环境科学', '本科'),  
('20230012', '卫十四', '男', '2023', '2027', '彝', '2004-03-05 00:00:00', '材料科学与工程学院', '材料科学与工程', '本科'),  
('20230013', '蒋十五', '女', '2023', '2027', '土', '2004-04-20 00:00:00', '化学学院', '化学', '本科'),  
('20230014', '沈十六', '男', '2023', '2027', '仡佬', '2004-05-10 00:00:00', '医学院', '临床医学', '本科'),  
('20230015', '韩十七', '女', '2023', '2027', '东乡', '2004-06-15 00:00:00', '生命科学学院', '生物科学', '本科'),  
('20230016', '杨十八', '男', '2023', '2027', '侗', '2004-07-01 00:00:00', '地理科学学院', '地理科学', '本科'),  
('20230017', '朱十九', '女', '2023', '2027', '达斡尔', '2004-08-10 00:00:00', '建筑工程学院', '土木工程', '本科'),  
('20230018', '秦二十', '男', '2023', '2027', '布朗', '2004-09-05 00:00:00', '机械工程学院', '机械设计制造及其自动化', '本科'),  
('20230019', '尤廿一', '女', '2023', '2027', '保安', '2004-10-15 00:00:00', '经济学院', '经济学', '本科');
```

## 1.1、学历id表

```sql
-- 学生学历
create table if not exists `student_type`
(
`id` bigint not null auto_increment comment '主键' primary key,
`type_id` bigint not null comment '学历id',
`student_type` varchar(256) not null comment '学历类型'
) comment '学生学历';


insert into `student_type` (`type_id`, `student_type`) values (1, '本科');
insert into `student_type` (`type_id`, `student_type`) values (1, '博士');
insert into `student_type` (`type_id`, `student_type`) values (5, '高中');
insert into `student_type` (`type_id`, `student_type`) values (5, '本科');
insert into `student_type` (`type_id`, `student_type`) values (6, '本科');
insert into `student_type` (`type_id`, `student_type`) values (5, '初中');
insert into `student_type` (`type_id`, `student_type`) values (5, '小学');
insert into `student_type` (`type_id`, `student_type`) values (3, '博士');
insert into `student_type` (`type_id`, `student_type`) values (6, '博士');
insert into `student_type` (`type_id`, `student_type`) values (3, '专科');
insert into `student_type` (`type_id`, `student_type`) values (5, '初中');
insert into `student_type` (`type_id`, `student_type`) values (1, '初中');
insert into `student_type` (`type_id`, `student_type`) values (4, '专科');
insert into `student_type` (`type_id`, `student_type`) values (3, '研究生');
insert into `student_type` (`type_id`, `student_type`) values (5, '初中');
insert into `student_type` (`type_id`, `student_type`) values (4, '博士');
insert into `student_type` (`type_id`, `student_type`) values (1, '高中');
insert into `student_type` (`type_id`, `student_type`) values (3, '博士');
insert into `student_type` (`type_id`, `student_type`) values (4, '研究生');
insert into `student_type` (`type_id`, `student_type`) values (6, '博士');
```























































