lyTree ��������������ȿ��ĵ������빤��ʾ������
���ı���element-UI��ȥ������ק�Ͷ������������ܻ���ȫ�������ˣ����������˵�ѡ���ܣ�����Ķ��ǳ���

��element-UI���������֧��H5�ˣ�APP���޷�������������Ƕ�׵����ݶ����������˴���ʹ�����ʱ��������nodeKey��

��APP���﷨���ϸ񣬸�����ֵ�bug����������ˣ����������ǳ�ͷ�ۣ�������Щϸ�ڷ�����ЩС���⣬ϣ����Ҷ�����~

����Ϊ���˹����ǳ�æ��û��̫��ʱ���ע����г��������������չ�������bug�������Լ�����ľ����Լ����Ӵ~

���ײ�H5�ˡ�APP�ˡ�΢��С������ã�����С����û�����Ӧ��Ҳûʲô����~~

����������ĳ����������~~~��ϲ���磬����������������������ֵ�������

��Ҫ�������������������⣨����ͨ������ֱ�Ӽ���QQ�������Ҷ���ظ��ģ���˭Ҳû�������������������⣬���ڿ��ƻ���������Ҫ�Լ�ʵ��һ�£�

ǿ�ҽ��齫�����ĵ����꣡����ǿ�ҽ��齫�����ĵ����꣡����ǿ�ҽ��齫�����ĵ����꣡��������ʾ����Ŀ��demo������

pages.json���ã��ǳ���Ҫ������С�����APP��ֻ��ʾһ���㼶������Ϊû������������ԣ�����ʹ�ø���ճ������Ҫ�Լ�д���ʣ�����

"globalStyle": {
        ...
        "usingComponents": {
            "ly-tree-node": "/components/ly-tree/ly-tree-node"
        }
    }
App.vue������CSS��ע����App.vueȫ�����룡������

@import url("components/ly-tree/ly-tree.css");
����render-content��

����Ϊuni��H5��Ŀǰ��֧��render���������������uni,Ŀǰ��������޷�ʵ�֣�����������������Ҫ�Զ���ڵ���ʽ�����������޸�ly-tree-nodeŶ~��

�����÷���

����ע��node-key�������ã�ָ������ÿ���ڵ��Ψһ��־,����ȷ���

�����������id��������ڵ��Ψһ��־��node-key����"id";

�����������guid��������ڵ��Ψһ��־��node-key����"guid";

�����������personId��������ڵ��Ψһ��־��node-key����"personId"...
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
            ready: false, // ����������������loading����״̬�������첽���ڼ������ݵĿյ���ʾ���������ݡ�
            treeData: []
        };
    },
    onLoad() {
        // ģ���첽����
        setTimeout(() => {
            this.treeData = [{
                id: 1,
                label: 'һ�� 1',
                children: [{
                    id: 11,
                    label: '���� 1-1',
                    children: [{
                        id: 111,
                        label: '���� 1-1-1'
                    }]
                }]
            }, {
                id: 2,
                label: 'һ�� 2',
                children: [{
                    id: 21,
                    label: '���� 2-1',
                    children: [{
                        id: 211,
                        label: '���� 2-1-1'
                    }]
                }, {
                    id: 22,
                    label: '���� 2-2',
                    children: [{
                        id: 221,
                        label: '���� 2-2-1'
                    }]
                }]
            }, {
                id: 3,
                label: 'һ�� 3',
                children: [{
                    id: 31,
                    label: '���� 3-1',
                    children: [{
                        id: 311,
                        label: '���� 3-1-1'
                    }]
                }, {
                    id: 32,
                    label: '���� 3-2',
                    children: [{
                        id: 321,
                        label: '���� 3-2-1'
                    }]
                }]
            }];

            this.ready = true;
        }, 2000);
    },
    methods: {
        // uni-app��emit�����ķ���ֻ�ܽ���һ�����������Ի�ش�һ�����󣬴�ӡ���󼴿ɼ������е�����
        handleNodeClick(obj) {
            console.log('handleNodeClick', JSON.stringify(obj));
        },
        handleNodeExpand(obj) {
            console.log('handleNodeExpand', JSON.stringify(obj));
        }
    }
};
���ʱ������������������ҵ����ݽṹ����{id: 11, label: 'xx', children: []}������{personId: 11, name: 'xx', childs: []},����ô�죿������props��������

props ����˵����

