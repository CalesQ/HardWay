1. 建表

	create table if not exists 'table_name'{
		'id' int unsigned auto_increment,
		'name' varchar(10) not null,
		'password' varchar(50) not null,
		primary key ('id')
	} engine = InnoDB Default  charset = utf8mb4; 
	
2. 修改列

	alter table 'table_name' add column 'grend' tinyint(1) not null; 