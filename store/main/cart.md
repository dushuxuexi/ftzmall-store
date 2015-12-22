# 购物车

###  模块规划
//to be done
###  设计思路
//to be done
###  代码实现

##### 1. php加载部分

    1.1 getCartList API获取购物车信息
        * itemId，quantity等
    1.2 调用getExtraInfoFromSearch获取实时和额外的信息
        * processGetSinglePrice 先去获取offerprice（这是最新最准的价格）
        * taxRate
        * buyable
        * minBuyCount
        * maxBuyCount
        * productId（对于多sku的商品，需要父id用于显示详情页）
        * promotionPrice（这个不是实时的，虽然会去取，但在实际中并没有使用）
        * children 组合商品会有这个字段
            - quantity
            - parentCatentryId（同 productID）
            - itemDisplayText
            - itemImageLink
            - Sales-Weight
            - Sales-WeightUOM
            - itemTaxRate
    1.3. 获取额外信息以后根据itemId一一映射到购物车的数据中用于显示和计算，如果某一个商品在search中没有返回结果，则对该商品进行下架处理。
    
    
    除此之外，还需要做两个辅助的工作：
    *  写cookie（数量，起订，限购等）
    *  写缓存（方便结算页后校验使用）


	至此，php加载结束，剩下的都在页面使用js动态加载

### 2. js异步加载
####2.1. 检查库存
        1.1 如果库存=0，提示无货，该商品不可操作
        1.2 根据当前要买的数量，起订，限购，库存之间的关系，得出一个合适的数量，如果当前用户添加的数量不满足这个关系自动帮用户修改数量，更新价格
####2.2. 刷新价格
        2.2.1 根据用户选中的商品（cartlineId），在php端组成计算需要的数据，计算价格：
        2.2.2. offerprice是从（1）里获取，promotionprice是从计算接口返回，
####2.3. 查询包邮提醒
        根据固定的tag查询是否有包邮规则
####2.4. 其他
        //TBD


### 3. 用户操作
####3.1 点选
        每次点选都会调用刷新价格，重新计算价格，促销
####3.2 更改数量
        3.2.1 检查所修改的数量是否符合规范
            (a) 限购，起订 这两个值是放在cookie里的
            (b) 库存，当库存大于500的时候库存会放在cookie中
            (c) 在最终更新的时候，仍然会检查库存（cookie中没有）
        3.2.1 刷新该商品所在的那一单的价格
####3.3 删除
        3.3.1 刷新价格
####3.4 提交
        3.4.1 根据选中的商品，提交数据（只提交cartlineId）
        3.4.2 在提交之前会对商品的购买规范进行检查，如果不通过，则不能提交
##  功能体验


##  开发和使用

// to be done 
####  待改进部分
1. 添加购物车的时候，购物车中保存了很多无用的信息，购物车中能用的只有一些静态信息，如 itemId, displaytext, imagelink之类的 price, buyable,taxrate等等信息或者不是实时的或者没有保存
2. 计算API，为了支持直接购买，每次计算需要传很大一块数据过去，基本上把购物车原封不动的传过去了，建议是不是可以把这两个API分开
3. 后端是否需要考虑把限购，库存等限制条件在后端处理
4. 购物车 平台包邮规则提醒
5. 提升计算速度，特别是计算接口
6. 在优惠券这块，检查券是否适应当前这个订单这个可以优化




	

