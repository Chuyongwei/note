```powershell
pip install Flask
```

## 创建网站

```python
from flask import Flask,request,jsonify

app = Flask(__name__)


@app.route('/index')
# http://127.0.0.1:8080/index?pwd=654321&age=19
def index():
    age = request.args.get("age")
    pwd = request.args.get("pwd")
    return '<h1>This is '+pwd +'</h1>'

# 接受方式
@app.route('/home',methods=['GET','POST'])
def home():
    xx = request.form.get("xx")
    yy = request.form.get("yy")
    print(xx,yy)
    print(request.json,type(request.json))
    return {
        "status": "ok",
        "data":"asadfgfdh"
    }

@app.route('/login',methods=['GET','POST'])
def  login():

    ordered_string = request.form.get("ordered_string")
    if not ordered_string:
        return jsonify({"status":False,"error":"参数错误"})

    encrypted_string = ordered_string+"832091480909802840"
    obj = hashlib.md5(encrypted_string.encode('utf-8'))
    sign = obj.hexdigest()
    return jsonify({"status":True,"data":sign})


    app.run(host="127.0.0.1", port=8080)

```

## 连接数据库

```powershell
pip isntall hashlib
pip install pymysql
```





```python
# 但连接池
def fetch_one(sql,params):
    conn = pymysql.connect(host='localhost',port = 3306,user='root',passwd='654321',db='test')
    cursor = conn.cursor()
    cursor.execute(sql,params)
    result = cursor.fetchone()
    cursor.close()
    conn.close()
    return result

## 配置连接池
POOL = PooledDB(
    creator = pymysql,
    maxconnections=10,
    mincached=2,
    maxcached=2,
    maxshare3=2,
    blocking=True,
    maxusage=None,
    setsession=set(),
    setresult=set(),
    autocommit=False,
    ping=0,
    host='localhost',port = 3306,user='root',passwd='654321',db='test',charset='utf8'
)

def fetch_all(sql,params):
    conn = POOL.connection()
    cursor = conn.cursor()
    cursor.execute(sql,params)
    result = cursor.fetchone()
    cursor.close()
    conn.close() ## 不是关闭连接，将此链接交还给连接池
    return result

def test_one():
    token = 1
    result = fetch_one('select * from users where id=%s',[token])
    if not result:
        return '认证失败'
    else:
        return result
```