������	����	Ĭ��ֵ	˵��
label	string, function(data, node)	-	ָ���ڵ��ǩΪ�ڵ�����ĳ������ֵ
icon[new]	String	-	ָ���ڵ�ͼ��Ϊ�ڵ�����ĳ������ֵ
children	string	-	ָ������Ϊ�ڵ�����ĳ������ֵ
disabled	String	boolean, function(data, node)	ָ���ڵ�ѡ����Ƿ����Ϊ�ڵ�����ĳ������ֵ
isLeaf	boolean, function(data, node)	true	ָ���ڵ��Ƿ�ΪҶ�ӽڵ㣬����ָ���� lazy ���Ե��������Ч
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
            // ������Զ��庯����Ⱦ�ڵ����Ƶȣ���Ҫ��props���óɷ������ö���ĺ������������props��һ������uni�Ὣ�����еĺ������˵����������еĺ������Իᱻ���˵�
            props: function() {
                return {
                    // �����label�Ϳ���ʹ�ú��������Զ������Ⱦ��
                    label(data, node) {
                        return '�ڵ�' + node.data.personName;
                    },
                    // label: 'personName', // ָ�������еġ�personName������labelҲ���ǽڵ�����
                    children: 'childs' // ָ�������еġ�childs������children�����ӽڵ�����
                }
            },
            // ���ֻ�ǵ����Ĵ�������ȡĳһ���ֶ���Ϊ��Ӧ���ԣ�props����ֱ������Ϊ����
            // props: {
            //  label: 'personName', // ָ�������еġ�personName������labelҲ���ǽڵ�����
            //  children: 'childs' // ָ�������еġ�childs������children�����ӽڵ�����
            // },
            treeData: [{
                personId: 1,
                personName: 'һ�� 1',
                childs: [{
                    personId: 11,
                    personName: '���� 1-1',
                    childs: [{
                        personId: 111,
                        personName: '���� 1-1-1'
                    }]
                }]
            }, {
                personId: 2,
                personName: 'һ�� 2',
                childs: [{
                    personId: 21,
                    personName: '���� 2-1',
                    childs: [{
                        personId: 211,
                        personName: '���� 2-1-1'
                    }]
                }, {
                    personId: 22,
                    personName: '���� 2-2',
                    childs: [{
                        personId: 221,
                        personName: '���� 2-2-1'
                    }]
                }]
            }, {
                personId: 3,
                personName: 'һ�� 3',
                childs: [{
                    personId: 31,
                    personName: '���� 3-1',
                    childs: [{
                        personId: 311,
                        personName: '���� 3-1-1'
                    }]
                }, {
                    personId: 32,
                    personName: '���� 3-2',
                    childs: [{
                        personId: 321,
                        personName: '���� 3-2-1'
                    }]
                }]
            }]
        };
    },
    methods: {
        // uni-app��emit�����ķ���ֻ�ܽ���һ�����������Ի�ش�һ�����󣬴�ӡ���󼴿ɼ������е�����
        handleNodeClick(obj) {
            console.log('handleNodeClick', JSON.stringify(obj));
        },
        handleNodeExpand(obj) {
            console.log('handleNodeExpand', JSON.stringify(obj));
        }
    }
};
�����أ�

