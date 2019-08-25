---
layout: post
title: Ruby Rails Active Record Basics CRUD Update 更新
date: 2017-12-25 00:00:00
author: "王振华"
tags: 
    - Ruby 
    - Rails 
    - Update
---

检索到Active Record 对象后，就可以用update修改其属性，然后将其保存到数据库中。  

主要使用以下几种方法来修改对象的属性，下面说明一下其区别和各自的适用范围：

#### 1. update(id, attributes)
Updates an object (or multiple objects) and saves it to the database, if validations pass. The resulting object is returned whether the object was saved successfully to the database or not.
可用于修改一个或多个对象的属性，通过数据验证后，就会将其保存到数据库中。无论是否成功保存到数据库中，该方法都会返回结果对象。**?(这就话我还不懂是什么意思)**

- id – This should be the id or an array of ids to be updated.
- attributes – This should be a hash of attributes to be set on the object, or an array of hashes.

```
#修改一个记录的多个属性
Person.update(11, :user_name => 'Tom', :group => 'expert')

#修改多个记录的属性
people = { 1 => {"first_name" => "David" }, 2 => { "first_name" = "Tom" } }
Person.update(people.keys, people.values)
```

下面的写法可能更常见：
```
user = User.first
user.update(attributes)
user.update!(attributes)  #爆炸方法，验证更严格。如果验证失败，会抛出ActiveRecord::RecordInvalid异常

user = User.find_by(name:'David')
user.update(name:'Frank',age:'27')  #可以一次更新多个属性
```

#### 2. update_all
批量更新多个记录，用类方法update_all；会跳过回调。
```
User.update_all(name:"Jack",age:"27")

User.where(name:'oldname').update_all(name:'newname')  
```

---

#### 3.update_attributes(attributes)

Updates all the attributes from the passed-in Hash and saves the record. **If the object is invalid, the saving will fail and false will be returned**.

```
user = User.first
user.update_attributes(name:"Tom",status:"active")
```

#### update_attribute(attribute,value)

```
user = User.first
user.upadte_attribute(:name, "Peter") #注意，参数必须写成用逗号分开的格式。

user.update_attribute(:name=>"Peter") #这样会报错，`ArgumentError: wrong number of arguments (given 1, expected 2)`
```


---

#### 4. update_columns(attributes) public

Updates the attributes **directly in the database** issuing an UPDATE SQL statement and sets them in the receiver:

`user.update_columns(last_request_at: Time.current)`

This is **the fastest way** to update attributes **because it goes straight to the database**, but take into account that in consequence the regular update procedures are totally bypassed. In particular:
- Validations are skipped. 会忽略验证
- Callbacks are skipped. 不会有回调
- updated_at/updated_on are not updated. 不会记录修改日期

This method raises an `ActiveRecord::ActiveRecordError` when called on** new objects**, or when at least one of the attributes is marked as **readonly**


#### update_column(name, value)

Equivalent to update_columns(name => value).
```
user.update_column(attrubute_name, value) #注意，此方法的参数必须写成这种格式，也就是用逗号分成两个参数。:attribute_name => :value 这种写法是一个参数，会报错ArgumentError: wrong number of arguments (given 1, expected 2)


```
`update_column`和`update_columns`会跳过validation验证，会有`mass assign`安全性问题。所以要谨慎使用`update_column`和`update_columns`

---

#### 5.数字字段可以使用increment和decrement方法

```
user = User.first
user.increment(:comments_count)
user.increment!(:comments_count)

user.decrement!(:comments_count)
user.decrement!(:comments_count)
```


参考资料：

[Rails Guides Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html#update)

[实战圣经 资料更新](https://ihower.tw/rails/activerecord-query-cn.html#sec3)

[Difference between update, update_columns, update_column, update_attributes, assign_attributes](https://cbabhusal.wordpress.com/2014/10/08/different-between-update-update_columns-update_column-update_attributes-assign_attributes/)
