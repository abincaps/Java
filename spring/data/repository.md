# Repository

## Repository\<T, ID>接口

Spring Data repository 抽象的中心接口是 `Repository<T, ID>`

* `T` 实体类类型
* `ID` 实体类的主键类型

## CrudRepository\<T, ID>

* `Iterable findAll(Sort sort)`  返回按给定选项排序的所有实体
* `Sort.by(Sort.Direction.DESC, "id")` 根据id降序



