对lyTree 树组件进行改造加上自己需求  （有问题先看文档，导入工程示例看） 看官网 https://ext.dcloud.net.cn/plugin?id=1000
·改编自element-UI，去掉了拖拽和动画，其他功能基本全部保留了，并且新增了单选功能，代码改动非常大；

·element-UI的树组件仅支持H5端，APP端无法给组件挂载深度嵌套的数据对象，所以做了处理，使用组件时必须配置nodeKey；

·APP端语法很严格，各种奇怪的bug（但都解决了），做起来非常头疼，可能有些细节方面有些小问题，希望大家多多包涵~

·因为本人工作非常忙，没有太多时间关注插件市场，所以如果有扩展需求或者bug，可以自己解决的尽量自己解决哟~

·亲测H5端、APP端、微信小程序可用，其他小程序没测过，应该也没什么问题~~

·心灵脆弱的程序媛求好评~~~不喜勿喷，有问题好商量，切勿做伸手党和喷子

不要带着问题打差评，有问题（或者通用需求）直接加我QQ，看到我都会回复的（另：谁也没有义务替你解决所有问题，过于客制化的需求需要自己实现一下）

强烈建议将以下文档看完！！！强烈建议将以下文档看完！！！强烈建议将以下文档看完！！！导入示例项目看demo！！！

pages.json配置（非常重要！！！小程序和APP端只显示一个层级就是因为没有配置这个属性！！请使用复制粘贴！不要自己写错单词！）：

"globalStyle": {
        ...
        "usingComponents": {
            "ly-tree-node": "/components/ly-tree/ly-tree-node"
        }
    }
App.vue中引入CSS（注意是App.vue全局引入！！）：

@import url("components/ly-tree/ly-tree.css");
关于render-content：

·因为uni非H5端目前不支持render函数，插件依赖于uni,目前这个需求还无法实现，如果有特殊的需求需要自定义节点样式，可以自行修改ly-tree-node哦~；

基本用法：

·请注意node-key必须配置，指数据中每个节点的唯一标志,打个比方：

如果数据中以id数据项当做节点的唯一标志，node-key就是"id";

如果数据中以guid数据项当做节点的唯一标志，node-key就是"guid";

如果数据中以personId数据项当做节点的唯一标志，node-key就是"personId"...
<template>
    <ly-tree :tree-data="treeData" 
        :ready="ready"
        node-key="id" 
        @node-expand="handleNodeExpand" 
        @node-click="handleNodeClick">
        </ly-tree>
</template>
import LyTree from '@/components/ly-tree/ly-tree.vue'
export default {
    components: {
        LyTree
    },
    data() {
        return {
            ready: false, // 这里用于自主控制loading加载状态，避免异步正在加载数据的空档显示“暂无数据”
            treeData: []
        };
    },
    onLoad() {
        // 模拟异步请求
        setTimeout(() => {
            this.treeData = [{
                id: 1,
                label: '一级 1',
                children: [{
                    id: 11,
                    label: '二级 1-1',
                    children: [{
                        id: 111,
                        label: '三级 1-1-1'
                    }]
                }]
            }, {
                id: 2,
                label: '一级 2',
                children: [{
                    id: 21,
                    label: '二级 2-1',
                    children: [{
                        id: 211,
                        label: '三级 2-1-1'
                    }]
                }, {
                    id: 22,
                    label: '二级 2-2',
                    children: [{
                        id: 221,
                        label: '三级 2-2-1'
                    }]
                }]
            }, {
                id: 3,
                label: '一级 3',
                children: [{
                    id: 31,
                    label: '二级 3-1',
                    children: [{
                        id: 311,
                        label: '三级 3-1-1'
                    }]
                }, {
                    id: 32,
                    label: '二级 3-2',
                    children: [{
                        id: 321,
                        label: '三级 3-2-1'
                    }]
                }]
            }];

            this.ready = true;
        }, 2000);
    },
    methods: {
        // uni-app中emit触发的方法只能接受一个参数，所以会回传一个对象，打印对象即可见到其中的内容
        handleNodeClick(obj) {
            console.log('handleNodeClick', JSON.stringify(obj));
        },
        handleNodeExpand(obj) {
            console.log('handleNodeExpand', JSON.stringify(obj));
        }
    }
};
这个时候可能有人提出：如果我的数据结构不是{id: 11, label: 'xx', children: []}，而是{personId: 11, name: 'xx', childs: []},该怎么办？？以下props属性配置

