<!--
 * @Author: whisperer
 * @Date: 2020-02-20 11:15:54
 * @LastEditors: whisperer
 * @LastEditTime: 2020-02-20 12:34:49
 * @Description: UI自动化测试添加属性说明
 -->
# UI自动化测试添加属性说明

##  一.背景
```
    出发点:
        前端UI自动化测试用例需要定位前端页面元素;由于层级和样式可能会随着版本迭代变动,导致用例需要重新维护,所以使用前端唯一属性来进行定位.

    说明:
        对前端项目来说,添加的属性是没有实际意义的,现在项目大多采用前端框架,即使用数据驱动理念进行项目开发,基本不会使用到手动dom操作,故添加此属性主要是为了服务于UI自动化测试
```
## 二.方案

 | 序号 | 方案描述 | 优点 | 缺点 |
 | ------  | ------ | ------ | ------  |
 | 1 | 使用id进行元素绑定 | 简便,方便添加 | 存在冲突风险,不可配置 |
 | 2 | 使用data-*自定义属性进行元素绑定 | 较简便,方便添加,扩展性好 | - |
 | 3 | 可配置属性进行元素绑定 | 可配置化,可以自定义属性名称 | 添加起来较前两者要麻烦些 |

## 三.各种方案对应实例
> 下面使用三种方案对目标按钮进行属性绑定

### (1) 使用id进行元素绑定
```
    <button id="btn">submit</button>
```
### (2) 使用data-*自定义属性进行元素绑定
```
    <button data-uid="btn">submit</button>
```
### (3) 可配置属性进行元素绑定
```
    1.属性名称配置与公共方法定义
    const UITestAttrName = 'data-uid';//此处可配置属性名称

    /*
    * @params {string} attrVal ui自动化测试目标元素的唯一属性值
    */
    const getUAttrObj = (attrVal) => {
        //此处可以进行一系列属性值格式化操作
        return {
            [UITestAttrName]:attrVal
        }
    }

    2. 页面引入与使用
    import {getUAttrObj} from '@utils';

    <button {...getUAttrObj('btn')}>submit</button>
```

## 四.添加属性作用延申
```
    很多系统存在用户帮助或者新手教程等用户友好功能,而此种功能的实现库大多基于jquery等第三方库,在使用的时候需要使用到元素定位,由于UI自动化测试的添加属性是唯一的,故可以复用添加的属性进行元素定位.

    例: 使用driver.js进行元素介绍与高亮
        const driver = new Driver();
        driver.highlight({
            element: '#btn',
            popover: {
                title: 'Title for the Popover',
                description: 'Description for it',
            }
        });
    上例的element字段是css选择器字符串,故可以使用UI自动化测试添加属性进行定位,如: [data-uid='btn']
```

## 五.相关链接
[新手帮助功能实现库之一:driver.js](https://github.com/kamranahmedse/driver.js/blob/master/readme.md 'driver.js')

[data-*自定义属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/data-* 'data-*自定义属性')
