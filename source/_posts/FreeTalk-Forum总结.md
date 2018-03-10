---
title: FreeTalk-Forum总结
date: 2017-12-19 18:26:33
comments: ture
categories:
	- Item
tags:
	- Item  summary
---

- 创建数据库
- 创建存储过程
- sql server日期时间转字符串
- 新加一个列（表已存在）
- java调用存储过程

<!-- more -->

------------------------------------------


## 创建数据库

```
/*创建数据库*/
create database FreeTalkDB 
on
(
name=FreeTalkDB_dat,
filename='F:\FreeTalk\FreeTalkDB.mdf',
size=5MB,
maxsize=50MB,
filegrowth=5%
)
log on
(
name=FreeTalkBD_log,
filename='F:\FreeTalk\FreeTalkDB.ldf',
size=3MB,
maxsize=20MB,
filegrowth=3MB
)
```

## 创建存储过程


```

/*创建用户表*/

create table Fuser (
UId  char(10) not null,                --用户ID [1712180001]   pk
UserName  varchar(25) not null,        --用户名
PassWord varchar(16) not null,         --密码
U_Sex int not null,                    --性别
U_Head varchar(50) not null,           --头像地址
U_Grand int not  null                  --权限等级
);



/*创建存储过程
  * 实现ID字段的自动增长并实现insert功能
  * */
 --创建UID_increment
 create proc UID_increment
 @uName varchar(25),
 @passWord varchar(16),
 @uSex int,
 @uHead varchar(50),
 @uGrand int
 as
 begin
 	declare @date char(6),@id char(4),@uID char(10)
 	select @date=right(convert(varchar(8),getdate(),112),6);
 	select @id=MAX(UID) from Fuser
 if @id IS null
 begin
 	set @id ='0001';
 end
 else
 	begin
 		select @id=(convert(int,RIGHT(UID,4))+1) from Fuser;
 		if @id <10
 			set @id = '000'+(convert(char,@id));
 		else if @id < 100 and @id >= 10
 			set @id = '00'+(convert(char,@id));
 		else if @id < 1000 and @id >= 100
 			set @id = '0'+(convert(char,@id));
	end
 		select @uID=@date+@id;
 	insert into Fuser values (@UID,@uName,@passWord,@uSex,@uHead,@uGrand);
 end;
	
/*调用存储过程*/
exec UId_increment 'adf','123',1,'sdfs',1;
	
/*查询结果*/
select * from Fuser;
```

## sql server日期时间转字符串

[sql server日期时间转字符串](https://www.cnblogs.com/zhangq723/archive/2011/02/16/1956152.html)


## 新加一个列（表已存在）

```

/*版块表(Section)增加一个列(S_notify)*/

alter table Section add S_notify varchar(256);

```

## java调用存储过程

```

public boolean addUser(User user) {
		Connection conn = DBConnection.getConnection();
		try {
			CallableStatement cs = conn.prepareCall("{call UId_increment（?,?,?,?,?）}");
			
			cs.setString(1, user.getUserName());
			cs.setString(2, user.getPassWord());
			cs.setInt(3, user.getSex());
			cs.setString(4, user.getuHead());
			cs.setInt(5,user.getGrand());
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			return false;
			
		} finally {
			try {
				conn.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}  
		}
		// TODO Auto-generated method stub
		return true;
	}

```


## 通过集合获取一个对象
```

public List<Section> getSections() {
		// TODO Auto-generated method stub
		Connection conn = DBConnection.getConnection();
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		List<Section> list = new ArrayList<Section>();
		String sql = "select * from Section order by ? asc";
		try {
			Section s = new Section();
			pstmt = conn.prepareStatement(sql);
			pstmt.setInt(1,s.getsId());
			rs = pstmt.executeQuery();
			while(rs.next()) {
				s.setsId((rs.getInt(1))); 
				s.setsName(rs.getString(2));
				s.setsNotice(rs.getString(3));
				User u1 = new User();
				u1.setuId(rs.getString(4));
				s.setModerator1(u1);
				User u2 = new User();
				u2.setuId(rs.getString(5));
				s.setModerator1(u2);
				User u3 = new User();
				u3.setuId(rs.getString(6));
				s.setModerator1(u3);
				s.setsClickCount(rs.getInt(7));
				
				list.add(s);
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			return null;
		}
		return list;
	}

```

## java中的几个集合类的详解

[java中的几个集合类的详解](https://www.cnblogs.com/azai/archive/2010/12/09/1901272.html)

--------------------------------------------------------------

作者：梦不若星辰
链接：https://aqqje.github.io
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。