tips:Ϊ��ȷ��ҳ�������ɺ��ȥ����load������thisָ����ȷ,ʹ��v-if���������������ҳ���onLoad֮�����

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
            //Ϊ��ȷ��ҳ�������ɺ��ȥ����load������thisָ����ȷ
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
        // ��Ϊ�����������Vueʵ������ĵط����ã���������ڲ���Ҫ�õ�this����Ҫ�ĳ�_self
        loadNode(node, resolve) {
            // _self.xxx; ������_self������this

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
����˵����

tips:Ϊ�������ref���ԣ���ref="tree"������÷�ʽΪthis.$refs.tree.getCheckedNodes(),���������ʾ�������ڵ��ѡ��

/*
 * @description �����ڵ����ɸѡ����
 * @method filter
 * @param {all} value �� filter-node-method ����Ϊ��һ������
 * @param {Object} data ����ָ���ڵ�Ľڵ����ݣ����������������нڵ㣬����Ҫ����A�ڵ���������ݣ���ônodeData����treeData��A�ڵ������
*/
filter(value, data) {
    if (!this.filterNodeMethod) throw new Error('[Tree] filterNodeMethod is required when filter');
    this.store.filter(value, data);
},

/*
 * @description ��ȡ�ڵ��Ψһ��ʶ��
 * @method getNodeKey
 * @param {String, Number} nodeId
 * @return {String, Number} ƥ�䵽�������е�ĳһ������
*/
getNodeKey(nodeId) {
    let node = this.store.root.getChildNodes([nodeId])[0];
    return getNodeKey(this.nodeKey, node.data);
},

/*
* @description ��ȡ�ڵ�·��
* @method getNodePath
* @param {Object} data �ڵ�����
* @return {Array} ·������
*/
getNodePath(data) {
    return this.store.getNodePath(data);
},

/*
 * @description ���ڵ�ɱ�ѡ�񣨼� show-checkbox Ϊ true�����򷵻�Ŀǰ��ѡ�еĽڵ�����ɵ�����
 * @method getCheckedNodes
 * @param {Boolean} leafOnly �Ƿ�ֻ��Ҷ�ӽڵ㣬Ĭ��false
 * @param {Boolean} includeHalfChecked �Ƿ������ѡ�ڵ㣬Ĭ��false
 * @return {Array} Ŀǰ��ѡ�еĽڵ�����ɵ�����
*/
getCheckedNodes(leafOnly, includeHalfChecked) {
    return this.store.getCheckedNodes(leafOnly, includeHalfChecked);
},

/*
 * @description ���ڵ�ɱ�ѡ�񣨼� show-checkbox Ϊ true�����򷵻�Ŀǰ��ѡ�еĽڵ�� key ����ɵ�����
 * @method getCheckedKeys
 * @param {Boolean} leafOnly �Ƿ�ֻ��Ҷ�ӽڵ㣬Ĭ��false,��Ϊ true ������ر�ѡ�е�Ҷ�ӽڵ�� keys
 * @return {Array} Ŀǰ��ѡ�еĽڵ�����ɵ�����
*/
getCheckedKeys(leafOnly) {
    return this.store.getCheckedKeys(leafOnly);
},

/*
 * @description ��ȡ��ǰ��ѡ�нڵ�� data����û�нڵ㱻ѡ���򷵻� null
 * @method getCurrentNode
 * @return {Object} ��ǰ��ѡ�нڵ�� data����û�нڵ㱻ѡ���򷵻� null
*/
getCurrentNode() {
    const currentNode = this.store.getCurrentNode();
    return currentNode ? currentNode.data : null;
},

/*
 * @description ��ȡ��ǰ��ѡ�нڵ�� key����û�нڵ㱻ѡ���򷵻� null
 * @method getCurrentKey
 * @return {all} ��ǰ��ѡ�нڵ�� key�� ��û�нڵ㱻ѡ���򷵻� null
*/
getCurrentKey() {
    const currentNode = this.getCurrentNode();
    return currentNode ? currentNode[this.nodeKey] : null;
},

/*
 * @description ����Ŀǰ��ѡ�Ľڵ�
 * @method setCheckedNodes
 * @param {Array} nodes ���չ�ѡ�ڵ����ݵ�����
 * @param {Boolean} leafOnly �Ƿ�ֻ��Ҷ�ӽڵ�, ��Ϊ true �������Ҷ�ӽڵ��ѡ��״̬��Ĭ��ֵΪ false
*/
setCheckedNodes(nodes, leafOnly) {
    this.store.setCheckedNodes(nodes, leafOnly);
},

/*
 * @description ͨ�� keys ����Ŀǰ��ѡ�Ľڵ�
 * @method setCheckedKeys
 * @param {Array} keys ��ѡ�ڵ�� key ������ 
 * @param {Boolean} leafOnly �Ƿ�ֻ��Ҷ�ӽڵ�, ��Ϊ true �������Ҷ�ӽڵ��ѡ��״̬��Ĭ��ֵΪ false
*/
setCheckedKeys(keys, leafOnly) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in setCheckedKeys');
    this.store.setCheckedKeys(keys, leafOnly);
},

/*
 * @description ͨ�� key / data ����ĳ���ڵ�Ĺ�ѡ״̬
 * @method setChecked
 * @param {all} data ��ѡ�ڵ�� key ���� data 
 * @param {Boolean} checked �ڵ��Ƿ�ѡ��
 * @param {Boolean} deep �Ƿ������ӽڵ� ��Ĭ��Ϊ false
*/
setChecked(data, checked, deep) {
    this.store.setChecked(data, checked, deep);
},

/*
 * @description ���ڵ�ɱ�ѡ�񣨼� show-checkbox Ϊ true�����򷵻�Ŀǰ��ѡ�еĽڵ�����ɵ�����
 * @method getHalfCheckedNodes
 * @return {Array} Ŀǰ��ѡ�еĽڵ�����ɵ�����
*/
getHalfCheckedNodes() {
    return this.store.getHalfCheckedNodes();
},

