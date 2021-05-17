<template>
	<el-container style="min-height: 100vh;">
		<el-header style="text-align: right; font-size: 20px;height: 50px;">
			<div class="headpic" style="float: left;padding-left: 60px;">
				<img src="../assets/pic1.png" style=" height: 50px; width: 70px; line-height: 10px;">
			</div>
			<div class="headpic" style="float: left;">
				<scan>环保应急管理系统</scan>
			</div>
			<div class="headpic" style="float: right">
				<el-button class="exit">退出登录</el-button>
				<div style="padding-right: 50px;padding-top: 7px;">
					<img src="../assets/13174508_13.jpeg" style=" height: 40px; width: 40px;border-radius: 25px;">
				</div>
			</div>
		</el-header>

		<el-container style="margin-left: 50px;margin-right: 50px;">
			<el-main>
				<el-tabs :tab-position="tabPosition">
					<el-tab-pane label="用户管理">
						<el-scorllbar>
							<el-row style="height: 20px;">
								<el-button style="background-color: green;margin-top: 5px;" @click="handleInsert()">新增</el-button>
								<el-dialog title="表单" :visible.sync="dialogVisible" width="30%">
									<el-form ref="formm" :model="form" label-width="80px" size="medium">
										<el-form-item prop="name" label="姓名" size="mini">
											<el-input v-model="form.name"></el-input>
										</el-form-item>
										<el-form-item prop="sex" label="性别" size="mini">
											<el-select :popper-append-to-body="false" v-model="form.sex" placeholder="请选择性别">
												<el-option label="男" value="男"></el-option>
												<el-option label="女" value="女"></el-option>
											</el-select>
										</el-form-item>
										<el-form-item prop="date" label="出生日期" size="mini">
											<el-date-picker type="date" placeholder="选择日期" v-model="form.date" style="width: 100%;" value-format="yyyyMMdd"></el-date-picker>
										</el-form-item>
										<el-form-item prop="num" label="账号" size="mini">
											<el-input v-model="form.num"></el-input>
										</el-form-item>
										<el-form-item prop="psw" label="密码" size="mini">
											<el-input v-model="form.psw"></el-input>
										</el-form-item>
										<el-form-item prop="type" label="会员类型" size="mini">
											<el-select v-model="form.type" placeholder="请选择类型">
												<el-option label="工作人员" value="工作人员"></el-option>
												<el-option label="指挥人员" value="指挥人员"></el-option>
												<el-option label="专家" value="专家"></el-option>
											</el-select>
										</el-form-item>
										<el-form-item prop="state" label="用户状态" size="mini">
											<el-select v-model="form.state" placeholder="请选择状态">
												<el-option label="有效" value="有效"></el-option>
												<el-option label="停用" value="停用"></el-option>
											</el-select>
										</el-form-item>
										<el-form-item>
											<el-button type="primary" style="margin-left: 50px;" @click="onSubmit('formm')">确 定</el-button>
										</el-form-item>
									</el-form>
								</el-dialog>
								<div style="float: right;">
									<el-button style="margin-top: auto;height: 40px;background-color:firebrick;color:white" @click="dialogVisible2 = true">精确搜索</el-button>
								</div>
								<el-dialog title="精确搜索" :visible.sync="dialogVisible2" width="30%">
									<el-form ref="form2" :model="form" label-width="80px" size="medium">
										<el-form-item prop="id" label="ID" size="mini">
											<el-input v-model="form.id"></el-input>
										</el-form-item>
										<el-form-item prop="name" label="姓名" size="mini">
											<el-input v-model="form.name"></el-input>
										</el-form-item>
										<el-form-item prop="num" label="账号" size="mini">
											<el-input v-model="form.num"></el-input>
										</el-form-item>
										<el-form-item prop="type" label="会员类型" size="mini">
											<el-select v-model="form.type" placeholder="请选择类型">
												<el-option label="工作人员" value="工作人员"></el-option>
												<el-option label="指挥人员" value="指挥人员"></el-option>
												<el-option label="专家" value="专家"></el-option>
											</el-select>
										</el-form-item>
										<el-form-item>
											<el-button type="primary" style="margin-left: 50px;" @click="handleSearch()">确 定</el-button>
										</el-form-item>
									</el-form>
								</el-dialog>
								<div style="float: right;">
									<el-button style="margin-top: auto;height: 40px;background-color:royalblue;color:white" @click="handleSingleSearch()">搜索</el-button>
								</div>
								<div style="float: right;">
									<el-input style="margin-top: auto;" placeholder="请输入内容" v-model="input1" clearable>
									</el-input>
								</div>
							</el-row>
							<el-table ref="infoTable" highlight-current-row @current-change="handleCurrentChange" style="background-color:transparent;"
							 height="210px" :data="tableData.slice((currentPage-1)*pageSize,currentPage*pageSize)">
								<el-table-column prop="id" label="ID">
								</el-table-column>
								<el-table-column prop="num" label="账号">
								</el-table-column>
								<el-table-column prop="psw" label="密码">
								</el-table-column>
								<el-table-column prop="name" label="姓名">
								</el-table-column>
								<el-table-column prop="sex" label="性别">
								</el-table-column>
								<el-table-column prop="birth" label="出生日期">
								</el-table-column>
								<el-table-column prop="type" label="会员类型">
								</el-table-column>
								<el-table-column prop="state" label="用户状态">
								</el-table-column>
								<el-table-column prop="do" label="操作">
									<template slot-scope="scope">
										<el-link slot="reference" size="small" style="color: red;" @click="handleDelete(scope.$index,scope.row)">删除</el-link>
										<el-link type="text" size="small" style="color: royalblue;" @click="handleCorrect(scope.$index,scope.row)">修改</el-link>
									</template>
								</el-table-column>
							</el-table>
						</el-scorllbar>
						<div style="float: left;margin-top: 15px;">
							<el-button style="color: black;" @click="all">显示全部</el-button>
						</div>
						<div class="block" style="float: right;margin-top: 10px;">
							<el-pagination :current-page.sync="currentPage" background="true" style="background-color: transparent;"
							 @current-change="currentChange" :total="total" :page-size="4" layout="prev, pager, next, jumper">
							</el-pagination>
						</div>
						<div class="userinfo" name="userinfo" style="margin-top: 50px;">
							<div class="Infobox" style="width: 150px;">
								<el-image style="width: 120px; height: 150px;float: right;" :src="url" :fit="fit">
								</el-image>
								<el-row>
									<div class="headpic" style="float: left;margin-top: 5px;"><label style="padding-top: 5px;">ID:</label><el-input class="infos" style="width: 150px;" size="mini"  v-model="id"></el-input></div>
								</el-row>
							</div>
							<div class="Infobox">
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">姓名：</label><el-input class="infos" style="width: 150px;" size="mini"  v-model="name"></el-input></div>
								</el-row>
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">性别：</label><el-input class="infos" style="width: 150px;" size="mini"  v-model="sex"></el-input></div>
								</el-row>
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">账号：</label><el-input class="infos" style="width: 150px;" size="mini" v-model="num"></el-input></div>
								</el-row>
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">出生日期：</label><el-input class="infos" style="width: 150px;" size="mini"  v-model="birth"></el-input></div>
								</el-row>
							</div>
							<div class="Infobox">
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">密码：</label><el-input class="infos" style="width: 150px;" size="mini" v-model="psw"></el-input></div>
								</el-row>
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">类型：</label><el-input class="infos" style="width: 150px;" size="mini" v-model="type"></el-input></div>
								</el-row>
								<el-row>
									<div class="headpic" ><label style="padding-top: 5px;">状态：</label><el-input class="infos" style="width: 150px;" size="mini"  v-model="state"></el-input></div>
								</el-row>
								<el-row style="float: right;">
									<el-button style="background-color: red;">删除</el-button>
									<el-button style="background-color:orange;">修改</el-button>
								</el-row>
							</div>
						</div>
					</el-tab-pane>
				</el-tabs>
			</el-main>
		</el-container>
	</el-container>
