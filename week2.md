
# 作业
我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

## 理解
区别不同情况不同对待
- 如果是必须返回一个值。比如验证用户类，比如根据id修改单个用户信息，可以直接向上抛出这个异常
```
 
func  ReadUser(id string) (*CurrentUser, error) {
    var name == nil
	if  id == emptyString {
		return nil, errors.New("empty token string")
	}
	err := reader.QueryRow("select name from users where id = ?", id).Scan(&amp;name)
	
	switch {
        case err == sql.ErrNoRows: 
             return nil,errors.New("no user")
        case err != nil:
           	return nil, err
        case err == nil:
            return &CurrentUser{name:name,id:id},nil
    }
}
```


- 如果不是必须返回一个值，例如查询评论列表新闻列表，直接日志记录，返回空对象列表



```
 
func Read( ) ( []string, error) {
	 
	rows, err := db.QueryRow("select remark form marks")
	if err != nil {
	     logger.Warning("query mapping error", err)
		return []string{}
	}
	defer rows.Close()
	ret := make([]string, len(rows))
	for i := 0; i < len(rows); i++ {
		ret[i] = m[i].Region
	}
	return ret
}
```
