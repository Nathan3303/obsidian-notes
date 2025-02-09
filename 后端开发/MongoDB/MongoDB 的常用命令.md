## MongoDB 命令行交互

### 数据库命令

1. 显示所有数据库
    
    ```
    show dbs
    ```
    
2. 使用（选择）数据库
    
    ```
    use [dbName]
    ```
    
3. 显示当前使用（选择）的数据库
    
    ```
    db
    ```
    
4. 删除当前数据库
    
    ```
    use [dbName]
    db.dropDatabase()
    ```
    

### 集合命令

1. 创建集合
    
    ```
    db.createCollection([collectionName])
    ```
    
2. 显示当前数据库中的所有集合
    
    ```
    show collections
    ```
    
3. 删除某个集合
    
    ```
    db.[collectionName].drop()
    ```
    
4. 重命名集合
    
    ```
    db.[collectionName].renameCollection([newCollectionName])
    ```
    

### 文档命令

1. 插入文档
    
    ```
    db.[collectionName].insert([documentObject])
    ```
    
2. 查询文档
    
    ```
    db.[collectionName].find([findCondition])
    ```
    
3. 更新文档
    
    ```
    // 全量更新（新对象完全代替旧对象）
    db.[collectionName].update([findCondition], [newDocumentObject])
    
    // 匹配更新（新对象匹配代替旧对象）
    db.[collectionName].update([findCondition], {$set:[newDocumentObject]})
    ```
    
4. 删除文档
    
    ```
    db.[collectionName].remove([findCondition])
    ```