props 属性说明：

属性名	类型	默认值	说明
label	string, function(data, node)	-	指定节点标签为节点对象的某个属性值
icon[new]	String	-	指定节点图标为节点对象的某个属性值
children	string	-	指定子树为节点对象的某个属性值
disabled	String	boolean, function(data, node)	指定节点选择框是否禁用为节点对象的某个属性值
isLeaf	boolean, function(data, node)	true	指定节点是否为叶子节点，仅在指定了 lazy 属性的情况下生效
<template>
    <ly-tree :tree-data="treeData" 
        :props="props"
        node-key="personId" 
        @node-expand="handleNodeExpand" 
        @node-click="handleNodeClick">
        </ly-tree>
</template>
import LyTree from '@/components/ly-tree/ly-tree.vue'
export default {
    components: {
        LyTree
    },
    data() {
        return {
            ready: false,
            // 如果想自定义函数渲染节点名称等，需要将props设置成返回配置对象的函数，否则如果props是一个对象（uni会将对象中的函数过滤掉），对象中的函数属性会被过滤掉
            props: function() {
                return {
                    // 这里的label就可以使用函数进行自定义的渲染了
                    label(data, node) {
                        return '节点' + node.data.personName;
                    },
                    // label: 'personName', // 指把数据中的‘personName’当做label也就是节点名称
                    children: 'childs' // 指把数据中的‘childs’当做children当做子节点数据
                }
            },
            // 如果只是单纯的从数据中取某一个字段作为对应属性，props可以直接设置为对象
            // props: {
            //  label: 'personName', // 指把数据中的‘personName’当做label也就是节点名称
            //  children: 'childs' // 指把数据中的‘childs’当做children当做子节点数据
            // },
            treeData: [{
                personId: 1,
                personName: '一级 1',
                childs: [{
                    personId: 11,
                    personName: '二级 1-1',
                    childs: [{
                        personId: 111,
                        personName: '三级 1-1-1'
                    }]
                }]
            }, {
                personId: 2,
                personName: '一级 2',
                childs: [{
                    personId: 21,
                    personName: '二级 2-1',
                    childs: [{
                        personId: 211,
                        personName: '三级 2-1-1'
                    }]
                }, {
                    personId: 22,
                    personName: '二级 2-2',
                    childs: [{
                        personId: 221,
                        personName: '三级 2-2-1'
                    }]
                }]
            }, {
                personId: 3,
                personName: '一级 3',
                childs: [{
                    personId: 31,
                    personName: '二级 3-1',
                    childs: [{
                        personId: 311,
                        personName: '三级 3-1-1'
                    }]
                }, {
                    personId: 32,
                    personName: '二级 3-2',
                    childs: [{
                        personId: 321,
                        personName: '三级 3-2-1'
                    }]
                }]
            }]
        };
    },
    methods: {
        // uni-app中emit触发的方法只能接受一个参数，所以会回传一个对象，打印对象即可见到其中的内容
        handleNodeClick(obj) {
            console.log('handleNodeClick', JSON.stringify(obj));
        },
        handleNodeExpand(obj) {
            console.log('handleNodeExpand', JSON.stringify(obj));
        }
    }
};
懒加载：

tips:为了确保页面加载完成后才去调用load方法，this指向正确,使用v-if懒加载树组件，在页面的onLoad之后调用

<template>
    <ly-tree v-if="isReady" :props="props" node-key="name" :load="loadNode" lazy>
    </ly-tree>
</template>
import LyTree from '@/components/ly-tree/ly-tree.vue'

