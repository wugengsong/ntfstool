<template>
    <el-container class="al-main">
        <el-header class="rheader rheaderd">
            <div class="rheaderd_div">
                <!--偏好设置-->
                <span>{{$t('preferences')}}</span>
            </div>
            <div class="rheader_1">
                <div class="rheader_1_t" v-bind:class="{'tab_active':(select_block == 1)}" @click="chose_block(1)">
                    <div class="rheader_imgd"><img src="../assets/general.png"></div>
                    <!--常规-->
                    <span>{{$t('general')}}</span>
                </div>

                <div class="rheader_1_t" v-bind:class="{'tab_active':(select_block == 2)}" @click="chose_block(2)">
                    <div class="rheader_imgd"><img src="../assets/notification.png"></div>
                    <!--消息通知-->
                    <span>{{$t('notification')}}</span>
                </div>

                <!--<div class="rheader_1_t" v-bind:class="{'tab_active':(select_block == 3)}" @click="chose_block(3)">-->
                    <!--<div class="rheader_imgd"><img src="../assets/ignoredisk.png"></div>-->
                    <!--忽略磁盘-->
                    <!--<span>{{$t('ignoredisk')}}</span>-->
                <!--</div>-->

                <div class="rheader_1_t" v-bind:class="{'tab_active':(select_block == 4)}" @click="chose_block(4)">
                    <div class="rheader_imgd"><img src="../assets/privacy.png"></div>
                    <!--隐私-->
                    <span>{{$t('privacy')}}</span>
                </div>


                <div class="rheader_1_t" v-bind:class="{'tab_active':(select_block == 5)}" @click="chose_block(5)">
                    <div class="rheader_imgd"><img src="../assets/update.png"></div>
                    <!--更新-->
                    <span>{{$t('update')}}</span>
                </div>

            </div>
        </el-header>


        <el-main class="lmain">
            <el-form class="main-from main-from_b1" v-if="select_block == 1">
                <div class="main-from_div">
                    <div class="main-from_div_1">
                        <div class="main-from_div_1_1">
                            <div class="mb10">
                                <span>AlNtfs for Mac {{$t('menu')}}</span>
                            </div>

                            <div class="main-from_div_1_1-div mb10">
                                <div class="main-from_div_1_1-div_1">
                                    <div style="display: flex;flex-direction: column;justify-content: center">
                                        <i  v-if="show_menu" class="iconfont icondot" style=" font-size: 10px;color: #67c23a">&#xe607;</i>
                                        <i  v-if="!show_menu" class="iconfont icondot" style=" font-size: 10px;color: orangered">&#xe607;</i>
                                    </div>

                                    <div style="display: flex;flex-direction: column;justify-content: center">
                                        <!--已启用 已关闭-->
                                        <span v-if="show_menu" class="main-from_div_1_1-div_1span" style="">{{$t('activated')}}</span>
                                        <span v-if="!show_menu" class="main-from_div_1_1-div_1span" style="color: orangered">{{$t('closed')}}</span>
                                    </div>
                                </div>
                                <div>
                                    <div style="display: flex;flex-direction: column;justify-content: center">
                                        <el-button v-if="show_menu" @click="setToggleTrayMenu">{{$t('disable')}}</el-button>
                                        <el-button v-if="!show_menu" @click="setToggleTrayMenu">{{$t('enable')}}</el-button>
                                    </div>
                                </div>
                            </div>

                            <div class="font12 mb20">
                                <!--警告: 禁用后您将无法直接菜单栏操作卷宗-->
                                <span v-if="show_menu">{{$t('notice_cannot_do_disk01')}}</span>
                                <!--提示: 开启后将在右上角显示快捷操作菜单-->
                                <span v-if="!show_menu">{{$t('notice_cannot_do_disk02')}}</span>
                            </div>

                            <div style="position: absolute;bottom: 0;">
                                <!--跟随系统启动-->
                                <el-checkbox v-model="auto_run"  @change="changeAutoRun()"  :label="$t('Followthesystemstartup')" name="type"></el-checkbox>
                            </div>
                        </div>
                        <div class="main-from_div_1_2">
                            <div class="main-from_div_1_2_1">
                                <div class="main-from_div_1_2_1_div block">
                                    <el-carousel
                                            ref="carouselObj"
                                            trigger="click"
                                            :autoplay="false"
                                            indicator-position="none"
                                            arrow="never"
                                            height="200px">
                                        <el-carousel-item class="main-from_div_1_2_1_div_item" v-for="item in 4"
                                                          :key="item">
                                            <img style="height: 100%;" src="../assets/theme2.png">
                                        </el-carousel-item>
                                    </el-carousel>
                                </div>
                            </div>
                            <el-form-item class="main-from_div_1_2_2" :label="$t('theme')">
                                <el-radio-group v-model="theme" @change="changeTheme()">
                                    <el-radio label="0" disabled>{{$t('system')}}</el-radio>
                                    <el-radio label="1" disabled>{{$t('dark')}}</el-radio>
                                    <el-radio label="2">{{$t('light')}}</el-radio>
                                </el-radio-group>
                            </el-form-item>
                        </div>
                    </div>
                </div>
                <div class="main-from_div2">
                    <div class="main-from_div2_1">
                        <table class="block1_table">
                            <tr>
                                <td><span class="mr20">{{$t('lang_select')}}</span></td>
                                <td>
                                    <select class="al_select al_select_lang"  v-model="lang" @change="changeLang()">
                                        <option
                                                v-for="item, index in this.lang_list" :key="item.text"  :value="item.val">{{item.text}}
                                        </option>
                                    </select>
                                </td>
                            </tr>

                            <tr>
                                <!--如何处理安装坏卷-->
                                <td><span class="mr20">{{$t('Howtodealwithmountingbadvolumes')}}</span></td>
                                <td>
                                    <select class="al_select al_select_lang" v-model="install_bug_type" @change="changeInstallBugType()">
                                        <!--自动处理-->
                                        <option value="auto_solve">{{$t('Automaticprocessing')}}</option>
                                        <option value="tip_solve">{{$t('Promptbeforeprocessing')}}</option>
                                        <option value="no_solve">{{$t('Donothing')}}</option>
                                    </select>
                                </td>
                            </tr>

                            <tr>
                                <!--如何处理休眠-->
                                <td><span class="mr20">{{$t('Howtodealwithhibernation')}} Windows</span></td>
                                <td>
                                    <select class="al_select al_select_lang" v-model="how_restart_window" @change="changeHowRestartWindow()">
                                        <option value="change_to_bacground">{{$t('Switchtobackground')}}</option>
                                        <option value="tip_solve">{{$t('Askhow')}}</option>
                                        <option value="auto_close">{{$t('Autoclose')}}</option>
                                    </select>
                                </td>
                            </tr>
                        </table>
                    </div>

                </div>
            </el-form>


            <el-form class="main-from" v-if="select_block == 2">

                <el-form-item :label="$t('notice') + ':'" class="mb20">
                    <!--挂载和推出时显示通知-->
                    <el-checkbox v-model="mount_show_msg"  @change="changeMountShowMsg()"  class="mb20" :label="$t('Shownotificationswhenmountedandlaunched')" name="type"></el-checkbox>

                    <!--有更新时显示通知-->
                    <el-checkbox v-model="update_show_msg"  @change="changeUpdateShowMsg()"  :label="$t('Shownotificationswhenupdatesareavailable')" name="type"></el-checkbox>
                    <span class="sub_form_title">
                        <!--官方存在更新版本时显示通知-->
                        {{$t('Shownotificationswhenanupdatedversionisofficiallyavailable')}}
                    </span>
                    <div class="mb20"></div>
                    <!--磁盘卷宗存在异常时通知-->
                    <el-checkbox v-model="error_disk_msg"  @change="changeErrorDiskMsg()"  :label="$t('Notifywhendiskvolumeisabnormal')" name="type"></el-checkbox>
                    <span class="sub_form_title">
                        <!--磁盘卷宗可能因为异常断开造成数据异常-->
                        {{$t('Diskvolumemaybeabnormalduetoabnormaldisconnection')}}
                    </span>
                </el-form-item>
            </el-form>

            <el-form class="main-from" v-if="select_block == 3">
                <div style="margin: 20px 0">
                    <span>忽略列表里的磁盘将不再出现在磁盘列表</span>
                </div>


                <div>
                    <el-transfer
                            v-model="value"
                            :data="data"
                            :titles="['磁盘', '忽略列表']"
                    ></el-transfer>
                </div>
            </el-form>


            <el-form class="main-from main-from-b4" v-if="select_block == 4">
                <div>
                     <!--收集的所有数据均可在更新后的隐私政策中查看-->
                    <span> AlNtfs {{$t('Alldatacollectedcanbeviewedintheupdatedprivacypolicy')}}</span>
                </div>

                <div class="main-from_div_1_1-div_1" @click="openPrivacyUrl">
                    <div style="display: flex;flex-direction: column;justify-content: center">
                        <i class="iconfont iconjump06" style="font-size: 16px;color: black;">
                            &#xe648;
                            <!--阅读隐私政策            -->
                            <span style="    font-size: 12px;font-family: cursive;
    vertical-align: middle;
    margin-left: 3px;">{{$t('Readtheprivacypolicy')}}</span>
                        </i>
                    </div>
                </div>
            </el-form>

            <el-form class="main-from" v-if="select_block == 5">
                <el-form-item :label="$t('update') + ':'" class="mb20">
                    <el-checkbox  v-model="auto_check"  @change="changeAutoCheck()"  :label="$t('Checkforupdatesautomatically')" name="type"></el-checkbox>
                    <el-checkbox  v-model="auto_beta_update"  @change="changeAutoBetaUpdate()"  :label="$t('DetectBetaversionupdates')" name="type"></el-checkbox>
                    <span class="sub_form_title">
                        <!--请谨慎更新到测试版本,因为它们包含实验性的功能.这些功能不稳定,也可能造成数据丢失.-->
                        {{$t('Pleaseupdatetothebetaversion1')}}
                    </span>
                    <div>
                        <el-button @click="checkSoftUpdate">{{$t('Checkforupdates')}}</el-button>
                        <!--重置所有配置-->
                        <el-button @click="resetConf">{{$t('Resetallconfiguration')}}</el-button>
                    </div>


                    <div>
                        <!--上次检查时间-->
                        <span style="font-size: 12px;">{{$t('Lastchecktime')}} : 2020.2.12 14:36</span>
                    </div>
                </el-form-item>
            </el-form>
        </el-main>
    </el-container>
</template>
<script>
    import setting from '@/lib/setting.js'
    export default setting
</script>

<style scoped src="@/theme/setting.css"></style>

<style scoped>

</style>