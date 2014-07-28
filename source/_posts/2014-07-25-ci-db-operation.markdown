---
layout: post
title: "CI 常用 DB 操作"
date: 2014-07-25 15:59:40 +0800
comments: true
categories: php
---
CI....蛋疼菊紧

<!--more-->

```php
/*
 ==================================
 查询
 $query = $this->db_query("SELECT * FROM table");
 ==================================
*/
 
//result() 返回对象数组
$data = $query->result();
 
//result_array() 返回数据 
$data = $query->result_array();
 
//row() 只返回一行对象数组
$data = $query->row();
 
//num_rows() 返回查询结果行数
$data = $query->num_rows();
 
//num_fields() 返回查询请求的字段个数
$data = $query->num_fields();
 
//row_array() 只返回一行数组
$data = $query->row_array();
 
//free_result() 释放当前查询所占用的内存并删除关联资源标识
$data = $query->free_result();
 
/*
 ==================================
 插入操作
 ==================================
*/
 
//上次插入操作生成的ID
echo $this->db->insert_id();
 
//写入和更新操作被影响的行数
echo $this->db->affected_rows();
 
//返回指定表的总行数
echo $this->db->count_all('table_name');
 
//输出当前的数据库版本号
echo $this->db->version();
 
//输出当前的数据库平台
echo $this->db->platform();
 
//返回最后运行的查询语句
echo $this->db->last_query();
 
//插入数据，被插入的数据会被自动转换和过滤，例如：
//$data = array('name' => $name, 'email' => $email, 'url' => $url);
$this->db->insert_string('table_name', $data);
 
/*
 ==================================
 更新操作
 ==================================
*/
 
//更新数据，被更新的数据会被自动转换和过滤，例如：
//$data = array('name' => $name, 'email' => $email, 'url' => $url);
//$where = "author_id = 1 AND status = 'active'";
$this->db->update_string('table_name', $data, $where);
 
/*
 ==================================
 选择数据
 ==================================
*/
 
//获取表的全部数据
$this->db->get('table_name');
 
//第二个参数为输出条数，第三个参数为开始位置
$this->db->get('table_name', 10, 20);
 
//获取数据，第一个参数为表名，第二个为获取条件，第三个为条数
$this->db->get_where('table_name', array('id'=>$id), $offset);
 
//select方式获取数据
$this->db->select('title, content, date');
$data = $this->db->get('table_name');
 
//获取字段的最大值，第二个参数为别名，相当于max(age) AS nianling
$this->db->select_max('age');
$this->db->select_max('age', 'nianling');
 
//获取字段的最小值
$this->db->select_min('age');
$this->db->select_min('age', 'nianling');
 
//获取字段的和
$this->db->select_sum('age');
$this->db->select_sum('age', 'nianling');
 
//自定义from表
$this->db->select('title, content, date');
$this->db->from('table_name');
 
//查询条件 WHERE name = 'Joe' AND title = 'boss' AND status = 'active'
$this->db->where('name', $name);
$this->db->where('title', $title);
$this->db->where('status', $status);
 
//范围查询
$this->db->where_in('item1', 'item2');
$this->db->where_not_in('item1', 'item2');
 
//匹配，第三个参数为匹配模式 title LIKE '%match%'
$this->db->like('title', 'match', 'before/after/both');
$this->db->not_like();
 
//分组 GROUP BY title, date
$this->db->group_by('title', 'date');
 
//限制条数
$this->db->limit(0, 20);
```
