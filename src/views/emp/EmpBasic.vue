<template>
    <div>
       基本资料
    </div>
</template>

<script>
    export default {
        name: "EmpBasic",
        data() {
            return {
                searchValue: {
                    politicId: null,
                    nationId: null,
                    jobLevelId: null,
                    posId: null,
                    engageForm: null,
                    departmentId: null,
                    beginDateScope: null
                },
                title: '',
                importDataBtnText: '导入数据',
                importDataBtnIcon: 'el-icon-upload2',
                importDataDisabled: false,
                showAdvanceSearchView: false,
                allDeps: [],
                emps: [],
                loading: false,
                popVisible: false,
                popVisible2: false,
                dialogVisible: false,
                total: 0,
                page: 1,
                keyword: '',
                size: 10,
                nations: [],
                joblevels: [],
                politicsstatus: [],
                positions: [],
                tiptopDegrees: ['本科', '大专', '硕士', '博士', '高中', '初中', '小学', '其他'],
                options: [{
                    value: '选项1',
                    label: '黄金糕'
                }, {
                    value: '选项2',
                    label: '双皮奶'
                }, {
                    value: '选项3',
                    label: '蚵仔煎'
                }, {
                    value: '选项4',
                    label: '龙须面'
                }, {
                    value: '选项5',
                    label: '北京烤鸭'
                }],
                inputDepName: '所属部门',
                emp: {
                    name: "javaboy",
                    gender: "男",
                    birthday: "1989-12-31",
                    idCard: "610122199001011256",
                    wedlock: "已婚",
                    nationId: 1,
                    nativePlace: "陕西",
                    politicId: 13,
                    email: "laowang@qq.com",
                    phone: "18565558897",
                    address: "深圳市南山区",
                    departmentId: null,
                    jobLevelId: 9,
                    posId: 29,
                    engageForm: "劳务合同",
                    tiptopDegree: "本科",
                    specialty: "信息管理与信息系统",
                    school: "深圳大学",
                    beginDate: "2017-12-31",
                    workState: "在职",
                    workID: "00000057",
                    contractTerm: 2,
                    conversionTime: "2018-03-31",
                    notworkDate: null,
                    beginContract: "2017-12-31",
                    endContract: "2019-12-31",
                    workAge: null
                },
                defaultProps: {
                    children: 'children',
                    label: 'name'
                },
                rules: {
                    name: [{required: true, message: '请输入用户名', trigger: 'blur'}],
                    gender: [{required: true, message: '请输入性别', trigger: 'blur'}],
                    birthday: [{required: true, message: '请输入出生日期', trigger: 'blur'}],
                    idCard: [{required: true, message: '请输入身份证号码', trigger: 'blur'}, {
                        pattern: /(^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$)|(^[1-9]\d{5}\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{2}$)/,
                        message: '身份证号码格式不正确',
                        trigger: 'blur'
                    }],
                    wedlock: [{required: true, message: '请输入婚姻状况', trigger: 'blur'}],
                    nationId: [{required: true, message: '请输入您组', trigger: 'blur'}],
                    nativePlace: [{required: true, message: '请输入籍贯', trigger: 'blur'}],
                    politicId: [{required: true, message: '请输入政治面貌', trigger: 'blur'}],
                    email: [{required: true, message: '请输入邮箱地址', trigger: 'blur'}, {
                        type: 'email',
                        message: '邮箱格式不正确',
                        trigger: 'blur'
                    }],
                    phone: [{required: true, message: '请输入电话号码', trigger: 'blur'}],
                    address: [{required: true, message: '请输入员工地址', trigger: 'blur'}],
                    departmentId: [{required: true, message: '请输入部门名称', trigger: 'blur'}],
                    jobLevelId: [{required: true, message: '请输入职称', trigger: 'blur'}],
                    posId: [{required: true, message: '请输入职位', trigger: 'blur'}],
                    engageForm: [{required: true, message: '请输入聘用形式', trigger: 'blur'}],
                    tiptopDegree: [{required: true, message: '请输入学历', trigger: 'blur'}],
                    specialty: [{required: true, message: '请输入专业', trigger: 'blur'}],
                    school: [{required: true, message: '请输入毕业院校', trigger: 'blur'}],
                    beginDate: [{required: true, message: '请输入入职日期', trigger: 'blur'}],
                    workState: [{required: true, message: '请输入工作状态', trigger: 'blur'}],
                    workID: [{required: true, message: '请输入工号', trigger: 'blur'}],
                    contractTerm: [{required: true, message: '请输入合同期限', trigger: 'blur'}],
                    conversionTime: [{required: true, message: '请输入转正日期', trigger: 'blur'}],
                    notworkDate: [{required: true, message: '请输入离职日期', trigger: 'blur'}],
                    beginContract: [{required: true, message: '请输入合同起始日期', trigger: 'blur'}],
                    endContract: [{required: true, message: '请输入合同结束日期', trigger: 'blur'}],
                    workAge: [{required: true, message: '请输入工龄', trigger: 'blur'}],
                }
            }
        },
        mounted() {
            this.initEmps();
            this.initData();
            this.initPositions();
        },
        methods: {
            searvhViewHandleNodeClick(data) {
                this.inputDepName = data.name;
                this.searchValue.departmentId = data.id;
                this.popVisible2 = !this.popVisible2
            },
            onError(err, file, fileList) {
                this.importDataBtnText = '导入数据';
                this.importDataBtnIcon = 'el-icon-upload2';
                this.importDataDisabled = false;
            },
            onSuccess(response, file, fileList) {
                this.importDataBtnText = '导入数据';
                this.importDataBtnIcon = 'el-icon-upload2';
                this.importDataDisabled = false;
                this.initEmps();
            },
            beforeUpload() {
                this.importDataBtnText = '正在导入';
                this.importDataBtnIcon = 'el-icon-loading';
                this.importDataDisabled = true;
            },
            exportData() {
                window.open('/employee/basic/export', '_parent');
            },
            emptyEmp() {
                this.emp = {
                    name: "",
                    gender: "",
                    birthday: "",
                    idCard: "",
                    wedlock: "",
                    nationId: 1,
                    nativePlace: "",
                    politicId: 13,
                    email: "",
                    phone: "",
                    address: "",
                    departmentId: null,
                    jobLevelId: 9,
                    posId: 29,
                    engageForm: "",
                    tiptopDegree: "",
                    specialty: "",
                    school: "",
                    beginDate: "",
                    workID: "",
                    contractTerm: 2,
                    conversionTime: "",
                    notworkDate: null,
                    beginContract: "",
                    endContract: "",
                    workAge: null
                }
                this.inputDepName = '';
            },
            showEditEmpView(data) {
                this.initPositions();
                this.title = '编辑员工信息';
                this.emp = data;
                this.inputDepName = data.department.name;
                this.dialogVisible = true;
            },
            deleteEmp(data) {
                this.$confirm('此操作将永久删除【' + data.name + '】, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    this.deleteRequest("/employee/basic/" + data.id).then(resp => {
                        if (resp) {
                            this.initEmps();
                        }
                    })
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消删除'
                    });
                });
            },
            doAddEmp() {
                if (this.emp.id) {
                    this.$refs['empForm'].validate(valid => {
                        if (valid) {
                            this.putRequest("/employee/basic/", this.emp).then(resp => {
                                if (resp) {
                                    this.dialogVisible = false;
                                    this.initEmps();
                                }
                            })
                        }
                    });
                } else {
                    this.$refs['empForm'].validate(valid => {
                        if (valid) {
                            this.postRequest("/employee/basic/", this.emp).then(resp => {
                                if (resp) {
                                    this.dialogVisible = false;
                                    this.initEmps();
                                }
                            })
                        }
                    });
                }
            },
            handleNodeClick(data) {
                this.inputDepName = data.name;
                this.emp.departmentId = data.id;
                this.popVisible = !this.popVisible
            },
            showDepView() {
                this.popVisible = !this.popVisible
            },
            showDepView2() {
                this.popVisible2 = !this.popVisible2
            },
            initPositions() {
                this.getRequest('/employee/basic/positions').then(resp => {
                    if (resp) {
                        this.positions = resp;
                    }
                })
            },
            getMaxWordID() {
                this.getRequest("/employee/basic/maxWorkID").then(resp => {
                    if (resp) {
                        this.emp.workID = resp.obj;
                    }
                })
            },
            initData() {
                if (!window.sessionStorage.getItem("nations")) {
                    this.getRequest('/employee/basic/nations').then(resp => {
                        if (resp) {
                            this.nations = resp;
                            window.sessionStorage.setItem("nations", JSON.stringify(resp));
                        }
                    })
                } else {
                    this.nations = JSON.parse(window.sessionStorage.getItem("nations"));
                }
                if (!window.sessionStorage.getItem("joblevels")) {
                    this.getRequest('/employee/basic/joblevels').then(resp => {
                        if (resp) {
                            this.joblevels = resp;
                            window.sessionStorage.setItem("joblevels", JSON.stringify(resp));
                        }
                    })
                } else {
                    this.joblevels = JSON.parse(window.sessionStorage.getItem("joblevels"));
                }
                if (!window.sessionStorage.getItem("politicsstatus")) {
                    this.getRequest('/employee/basic/politicsstatus').then(resp => {
                        if (resp) {
                            this.politicsstatus = resp;
                            window.sessionStorage.setItem("politicsstatus", JSON.stringify(resp));
                        }
                    })
                } else {
                    this.politicsstatus = JSON.parse(window.sessionStorage.getItem("politicsstatus"));
                }
                if (!window.sessionStorage.getItem("deps")) {
                    this.getRequest('/employee/basic/deps').then(resp => {
                        if (resp) {
                            this.allDeps = resp;
                            window.sessionStorage.setItem("deps", JSON.stringify(resp));
                        }
                    })
                } else {
                    this.allDeps = JSON.parse(window.sessionStorage.getItem("deps"));
                }
            },
            sizeChange(currentSize) {
                this.size = currentSize;
                this.initEmps();
            },
            currentChange(currentPage) {
                this.page = currentPage;
                this.initEmps();
            },
            showAddEmpView() {
                this.emptyEmp();
                this.title = '添加员工';
                this.getMaxWordID();
                this.dialogVisible = true;
            },
            initEmps(type) {
                this.loading = true;
                let url = '/employee/basic/?page=' + this.page + '&size=' + this.size;
                if (type && type == 'advanced') {
                    if (this.searchValue.politicId) {
                        url += '&politicId=' + this.searchValue.politicId;
                    }
                    if (this.searchValue.nationId) {
                        url += '&nationId=' + this.searchValue.nationId;
                    }
                    if (this.searchValue.jobLevelId) {
                        url += '&jobLevelId=' + this.searchValue.jobLevelId;
                    }
                    if (this.searchValue.posId) {
                        url += '&posId=' + this.searchValue.posId;
                    }
                    if (this.searchValue.engageForm) {
                        url += '&engageForm=' + this.searchValue.engageForm;
                    }
                    if (this.searchValue.departmentId) {
                        url += '&departmentId=' + this.searchValue.departmentId;
                    }
                    if (this.searchValue.beginDateScope) {
                        url += '&beginDateScope=' + this.searchValue.beginDateScope;
                    }
                } else {
                    url += "&name=" + this.keyword;
                }
                this.getRequest(url).then(resp => {
                    this.loading = false;
                    if (resp) {
                        this.emps = resp.data;
                        this.total = resp.total;
                    }
                });
            }
        }
    }
</script>

<style>
    /* 可以设置不同的进入和离开动画 */
    /* 设置持续时间和动画函数 */
    .slide-fade-enter-active {
        transition: all .8s ease;
    }

    .slide-fade-leave-active {
        transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }

    .slide-fade-enter, .slide-fade-leave-to
        /* .slide-fade-leave-active for below version 2.1.8 */
    {
        transform: translateX(10px);
        opacity: 0;
    }
</style>