var _self;
export default {
    components: {
        LyTree
    },
    data() {
        return {
            //为了确保页面加载完成后才去调用load方法，this指向正确
            isReady: false,
            props: {
                label: 'name',
                children: 'zones',
                isLeaf: 'leaf'
            }
        };
    },

    onLoad() {
        _self = this;
        this.isReady = true;
    },

    methods: {
        // 因为这个函数是在Vue实例以外的地方调用，如果函数内部需要用到this，需要改成_self
        loadNode(node, resolve) {
            // _self.xxx; 这里用_self而不是this

            if (node.level === 0) {
                setTimeout(() => {
                    resolve([{
                        name: 'region'
                    }]);
                }, 1000);
            } else {
                if (node.level > 1) return resolve([]);

                setTimeout(() => {
                    const data = [{
                        name: 'leaf',
                        leaf: true
                    }, {
                        name: 'zone'
                    }];

                    resolve(data);
                }, 500);
            }
        }
    }
};
方法说明：

tips:为树组件绑定ref属性，如ref="tree"，则调用方式为this.$refs.tree.getCheckedNodes(),详情见工程示例“树节点的选择”

/*
 * @description 对树节点进行筛选操作
 * @method filter
 * @param {all} value 在 filter-node-method 中作为第一个参数
 * @param {Object} data 搜索指定节点的节点数据，不传代表搜索所有节点，假如要搜索A节点下面的数据，那么nodeData代表treeData中A节点的数据
*/
filter(value, data) {
    if (!this.filterNodeMethod) throw new Error('[Tree] filterNodeMethod is required when filter');
    this.store.filter(value, data);
},

/*
 * @description 获取节点的唯一标识符
 * @method getNodeKey
 * @param {String, Number} nodeId
 * @return {String, Number} 匹配到的数据中的某一项数据
*/
getNodeKey(nodeId) {
    let node = this.store.root.getChildNodes([nodeId])[0];
    return getNodeKey(this.nodeKey, node.data);
},

/*
* @description 获取节点路径
* @method getNodePath
* @param {Object} data 节点数据
* @return {Array} 路径数组
*/
getNodePath(data) {
    return this.store.getNodePath(data);
},

/*
 * @description 若节点可被选择（即 show-checkbox 为 true），则返回目前被选中的节点所组成的数组
 * @method getCheckedNodes
 * @param {Boolean} leafOnly 是否只是叶子节点，默认false
 * @param {Boolean} includeHalfChecked 是否包含半选节点，默认false
 * @return {Array} 目前被选中的节点所组成的数组
*/
getCheckedNodes(leafOnly, includeHalfChecked) {
    return this.store.getCheckedNodes(leafOnly, includeHalfChecked);
},

/*
 * @description 若节点可被选择（即 show-checkbox 为 true），则返回目前被选中的节点的 key 所组成的数组
 * @method getCheckedKeys
 * @param {Boolean} leafOnly 是否只是叶子节点，默认false,若为 true 则仅返回被选中的叶子节点的 keys
 * @return {Array} 目前被选中的节点所组成的数组
*/
getCheckedKeys(leafOnly) {
    return this.store.getCheckedKeys(leafOnly);
},

/*
 * @description 获取当前被选中节点的 data，若没有节点被选中则返回 null
 * @method getCurrentNode
 * @return {Object} 当前被选中节点的 data，若没有节点被选中则返回 null
*/
getCurrentNode() {
    const currentNode = this.store.getCurrentNode();
    return currentNode ? currentNode.data : null;
},

/*
 * @description 获取当前被选中节点的 key，若没有节点被选中则返回 null
 * @method getCurrentKey
 * @return {all} 当前被选中节点的 key， 若没有节点被选中则返回 null
*/
getCurrentKey() {
    const currentNode = this.getCurrentNode();
    return currentNode ? currentNode[this.nodeKey] : null;
},

/*
 * @description 设置目前勾选的节点
 * @method setCheckedNodes
 * @param {Array} nodes 接收勾选节点数据的数组
 * @param {Boolean} leafOnly 是否只是叶子节点, 若为 true 则仅设置叶子节点的选中状态，默认值为 false
*/
setCheckedNodes(nodes, leafOnly) {
    this.store.setCheckedNodes(nodes, leafOnly);
},

