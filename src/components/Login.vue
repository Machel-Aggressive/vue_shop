<template>
    <div class="login_container">
        <div class="login_box">
            <!-- 头像区域 -->
            <div class="avatar_box">
                <img src="@/assets/logo.png" alt="">
            </div>
            <!-- 表单区域 -->
            <el-form :model="form" ref="refForm" :rules="rules" label-width="80px" class="login_form">
                <el-form-item label="用户名" prop="username">
                    <el-input
                        placeholder="请输入用户名"
                        prefix-icon="el-icon-user"
                        v-model="form.username">
                    </el-input>
                </el-form-item>
                <el-form-item label="密码" prop="password">
                    <el-input
                        placeholder="请输入密码"
                        prefix-icon="el-icon-lock"
                        v-model="form.password"
                        show-password>
                    </el-input>
                </el-form-item>
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="reset">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data() {
      return {
        form: {
            username: 'admin',
            password: '123456'
        },
        rules: {
            username: [
                {required: true, message: '请输入用户名', trigger: 'blur'},
                {min: 3, max: 10, message: '用户名长度在3到10之间', trigger: 'blur'}
            ],
            password: [
                {required: true, message: '请输入密码', trigger: 'blur'},
                {min: 6, max: 10, message: '密码长度在6到10之间', trigger: 'blur'}
            ]
        }
      }
    },
    methods: {
        reset() {
            this.$refs.refForm.resetFields();
        },
        login() {
            this.$refs.refForm.validate(async valid => {
                if (!valid) return;
                const {data: result} = await this.$http.post('login', this.form);
                if (result.meta.status != 200) {
                    this.$message.error('登录失败');
                    return;
                }
                this.$message.success('登陆成功');
                window.localStorage.setItem('token', result.data.token);
                this.$router.push('/home');
            });
        }
    }
}
</script>

<style lang="less" scoped>
.login_container {
    background-color: #2b4b6b;
    height: 100%;
}
.login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .avatar_box {
        width: 130px;
        height: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #fff;
        img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
    }
}
.login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
}
.btns {
    display: flex;
    justify-content: flex-end;
}
</style>
