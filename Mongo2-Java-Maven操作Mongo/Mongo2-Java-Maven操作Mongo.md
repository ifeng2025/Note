## 准备Mongo数据库

[Docker连编部署mongo yml部署](https://blog.csdn.net/weixin_39381833/article/details/88245260)

## 创建Mongo数据库



```
use skedu
```

## IDEA创建Maven项目

```
new project ------> Maven ------->Create from archetype 
```

```
//选择自动导入
org.apache.maven.archetypes:maven-archetype-quickstart
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190315170358172.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTM4MTgzMw==,size_16,color_FFFFFF,t_70)

## 编辑pol.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>mongodb</groupId>
    <artifactId>mongodb</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.mongodb</groupId>
            <artifactId>mongo-java-driver</artifactId>
            <version>2.10.1</version>
        </dependency>
    </dependencies>


</project>

```

**右下角弹出窗口 选择自动下载更新包**

## App.java

```
package mongotest;

import java.net.UnknownHostException;
import java.util.Date;
import com.mongodb.*;
import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.MongoClient;
import com.mongodb.MongoException;

/**
 * Java + MongoDB Hello world Example
 *
 */
public class App {
    public static void main(String[] args) {

        try {

            /**** Connect to MongoDB ****/
            // Since 2.10.0, uses MongoClient
            MongoClient mongo = new MongoClient("localhost", 16161);

            /**** Get database ****/
            // if database doesn't exists, MongoDB will create it for you
            DB db = mongo.getDB("skedu");

            //DBCollection coll = (DBCollection) db.createCollection("student");

            /**** Get collection / table from 'testdb' ****/
            // if collection doesn't exists, MongoDB will create it for you
            DBCollection table = db.getCollection("book");

            /**
             * db.books.insertMany([
             * {
             * bname:"java",
             * price:20,
             * number:5,
             * status:"在售"
             *
             * },
             * {
             * bname:"oracle",
             * price:30,
             * number:5,
             * status:"缺货"
             * }
             * ])
             * **/
            BasicDBObject document1 = new BasicDBObject();
            document1.put("bname", "java");
            document1.put("price", 20);
            document1.put("number", 5);
            document1.put("status", "缺货");
            table.insert(document1);

            BasicDBObject document2 = new BasicDBObject();
            document2.put("bname", "oracle");
            document2.put("price", 30);
            document2.put("number", 5);
            document2.put("status", "缺货");
            table.insert(document2);

            /**** Insert ****/
            // create a document to store key and value
/*            BasicDBObject document = new BasicDBObject();
            document.put("sname", "mkyong");
            document.put("sage", 18);
            document.put("sex", "man");
            table.insert(document);*/

            /**** Find and display ****/
/*            BasicDBObject searchQuery = new BasicDBObject();
            searchQuery.put("sname", "mkyong");*/

            BasicDBObject searchQuery = new BasicDBObject();
            searchQuery.put("bname", "java");

            DBCursor cursor = table.find(searchQuery);

            while (cursor.hasNext()) {
                System.out.println(cursor.next());
            }
            BasicDBObject searchQuery2 = new BasicDBObject();
            searchQuery.put("bname", "oracle");

            DBCursor cursor2 = table.find(searchQuery);

            while (cursor2.hasNext()) {
                System.out.println(cursor2.next());
            }

            /**** Update ****/
            // search document where name="mkyong" and update it with new values
            BasicDBObject query = new BasicDBObject();
            query.put("bname", "oracle");

            BasicDBObject newDocument = new BasicDBObject();
            newDocument.put("bname", "oracle2");

            BasicDBObject updateObj = new BasicDBObject();
            updateObj.put("$set", newDocument);

            table.update(query, updateObj);




            /**** Find and display ****/
            BasicDBObject searchQuery9
                    = new BasicDBObject().append("bname", "oracle2");

            DBCursor cursor9 = table.find(searchQuery9);

            while (cursor9.hasNext()) {


                System.out.println(cursor9.next());
            }

            /**** Done ****/
            System.out.println("Done");

        } catch (UnknownHostException e) {
            e.printStackTrace();
        } catch (MongoException e) {
            e.printStackTrace();
        }

    }
}
```
![屏幕快照 2019-03-22 下午2.49.01.png](resources/8D78462429FF5499C9835B44B35DA9C7.png =1467x334)


## Navicat检查

![屏幕快照 2019-03-22 下午2.51.02.png](resources/C6D2C011AEA1D9E2A5F597A307AF6F59.png =452x145)