/*
 * @description 通过 keys 设置目前勾选的节点
 * @method setCheckedKeys
 * @param {Array} keys 勾选节点的 key 的数组 
 * @param {Boolean} leafOnly 是否只是叶子节点, 若为 true 则仅设置叶子节点的选中状态，默认值为 false
*/
setCheckedKeys(keys, leafOnly) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in setCheckedKeys');
    this.store.setCheckedKeys(keys, leafOnly);
},

/*
 * @description 通过 key / data 设置某个节点的勾选状态
 * @method setChecked
 * @param {all} data 勾选节点的 key 或者 data 
 * @param {Boolean} checked 节点是否选中
 * @param {Boolean} deep 是否设置子节点 ，默认为 false
*/
setChecked(data, checked, deep) {
    this.store.setChecked(data, checked, deep);
},

/*
 * @description 若节点可被选择（即 show-checkbox 为 true），则返回目前半选中的节点所组成的数组
 * @method getHalfCheckedNodes
 * @return {Array} 目前半选中的节点所组成的数组
*/
getHalfCheckedNodes() {
    return this.store.getHalfCheckedNodes();
},

/*
 * @description 若节点可被选择（即 show-checkbox 为 true），则返回目前半选中的节点的 key 所组成的数组
 * @method getHalfCheckedKeys
 * @return {Array} 目前半选中的节点的 key 所组成的数组
*/
getHalfCheckedKeys() {
    return this.store.getHalfCheckedKeys();
},

/*
 * @description 通过 node 设置某个节点的当前选中状态
 * @method setCurrentNode
 * @param {Object} node 待被选节点的 node
*/
setCurrentNode(node) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in setCurrentNode');
    this.store.setUserCurrentNode(node);
},

/*
 * @description 通过 key 设置某个节点的当前选中状态
 * @method setCurrentKey
 * @param {all} key 待被选节点的 key，若为 null 则取消当前高亮的节点
*/
setCurrentKey(key) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in setCurrentKey');
    this.store.setCurrentNodeKey(key);
},

/*
 * @description 根据 data 或者 key 拿到 Tree 组件中的 node
 * @method getNode
 * @param {all} data 要获得 node 的 key 或者 data
*/
getNode(data) {
    return this.store.getNode(data);
},

/*
 * @description 删除 Tree 中的一个节点
 * @method remove
 * @param {all} data 要删除的节点的 data 或者 node
*/
remove(data) {
    this.store.remove(data);
},

/*
 * @description 为 Tree 中的一个节点追加一个子节点
 * @method append
 * @param {Object} data 要追加的子节点的 data 
 * @param {Object} parentNode 子节点的 parent 的 data、key 或者 node
*/
append(data, parentNode) {
    this.store.append(data, parentNode);
},

/*
 * @description 为 Tree 的一个节点的前面增加一个节点
 * @method insertBefore
 * @param {Object} data 要增加的节点的 data 
 * @param {all} refNode 要增加的节点的后一个节点的 data、key 或者 node
*/
insertBefore(data, refNode) {
    this.store.insertBefore(data, refNode);
},

/*
 * @description 为 Tree 的一个节点的后面增加一个节点
 * @method insertAfter
 * @param {Object} data 要增加的节点的 data 
 * @param {all} refNode 要增加的节点的前一个节点的 data、key 或者 node
*/
insertAfter(data, refNode) {
    this.store.insertAfter(data, refNode);
},

/*
 * @description 通过 keys 设置节点子元素
 * @method updateKeyChildren
 * @param {String, Number} key 节点 key 
 * @param {Object} data 节点数据的数组
*/
updateKeyChildren(key, data) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in updateKeyChild');
    this.store.updateChildren(key, data);
}
属性说明：

