# python中操作Mysql
## pymsql 
    
pymsql模块是python中操作MySQL的模块，使用方法与MySQL几乎相同。

### 1 执行SQL

    #-*- coding:utf-8 -*-
    import pymysql
    #创建连接
    conn = pymysql.connect(host='127.0.0.1',port=3306,user='root',passwd='111111',db='db1')
    #创建游标
    cursor = conn.cursor()

    #执行SQL,并返回受影响的行数
    effect_row = cursor.execute('update hosts set host='1.1.1.2')

    #执行SQL，并返回受影响的行数
    #effect_row = cursor.executemany("insert into hosts(host,color_id)values(%s,%s)",[("1.2.1.11",1),("1.1.1.11",2)])

    #提交，否则无法保存新建的或者修改的数据
    conn.commit()

    #关闭游标
    cursor.close()
    #关闭连接
    conn.close()

### 2 获取新创建数据自增ID

    #-*- coding:utf-8 -*-
    import pymysql
    conn = pymysql.connect(host='127.0.0.1',port=3306,user='root',passwd='111111',db='db1')
    cursor = conn.cursor()
    cursor.executemany("insert into hosts(host,color_id)values(%s,%s)",[("1.2.1.11",1),("1.1.1.11",2)])
    conn.commit()
    curcor.close()
    conn.close()

    #获取最新的自增ID
    new_id = cursor.lastrowid

### 3 获取查询到的数据

    #-*- coding:utf-8 -*-
    import pymysql
    conn = pymysql.connect(host='127.0.0.1',port=3306,user='root',passwd='111111',db='db1')
    cursor = conn.cursor()
    cursor.execute("select * from t1")

    #获取第一行数据
    row_1 = cuesor.fetchone()

    #获取前10行数据
    row_2 = cursor.fetchmany(10)

    #获取查询到的所有数据
    row_3 = cursor.fetchall()

    conn.commit()
    cursor.close()
    conn.close()

注：在fetch数据时按照顺序进行，可以使用cursor.scroll(num,mode)来移动游标位置，如：  

cursor.scroll(1,mode='relative')  # 相对当前位置移动  
cursor.scroll(2,mode='absolute') # 相对绝对位置移动  

### 4 fetch的数据类型
默认是元组的数据类型，如果想得到字典的数据类型的数据，即：

    #-*- coding:utf-8 -*-
    import pymysql
    conn = pymysql.connect(host='127.0.0.1',port=3306,user='root',passwd='111111',db='db1')
    #游标设置为字典类型
    cursor = conn.cursor(cursor=pymysql.sursor.DictCursor)
    r = cursor.execute("call p1()")

    result = cursor.fetchone()

    conn.commit()
    cursor.close()
    conn.close()

## SQLALchemy
SQLAlchemy是Python编程语言下的一款ORM框架，该框架建立在数据库API之上，使用关系对象映射进行数据库操作，简言之便是：将对象转换成SQL，然后使用数据API执行SQL并获取执行结果

SQLAlchemy本身无法操作数据库，其必须以来pymsql等第三方插件，Dialect用于和数据API进行交流，根据配置文件的不同调用不同的数据库API，从而实现对数据库的操作，如：

    MySQL-Python
    mysql+mysqldb://<user>:<password>@<host>[:<port>]/<dbname>
   
    pymysql
        mysql+pymysql://<username>:<password>@<host>/<dbname>[?<options>]
       
    MySQL-Connector
        mysql+mysqlconnector://<user>:<password>@<host>[:<port>]/<dbname>
       
    cx_Oracle
        oracle+cx_oracle://user:pass@host:port/dbname[?key=value&key=value...]
       
    更多详见：http://docs.sqlalchemy.org/en/latest/dialects/index.html

### ORM 功能使用
使用 ORM/Schema Type/SQL Expression Language/Engine/ConnectionPooling/Dialect 所有组件对数据进行操作。根据类创建对象，对象转换成SQL，执行SQL。

#### 1 创建表

    -*- coding:utf-8 -*-
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy import Column,Integer,String,CHAR,VARCHAR,ForreighnKey,UniquConstraint,Index
    from sqlalchemy.orm import sessionmaker,relationship
    from sqlalchemy import create_engine

    engine = create_engine("mysql_pymysql://root:111111@127.0.0.1:3306/t1",max_overflow = 5)

    Base = declarative_base()
    
    #创建表
    class Users(Base):
        __tablename__ = 'users'
        id = Column(Integer,primary_key=True)
        name = Column(String(32))
        extra = Column(String(16))

        __table_args__=(
        UniqueConstraint('id','name',name='uix_id_name'),
            Index('ix_id_name','name','extra'),
        )

    #一对多
    class Favor(Base):
        __tablename__ = 'favor'
        nid = Column(Integer,primary_key=True)
        caption = Column(String(50),default='red',unique=True)

    class Person(Base):
        __tablename__='person'
        id=Column(Integer,primary_key=True)
        name = Column(String(32), index=True, nullable=True)
        favor_if = Column(Integer,ForeignKey('favor.nid'))

    #多对多
    class Group(Base):
        __tablename__ = 'group'
        id = Column(Integer,primary_key=True)
        name = Column(String(64),unique=True,nullable=True)
        port = Column(Integer,default=22)

    class Server(Base):
        __tablename__ = 'server'
        id = Column(Integer, primary_key=True, autoincrement=True)
        hostname = Column(String(64), unique=True, nullable=False)

    class ServerToGroup(Base):
        __tablename__ = 'servertogroup'
        nid = Column(Integer, primary_key=True, autoincrement=True)
        server_id = Column(Integer, ForeignKey('server.id'))
        group_id = Column(Integer, ForeignKey('group.id'))

    def init_db():
        Base.metdata.create_all(engine)
    def drop_db():
        Base_metdata.drop_all(engine)


#### 操作表
    
    # -*- coding:utf-8 -*-
    from sqlalchemy.ext.declarative import declarative_base
    from sqlalchemy import Column, Integer, String, ForeignKey, UniqueConstraint, Index
    from sqlalchemy.orm import sessionmaker, relationship
    from sqlalchemy import create_engine

    engine = create_engine("mysql+pymysql://root:123@127.0.0.1:3306/t1", max_overflow=5)

    Base = declarative_base()

    # 创建单表
    class Users(Base):
        __tablename__ = 'users'
        id = Column(Integer, primary_key=True)
        name = Column(String(32))
        extra = Column(String(16))

        __table_args__ = (
        UniqueConstraint('id', 'name', name='uix_id_name'),
            Index('ix_id_name', 'name', 'extra'),
        )

        def __repr__(self):
            return "%s-%s" %(self.id, self.name)

    # 一对多
    class Favor(Base):
        __tablename__ = 'favor'
        nid = Column(Integer, primary_key=True)
        caption = Column(String(50), default='red', unique=True)

        def __repr__(self):
            return "%s-%s" %(self.nid, self.caption)
    class Person(Base):
        __tablename__ = 'person'
        nid = Column(Integer, primary_key=True)
        name = Column(String(32), index=True, nullable=True)
        favor_id = Column(Integer, ForeignKey("favor.nid"))
        # 与生成表结构无关，仅用于查询方便
        favor = relationship("Favor", backref='pers')

    # 多对多
    class ServerToGroup(Base):
        __tablename__ = 'servertogroup'
        nid = Column(Integer, primary_key=True, autoincrement=True)
        server_id = Column(Integer, ForeignKey('server.id'))
        group_id = Column(Integer, ForeignKey('group.id'))
        group = relationship("Group", backref='s2g')
        server = relationship("Server", backref='s2g')

    class Group(Base):
        __tablename__ = 'group'
        id = Column(Integer, primary_key=True)
        name = Column(String(64), unique=True, nullable=False)
        port = Column(Integer, default=22)
        # group = relationship('Group',secondary=ServerToGroup,backref='host_list')


    class Server(Base):
        __tablename__ = 'server'

        id = Column(Integer, primary_key=True, autoincrement=True)
        hostname = Column(String(64), unique=True, nullable=False)


    def init_db():
        Base.metadata.create_all(engine)
    def drop_db():
        Base.metadata.drop_all(engine)

    Session = sessionmaker(bind=engine)
    session=Session()
##### 增
    
    obj = Users(name='agg',extra='assdd')
    session.add(obj)
    session.add_all([
        Users(name='ddddd',extra='wee'),
        Users(name='dsads',extra='sdsfdf'),
    ])

    session.commit()

##### 删

    session.query(Users).filter(Users.id>2).delete()
    session.commit()

##### 改

    session.query(Users).filter(Users.id > 2).update({"name" : "099"})
    session.query(Users).filter(Users.id > 2).update({Users.name: Users.name + "099"}, synchronize_session=False)
    session.query(Users).filter(Users.id > 2).update({"num": Users.num + 1}, synchronize_session="evaluate")
    session.commit()

##### 查

    ret = session.query(Users).all()
    ret = session.query(Users.name, Users.extra).all()
    ret = session.query(Users).filter_by(name='alex').all()
    ret = session.query(Users).filter_by(name='alex').first()

    ret = session.query(Users).filter(text("id<:value and name=:name")).params(value=224, name='fred').order_by(User.id).all()

    ret = session.query(Users).from_statement(text("SELECT * FROM users where name=:name")).params(name='ed').all()

##### 其他


    #　条件
    ret = session.query(Users).filter_by(name='alex').all()
    ret = session.query(Users).filter(Users.id > 1, Users.name == 'eric').all()
    ret = session.query(Users).filter(Users.id.between(1, 3), Users.name == 'eric').all()
    ret = session.query(Users).filter(Users.id.in_([1,3,4])).all()
    ret = session.query(Users).filter(~Users.id.in_([1,3,4])).all()
    ret = session.query(Users).filter(Users.id.in_(session.query(Users.id).filter_by(name='eric'))).all()
    from sqlalchemy import and_, or_
    ret = session.query(Users).filter(and_(Users.id > 3, Users.name == 'eric')).all()
    ret = session.query(Users).filter(or_(Users.id < 2, Users.name == 'eric')).all()
    ret = session.query(Users).filter(
        or_(
            Users.id < 2,
            and_(Users.name == 'eric', Users.id > 3),
            Users.extra != ""
        )).all()


    # 通配符
    ret = session.query(Users).filter(Users.name.like('e%')).all()
    ret = session.query(Users).filter(~Users.name.like('e%')).all()

    # 限制
    ret = session.query(Users)[1:2]

    # 排序
    ret = session.query(Users).order_by(Users.name.desc()).all()
    ret = session.query(Users).order_by(Users.name.desc(), Users.id.asc()).all()

    # 分组
    from sqlalchemy.sql import func

    ret = session.query(Users).group_by(Users.extra).all()
    ret = session.query(
        func.max(Users.id),
        func.sum(Users.id),
        func.min(Users.id)).group_by(Users.name).all()

    ret = session.query(
        func.max(Users.id),
        func.sum(Users.id),
        func.min(Users.id)).group_by(Users.name).having(func.min(Users.id) >2).all()

    # 连表

    ret = session.query(Users, Favor).filter(Users.id == Favor.nid).all()

    ret = session.query(Person).join(Favor).all()

    ret = session.query(Person).join(Favor, isouter=True).all()


    # 组合
    q1 = session.query(Users.name).filter(Users.id > 2)
    q2 = session.query(Favor.caption).filter(Favor.nid < 2)
    ret = q1.union(q2).all()

    q1 = session.query(Users.name).filter(Users.id > 2)
    q2 = session.query(Favor.caption).filter(Favor.nid < 2)
    ret = q1.union_all(q2).all()
