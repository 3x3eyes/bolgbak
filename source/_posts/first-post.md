---
title: 商城前台-三级分类筛选技巧
date: 2019-01-07 21:06:43
tags:
	- 两个技巧
	- 层叠关系
---

什么时候使用两个技巧:

1、数据表的数据具有层级（父子）关系（如分类表、权限表,有字段指向父级id如：pid）

2、数据在模板中展示也具有层级关系。



由于从数据库取出来的所有数据一般都是二维数组，且下标默认从0开始。

技巧1：以二维数组中的每个元素的主键字段值作为其相应下标。

技巧2：以指向父字段值（如pid字段）进行分组，即把具有相同的pid值分为同一组。



掌握两个技巧具体可以应用的场景：

1、后台左侧菜单权限遍历

2、rbac给角色分配权限

3、jd商城前台的三级分类筛选

4、导航中的菜单



核心代码如下:

    public function add(){
        //查询取出Auth下的所有权限数据
        $oldAuth = Auth::select()->toArray();
        //技巧1:循环遍历$oldAuth数组 让每个元素的主键值作为其对应的下标
        $auths = [];
        foreach ($oldAuth as $v) {
            //如果数组中[]没有值,下标默认从0开始递增
            $auths[ $v['auth_id'] ] = $v;
        }
        //技巧二:循环$oldAuth数组 把每个元素通过pid进行分组 把相同的pid的元素划分为同一组
        $authdata = [];
        foreach ($oldAuth as $v){
            $authdata[ $v['pid'] ][] = $v['auth_id'];
        }
        return $this->fetch('',[
            'auths' => $auths,
            'authdata' => $authdata
        ]);
    }