/*
 * @description ���ڵ�ɱ�ѡ�񣨼� show-checkbox Ϊ true�����򷵻�Ŀǰ��ѡ�еĽڵ�� key ����ɵ�����
 * @method getHalfCheckedKeys
 * @return {Array} Ŀǰ��ѡ�еĽڵ�� key ����ɵ�����
*/
getHalfCheckedKeys() {
    return this.store.getHalfCheckedKeys();
},

/*
 * @description ͨ�� node ����ĳ���ڵ�ĵ�ǰѡ��״̬
 * @method setCurrentNode
 * @param {Object} node ����ѡ�ڵ�� node
*/
setCurrentNode(node) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in setCurrentNode');
    this.store.setUserCurrentNode(node);
},

/*
 * @description ͨ�� key ����ĳ���ڵ�ĵ�ǰѡ��״̬
 * @method setCurrentKey
 * @param {all} key ����ѡ�ڵ�� key����Ϊ null ��ȡ����ǰ�����Ľڵ�
*/
setCurrentKey(key) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in setCurrentKey');
    this.store.setCurrentNodeKey(key);
},

/*
 * @description ���� data ���� key �õ� Tree ����е� node
 * @method getNode
 * @param {all} data Ҫ��� node �� key ���� data
*/
getNode(data) {
    return this.store.getNode(data);
},

/*
 * @description ɾ�� Tree �е�һ���ڵ�
 * @method remove
 * @param {all} data Ҫɾ���Ľڵ�� data ���� node
*/
remove(data) {
    this.store.remove(data);
},

/*
 * @description Ϊ Tree �е�һ���ڵ�׷��һ���ӽڵ�
 * @method append
 * @param {Object} data Ҫ׷�ӵ��ӽڵ�� data 
 * @param {Object} parentNode �ӽڵ�� parent �� data��key ���� node
*/
append(data, parentNode) {
    this.store.append(data, parentNode);
},

/*
 * @description Ϊ Tree ��һ���ڵ��ǰ������һ���ڵ�
 * @method insertBefore
 * @param {Object} data Ҫ���ӵĽڵ�� data 
 * @param {all} refNode Ҫ���ӵĽڵ�ĺ�һ���ڵ�� data��key ���� node
*/
insertBefore(data, refNode) {
    this.store.insertBefore(data, refNode);
},

/*
 * @description Ϊ Tree ��һ���ڵ�ĺ�������һ���ڵ�
 * @method insertAfter
 * @param {Object} data Ҫ���ӵĽڵ�� data 
 * @param {all} refNode Ҫ���ӵĽڵ��ǰһ���ڵ�� data��key ���� node
*/
insertAfter(data, refNode) {
    this.store.insertAfter(data, refNode);
},

/*
 * @description ͨ�� keys ���ýڵ���Ԫ��
 * @method updateKeyChildren
 * @param {String, Number} key �ڵ� key 
 * @param {Object} data �ڵ����ݵ�����
*/
updateKeyChildren(key, data) {
    if (!this.nodeKey) throw new Error('[Tree] nodeKey is required in updateKeyChild');
    this.store.updateChildren(key, data);
}
����˵����

