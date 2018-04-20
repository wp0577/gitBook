## 1.内连接查询

显示内连接：

select \* from category c, product p where c.cid = p.category\_id;

or

隐式内连接：

select \* from category inner join product on cid = category\_id;

## 2.外链接查询

左连接：**（左右连接的区别在于，当主表和副表中有未对应的数据时，如cid=004而category\_id没有004时，左连接会将主表内容全部显示而省略未reference的副表内容。反之亦然。）**

---

select \* from category left join product on cid = category\_id;

\(都是主表在前，副表在后\)

右连接：

select \* from category right join product on cid = category\_id;

![](/mysql2/import.png)

