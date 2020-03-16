+ `//`：从根的子子孙孙中寻找
+ `/`：从根的孩子中寻找
+ `.//`：从当前节点的子子孙孙中寻找
+ `./`：从当前节点的孩子中寻找
+ `//div`：从根的子子孙孙中寻找`div`元素
+ `//div[@id="text"]`：从根的子子孙孙中寻找含有属性`id=text`的`div`元素
+ `//div/@href`：获取所有`div`元素的`href`值
+ `//div[@id="text"]/text()`：获取文本信息
+ 其他
    ```python
    # 匹配具有多个属性的元素
    hxs = Selector(response=response).xpath('//a[@href="link.html"][@id="i1"]')
    print(hxs)

    # 匹配某属性包含某字符串的a元素
    hxs = Selector(response=response).xpath('//a[contains(@href, "link")]')
    print(hxs)

    # 匹配某属性由某字符串开头的a元素
    hxs = Selector(response=response).xpath('//a[starts-with(@href, "link")]')
    print(hxs)

    # 正则匹配
    hxs = Selector(response=response).xpath('//a[re:test(@id, "i\d+")]')
    print(hxs)
    hxs = Selector(response=response).xpath('//a[re:test(@id, "i\d+")]/text()').extract()
    print(hxs)
    hxs = Selector(response=response).xpath('//a[re:test(@id, "i\d+")]/@href').extract()
    print(hxs)

    # 链式匹配
    hxs = Selector(response=response).xpath('/html/body/ul/li/a/@href').extract()
    print(hxs)
    hxs = Selector(response=response).xpath('//body/ul/li/a/@href').extract_first()
    print(hxs)
    ```