属性名	类型	默认值	说明
nodeKey	String	-	必填，每个树节点用来作为唯一标识的属性，整棵树应该是唯一的
treeData	Array	-	非懒加载时的展示数据
ready[new]	Boolean	true	自主控制loading加载，避免数据还没获取到的空档出现“暂无数据”字样
emptyText	String	暂无数据	内容为空的时候展示的文本
renderAfterExpand	Boolean	true	是否在第一次展开某个树节点后才渲染其子节点
checkStrictly	Boolean	false	在显示复选框的情况下，是否严格的遵循父子不互相关联的做法，默认为 false，如果设置了checkOnlyLeaf为true，这里也将变为true
defaultExpandAll	Boolean	false	是否默认展开所有节点
expandOnClickNode	Boolean	true	是否在点击节点的时候展开或者收缩节点， 默认值为 true，如果为 false，则只有点箭头图标的时候才会展开或者收缩节点
checkOnClickNode	Boolean	false	是否在点击节点的时候选中节点，默认值为 false，即只有在点击复选框时才会选中节点
autoExpandParent	Boolean	true	是否在第一次展开某个树节点后才渲染其子节点
defaultCheckedKeys	Array	-	默认勾选的节点的 key 的数组
defaultExpandedKeys	Array	-	默认展开的节点的 key 的数组
currentNodeKey	String, Number	-	当前选中的节点
showCheckbox	Boolean	false	节点是否可被选择
showRadio	Boolean	false	节点是否可被单选
checkOnlyLeaf[new]	Boolean	false	是否最后一层叶子节点才显示单选/多选框
props	Object	见下文	数据中对应的属性名
lazy	Boolean	false	是否懒加载子节点，需与 load 方法结合使用
highlightCurrent	Boolean	false	是否高亮当前选中节点，默认值是 false
load	Function	-	加载子树数据的方法，仅当 lazy 属性为true 时生效
filterNodeMethod	Function	-	对树节点进行筛选时执行的方法，返回 true 表示这个节点可以显示，返回 false 则表示这个节点会被隐藏
childVisibleForFilterNode[new]	Boolean	false	搜索时是否展示匹配项的所有子节点
accordion	Boolean	false	是否每次只打开一个同级树节点展开
indent	Number	18	相邻级节点间的水平缩进，单位为像素
iconClass	String	-	自定义树节点的展开图标，图标class
showNodeIcon[new]	Boolean	false	是否显示节点图标，如果配置为true,需要配置props中对应的图标属性名称
版本更新：

版本	更新时间	更新说明
1.01	2019-12-04	修复异步获取数据treeData不加载的问题
1.02	2019-12-12	解决小程序手风琴不生效的bug 使用闭包方式解决多个树并存时，store对象被覆盖导致的一系列指向问题
1.03	2019-12-16	修改样式class(iconfont => ly-iconfont)，避免与项目冲突
1.04	2019-12-21	解决checkOnClickNode在单选中失效的bug 新增checkOnlyLeaf配置，当节点是最后一层叶子节点时才显示选择框
1.05	2019-12-26	解决undefined is not an object (evaluating 'e.detail.args'); 的bug
1.06	2019-12-27	uni全局监听事件加上唯一标志前缀，避免多个tree并存时监听到同一个事件
1.07	2020-01-04	新增showNodeIcon，可配置是否显示节点图标，如果配置为true,需要配置props中对应的图标属性名称 新增childVisibleForFilterNode配置，可配置搜索时是否展示匹配项的所有子节点，默认false
1.08	2020-01-05	新增搜索指定空节点功能及样例
1.09	2020-02-08	解决HbuilderX2.5.1版本更新后由于底层编译机制变化导致APP端显示“暂无数据”的问题；新增ready属性已自主控制loading加载状态避免未获取到数据时显示“暂无数据”的问题；新增在抽屉/弹框中使用树组件的示例
1.10	2020-02-20	修改了一个小bug
1.11	2020-03-19	解决icon不支持class样式的问题，解决icon图标在APP端的显示问题 新增toggleExpendAll配置：切换全部展开、全部折叠
1.12	2020-03-23	一些实现方式的调整，补充方法说明文档,样例样式改为普通css，避免部分用户需要安装sass插件才能运行
1.13	2020-04-06	解决showRadio时，默认选中会选择多个的问题；支持props配置函数，返回配置对象，解决label属性等无法使用函数渲染的问题
方法及事件： 参见elementUI说明：https://element.eleme.cn/#/zh-CN/component/tree