</template>
<style>
	.el-header {
		background-color: #fff2d7;
		color: #333;
		line-height: 60px;
	}

	.el-aside {
		color: #333;
	}

	.text {
		font-size: 12px;
	}

	.userinfo {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		border-radius: 4px;
		border: 1px solid #c0c0c0;
		height: 195px;
		width: 800px;
		margin: auto;
		box-shadow: 3px -3px 4px rgb(8 8 8 / 15%), 0 0 6px rgb(0 0 0 / 4%);
	}

	.headpic {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.Infobox {
		margin: 10px;
		height: 190px;
		width: 240px;
	}

	.el-row {
		margin-bottom: 25px;
	}

	.el-button {
		color: white;
		height: 35px;
		font-size: 10px;
		text-align: center;
		margin-top: -10px;
	}

	.el-input {
		height: 5px;
	}

	.exit {
		margin-top: 10px;
		background-color: #fff2d7;
		color: #ff7011;
		border: 0;
		font-weight: 600;

	}

	.exit:hover {
		background-color: #fff2d7;
		color: black;
	}

	.el-select__caret.el-input__icon.el-icon-arrow-up {
		margin-top: 25px;
	}

	.el-button.el-button--default.el-button--small {
		color: #000000;
	}

	.el-button.el-button--default.el-button--small.el-button--primary {
		color: #ffffff;
	}
	
	
</style>

<script>
	export default {
		data() {
			const item1 = {
				id: '1',
				num: '20185279',
				psw: 'yuanye1258',
				name: '袁野',
				sex: '男',
				birth: '20001111',
				type: '工作人员',
				state: '有效',
			},
			item2 = {
				id: '2',
				num: '20185102',
				psw: 'yuanye',
				name: '张志',
				sex: '男',
				birth: '20001111',
				type: '指挥人员',
				state: '有效',
			},
			item3 = {
				id: '3',
				num: '20195408',
				psw: 'xcvz',
				name: '张宇奇',
				sex: '男',
				birth: '19990131',
				type: '指挥人员',
				state: '有效',
			},
			item4 = {
				id: '4',
				num: '20172299',
				psw: 'xcvz00022',
				name: '张博',
				sex: '男',
				birth: '19991222',
				type: '指挥人员',
				state: '有效',
			},
			item5 = {
				id: '5',
				num: '20175408',
				name: '茹禹然',
				sex: '男',
				birth: '20000621',
				type: '专家',
				state: '有效',
			};
			return {
				form: {
					id:'',
					name: '',
					sex: '',
					date: '',
					num: '',
					psw: '',
					type: '',
					state: ''
				},
				nrow: {
					name: '',
					sex: '',
					date: '',
					num: '',
					psw: '',
					type: '',
					state: ''
				},
				currentrRow: null,
				currentid: 6,
				forminsert: true,
				visiblepop: false,
				dialogVisible: false,
				dialogVisible2: false,
				total: 5,
				pageSize: 4,
				currentPage: 1,
				tabPosition: 'left',
				baseData:[item1,item2,item3,item4,item5],
				tableData: [item1,item2,item3,item4,item5],
				fits: ['fill'],
				url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg',
				input1: '',
				name: '',
				sex: '',
				birth: '',
				num: '',
				psw: '',
				type: '',
				state: '',
				
				id:'',
				
			}
		},
		methods: {
			currentChange(currentPage) {
				this.currentPage = currentPage;
			},
			onSubmit(formm) {
				this.all();
				if (this.forminsert == true) {
					this.baseData.push({
						id: this.currentid,
						name: this.form.name,
						sex: this.form.sex,
						birth: this.form.date,
						num: this.form.num,
						psw: this.form.psw,
						type: this.form.type,
						state: this.form.state,

					});
					this.tableData = this.baseData;
					this.$message({
						message: '已成功添加新用户：' + this.form.name,
						type: 'success'
					});
					this.total++;
					this.currentid++;
				} else {
					this.nrow.name = this.form.name;
					this.nrow.sex = this.form.sex;
					this.nrow.birth = this.form.date;
					this.nrow.num = this.form.num;
					this.nrow.psw = this.form.psw;
					this.nrow.type = this.form.type;
					this.nrow.state = this.form.state;
					this.$message({
						message: '已成功修改用户信息：' + this.form.name,
						type: 'success'
					});
				};
				this.$refs[formm].resetFields();
				this.dialogVisible = false;


			},
			handleDelete(index, nrow) {
				this.$confirm("确定删除此用户？", '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					var i = 0;
					var index;
					for (i = 0; i < this.baseData.length; i++) {
						if (this.baseData[i].id == nrow.id) {
							index = i;
							break;
						}
					}
					this.baseData.splice(index, 1);
					this.tableData = this.baseData;
					this.$message({
						type: 'success',
						message: '删除成功',
					});
					this.all();
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			},
			handleCorrect(index, nrow) {
				this.form.name = nrow.name;
				this.form.sex = nrow.sex;
				this.form.date = nrow.birth;
				this.form.num = nrow.num;
				this.form.psw = nrow.psw;
				this.form.type = nrow.type;
				this.form.state = nrow.state;
				this.dialogVisible = true;
				this.forminsert = false;
				this.nrow = nrow;
			},
			handleInsert() {
				this.form.name = "";
				this.form.sex = "";
				this.form.date = "";
				this.form.num = "";
				this.form.psw = "";
				this.form.type = "";
				this.form.state = "";
				this.dialogVisible = true;
				this.forminsert = true;
			},
			handleSearch() {
				var i = 0;
				var j = 0;
				this.tableData = [];
				this.dialogVisible2 = true;
				for (i = 0; i < this.baseData.length; i++) {
					if ((this.form.id == this.baseData[i].id || this.form.id == "") && (this.form.name == this.baseData[i].name || this.form.name == "") &&
					(this.form.num == this.baseData[i].num || this.form.num == "") && (this.form.type == this.baseData[i].type ||this.form.type == "")) {
						this.tableData[j] = this.baseData[i];
						j++;
					}
				}
				this.currentPage = 1;
				this.total = j;
				this.dialogVisible2 = false;
			},
			all() {
				this.tableData = this.baseData;
				this.total = this.tableData.length;
			},
			handleSingleSearch() {
				var i = 0;
				var j = 0;
				this.tableData = [];
				for (i = 0; i < this.baseData.length; i++) {
					if (this.input1 == this.baseData[i].id || this.input1 == this.baseData[i].name || this.input1 == this.baseData[i].num ||
						this.input1 == this.baseData[i].type) {
						this.tableData[j] = this.baseData[i];
						j++;
					}
				}
				this.currentPage = 1;
				this.total = j;
			},
			handleCurrentChange(val) {
				this.name = val.name;
				this.psw = val.psw;
				this.num = val.num;
				this.sex = val.sex;
				this.birth = val.birth;
				this.id = val.id;
				this.type = val.type;
				this.state = val.state;
			}
		}
	};
</script>
