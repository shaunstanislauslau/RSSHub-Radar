<template>
    <div class="list">
        <el-main>
            <div class="title">规则列表</div>
            <div class="tip">
                <p>更多规则支持中，快来<a target="_blank" href="https://docs.rsshub.app/joinus/">参与我们</a>吧！</p>
                <p>{{ time }}前更新</p>
            </div>
            <div class="content" v-loading="loading">
                <el-collapse accordion>
                    <el-collapse-item v-for="(rule, domain) in rules" :key="domain" :title="rule._name + ' - ' + domain">
                        <div v-for="(subrule, subdomain) in rule" v-if="subdomain[0] !== '_'" :key="subdomain">
                            <p v-for="subsubrule in subrule" :key="subsubrule.title">
                                <a target="_blank" :href="subsubrule.docs">{{ subsubrule.title }}</a>
                            </p>
                        </div>
                    </el-collapse-item>
                </el-collapse>
            </div>
        </el-main>
    </div>
</template>

<script>
import { getRules, getRulesDate } from '../../common/rules';
import { secondToTime } from '../../common/utils';

export default {
    name: 'List',
    data: () => ({
        loading: true,
        rules: {},
        time: '',
    }),
    created() {
        getRulesDate((date) => {
            let second = (+new Date - +date) / 1000;
            this.time = secondToTime(second);
            this.refreshTime();
        });
        getRules((rules) => {
            this.rules = rules;
            this.loading = false;
        });
    },
    methods: {
        refreshTime() {
            getRulesDate((date) => {
                this.time = secondToTime((+new Date - +date) / 1000);
                setTimeout(() => {
                    this.refreshTime();
                }, 1000);
            });
        }
    }
}
</script>

<style lang="less" scoped>
a {
    text-decoration: none;
    color: #f5712c;
}

.list {
    width: 100%;
    max-width: 800px;
    margin: 0 auto;
    padding: 40px 10px;
}

.title {
    font-size: 25px;
    font-weight: bold;
    border-bottom: 1px solid #e6e6e6;
    padding-bottom: 10px;
    color: #f5712c;
}

.tip {
    font-size: 14px;
    margin: 20px 0;
    color: #555;
}

.content {
    margin-top: 20px;
    color: #222;
}
</style>