������	����	Ĭ��ֵ	˵��
nodeKey	String	-	���ÿ�����ڵ�������ΪΨһ��ʶ�����ԣ�������Ӧ����Ψһ��
treeData	Array	-	��������ʱ��չʾ����
ready[new]	Boolean	true	��������loading���أ��������ݻ�û��ȡ���Ŀյ����֡��������ݡ�����
emptyText	String	��������	����Ϊ�յ�ʱ��չʾ���ı�
renderAfterExpand	Boolean	true	�Ƿ��ڵ�һ��չ��ĳ�����ڵ�����Ⱦ���ӽڵ�
checkStrictly	Boolean	false	����ʾ��ѡ�������£��Ƿ��ϸ����ѭ���Ӳ����������������Ĭ��Ϊ false�����������checkOnlyLeafΪtrue������Ҳ����Ϊtrue
defaultExpandAll	Boolean	false	�Ƿ�Ĭ��չ�����нڵ�
expandOnClickNode	Boolean	true	�Ƿ��ڵ���ڵ��ʱ��չ�����������ڵ㣬 Ĭ��ֵΪ true�����Ϊ false����ֻ�е��ͷͼ���ʱ��Ż�չ�����������ڵ�
checkOnClickNode	Boolean	false	�Ƿ��ڵ���ڵ��ʱ��ѡ�нڵ㣬Ĭ��ֵΪ false����ֻ���ڵ����ѡ��ʱ�Ż�ѡ�нڵ�
autoExpandParent	Boolean	true	�Ƿ��ڵ�һ��չ��ĳ�����ڵ�����Ⱦ���ӽڵ�
defaultCheckedKeys	Array	-	Ĭ�Ϲ�ѡ�Ľڵ�� key ������
defaultExpandedKeys	Array	-	Ĭ��չ���Ľڵ�� key ������
currentNodeKey	String, Number	-	��ǰѡ�еĽڵ�
showCheckbox	Boolean	false	�ڵ��Ƿ�ɱ�ѡ��
showRadio	Boolean	false	�ڵ��Ƿ�ɱ���ѡ
checkOnlyLeaf[new]	Boolean	false	�Ƿ����һ��Ҷ�ӽڵ����ʾ��ѡ/��ѡ��
props	Object	������	�����ж�Ӧ��������
lazy	Boolean	false	�Ƿ��������ӽڵ㣬���� load �������ʹ��
highlightCurrent	Boolean	false	�Ƿ������ǰѡ�нڵ㣬Ĭ��ֵ�� false
load	Function	-	�����������ݵķ��������� lazy ����Ϊtrue ʱ��Ч
filterNodeMethod	Function	-	�����ڵ����ɸѡʱִ�еķ��������� true ��ʾ����ڵ������ʾ������ false ���ʾ����ڵ�ᱻ����
childVisibleForFilterNode[new]	Boolean	false	����ʱ�Ƿ�չʾƥ����������ӽڵ�
accordion	Boolean	false	�Ƿ�ÿ��ֻ��һ��ͬ�����ڵ�չ��
indent	Number	18	���ڼ��ڵ���ˮƽ��������λΪ����
iconClass	String	-	�Զ������ڵ��չ��ͼ�꣬ͼ��class
showNodeIcon[new]	Boolean	false	�Ƿ���ʾ�ڵ�ͼ�꣬�������Ϊtrue,��Ҫ����props�ж�Ӧ��ͼ����������
�汾���£�

�汾	����ʱ��	����˵��
1.01	2019-12-04	�޸��첽��ȡ����treeData�����ص�����
1.02	2019-12-12	���С�����ַ��ٲ���Ч��bug ʹ�ñհ���ʽ������������ʱ��store���󱻸��ǵ��µ�һϵ��ָ������
1.03	2019-12-16	�޸���ʽclass(iconfont => ly-iconfont)����������Ŀ��ͻ
1.04	2019-12-21	���checkOnClickNode�ڵ�ѡ��ʧЧ��bug ����checkOnlyLeaf���ã����ڵ������һ��Ҷ�ӽڵ�ʱ����ʾѡ���
1.05	2019-12-26	���undefined is not an object (evaluating 'e.detail.args'); ��bug
1.06	2019-12-27	uniȫ�ּ����¼�����Ψһ��־ǰ׺��������tree����ʱ������ͬһ���¼�
1.07	2020-01-04	����showNodeIcon���������Ƿ���ʾ�ڵ�ͼ�꣬�������Ϊtrue,��Ҫ����props�ж�Ӧ��ͼ���������� ����childVisibleForFilterNode���ã�����������ʱ�Ƿ�չʾƥ����������ӽڵ㣬Ĭ��false
1.08	2020-01-05	��������ָ���սڵ㹦�ܼ�����
1.09	2020-02-08	���HbuilderX2.5.1�汾���º����ڵײ������Ʊ仯����APP����ʾ���������ݡ������⣻����ready��������������loading����״̬����δ��ȡ������ʱ��ʾ���������ݡ������⣻�����ڳ���/������ʹ���������ʾ��
1.10	2020-02-20	�޸���һ��Сbug
1.11	2020-03-19	���icon��֧��class��ʽ�����⣬���iconͼ����APP�˵���ʾ���� ����toggleExpendAll���ã��л�ȫ��չ����ȫ���۵�
1.12	2020-03-23	һЩʵ�ַ�ʽ�ĵ��������䷽��˵���ĵ�,������ʽ��Ϊ��ͨcss�����ⲿ���û���Ҫ��װsass�����������
1.13	2020-04-06	���showRadioʱ��Ĭ��ѡ�л�ѡ���������⣻֧��props���ú������������ö��󣬽��label���Ե��޷�ʹ�ú�����Ⱦ������
�������¼��� �μ�elementUI˵����https://element.eleme.cn/#/zh-CN/component/tree