<style lang="less">
    @import '../../../../less/index.less';
    @import '../../../../less/user-info.less';
    #user-info-order {

    }
</style>

<template>
    <div id="user-info-order" class="user-info-content-box">
        <Row>
            <Col span="24">
                <h4 class="title">
                    您累计购买了<span class="number">{{totalNumber}}</span>个资源
                </h4>
                <Tabs @on-click="getData">
                    <TabPane label="运输资源">
                        <Item :parent="parent" :items="itemsData" :conveyanceStatus="showCondition"></Item>
                    </TabPane>
                    <TabPane label="仓储资源">
                        <Item :parent="parent" :items="itemsData" :warehouseStatus="showCondition"></Item>
                    </TabPane>
                </Tabs>
            </Col>
        </Row>
    </div>
</template>

<script>
    import UserAPI from '../../../api/user.js';
    import Item from './item.vue';
    export default {
        data() {
            return {
                userId: 0,
                parent: 'order',
                showCondition: true,
                type: 'conveyance',
                itemsData: []
            }
        },
        /**
         * 获取数据
         */
        mounted: function() {
            let _this = this;
            setTimeout(function(){
                _this.userId = _this.$store.state.user.userInfo.userId;
                _this.getConveyanceOrderData();
            }, 10);
        },
        computed: {
            totalNumber: function () {
                return this.itemsData.length;
            }
        },
        methods: {
            /**
             * 获取数据
             */
            getData: function (name) {
                if(name === 0) {
                    this.getConveyanceOrderData();
                } else if(name === 1) {
                    this.getWarehouseOrderData();
                }
            },
            /**
             * 获取运输订单数据
             */
            getConveyanceOrderData: function () {
                let _this = this;
                UserAPI.getConveyanceOrderData(this.userId).then( function(response){
                    if(response.data.errCode === 0) {
                        _this.itemsData = response.data.data;
                    } else {
                        _this.$Message.warning(response.data.data);
                    }
                }).catch( function(){
                    _this.$Message.warning('请求出现异常');
                });
            },
            /**
             * 获取仓储订单数据
             */
            getWarehouseOrderData: function () {
                let _this = this;
                UserAPI.getWarehouseOrderData(this.userId).then( function(response){
                    if(response.data.errCode === 0) {
                        _this.itemsData = response.data.data;
                    } else {
                        _this.$Message.warning(response.data.data);
                    }
                }).catch( function(){
                    _this.$Message.warning('请求出现异常');
                });
            }
        },
        components: {
            Item
        }
    }
</script>