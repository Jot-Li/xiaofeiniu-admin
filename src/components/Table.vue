<template>
    <div class="xfn-table-info">
        <el-card shadow="hover">
            <div class="xfn-table" :style="{background:getTableColor(data.status)}">
            {{data.tid}}号桌：{{data.status|tableStatus}}</div>
            <el-button type="success" plain size="mini"
            @click="showTableDetail">详情</el-button>
            <el-button type="danger" plain size="mini">修改</el-button>
        </el-card>

        <!-- 桌台详情对话框 -->
        <el-dialog :title="data.tid+'号桌台详情'"
        :visible="dialogTableDetailVisible"
        :before-close="closeDialogTableDetile">
            <!-- 对话框主体 -->
            <el-tabs @tab-click="makeQRCode" type="border-card">
                <el-tab-pane label="桌台状态">状态</el-tab-pane>
                <el-tab-pane label="桌台二维码">
                    <img :src="qrcodeData">
                </el-tab-pane>
            </el-tabs>
            <!-- 对话框尾部 -->
            <div slot="footer">
                <el-button type="primary"
                @click="dialogTableDetailVisible=false">确定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
export default {
    data(){
        return{
            dialogTableDetailVisible:false,
            qrcodeData:''  //二维码图片数据：Base64编码的字符串
        } 
    },
    props:['data'],
    methods: {
        getTableColor(status){
            if(status==1)return '#67c23a';
            else if(status==2) return '#e6a23c';
            else if(status==3) return '#f56c6c';
            else return '#909399';
        },
        showTableDetail(){
            this.dialogTableDetailVisible = true;
        },
        closeDialogTableDetile(){
            this.dialogTableDetailVisible = false;
        },
        makeQRCode(){
            //创建二维码——注意此方法不能再当前组件的mounted中调用，
            //因为绘图必须的canvas在el-dialog中，对话框加载时并不在DOM树上
            var qrcode = require('qrcode');
            var canvas = document.getElementById('qrcanvas');
            //每个桌子对应的URL应该形如：
            //http://127.0.0.1:8092/#/3
            var tableUrl = this.$store.state.globalSettings.appUrl +
            '/#/'+this.data.tid;
            //把回执得到的图片二进制数据转换为Base64编码的字符串
            qrcode.toDataURL(tableUrl, {
                width:300,
                errorCorrectionLevel: 'H'
            }, (err,url)=>{
                //console.log('二维码图片回执成功，数据如下：');
                //console.log(url);
                this.qrcodeData = url;
            })
        }
    }
}
</script>

<style lang="scss">
    .xfn-table-info {
        padding: 3px;
        text-align: center;
        .xfn-table {
            width: 90%;
            height: 120px;
            line-height: 120px;
            border: 1px solid #aaa;
            border-radius: 50%;
            box-shadow: 3px -4px 5px #666;
            margin: 5px auto;
        }
    }
</style>