<template>
	<div class="wrap">
		<div class="exp">
			<div class="title">实例</div>
			<div class="tip">请输入以下信息：</div>
			<rTelephone :attrs="config" :ref="config.name"></rTelephone>
			<rTelephone :attrs="config0" @oninput="oninput" @onclear="onclear" @onconfirm="onconfirm" :ref="config0.name"></rTelephone>
			<div class="btn" @click="doSubmit">提交</div>
		</div>		
		<div class="cb">
			<div class="cb0">思聪老公手机号输入时，触发事件的操作返回结果：</div>
			<div class="cb1" v-text="phone"></div>
		</div>
		<div class="cb">
			<div class="cb0">思聪老公手机号清空时，触发事件的操作返回结果：</div>
			<div class="cb1" v-text="txt"></div>
		</div>
	<div class="title">说明</div>
	<div class="content">
		<div class="row">组件参数继承rNumber组件，限定type=int,attrs里{maxlength:11,unit:""}</div>
	</div>
	<div class="title">示例代码</div>
	<textarea name="" id="ta" cols="50" rows="5">
		<rTelephone :attrs="config0" @oninput="oninput" @onclear="onclear" @onconfirm="onconfirm" :ref="config0.name"></rTelephone>
	</textarea>
	</div>
</template>
<script>
	export default{
		name: "rTelephoneExp",
		data(){
			return {
				phone: "",
				count: 0,
				txt: "",
				config: {
					title: "马云爸爸手机号码(不校验)",
					name: 'fatherPhone',
					value: "",
					placeholder: "请输入",
					disabled: false,
					readonly: false,
					unit: '',
					needVerify: false
				},
				config0: {
					title: "思聪老公的手机号码(键盘确定时校验)",
					name: 'sonPhone',
					value: "",
					placeholder: "请输入",
					disabled: false,
					readonly: false,
					unit: ''
				}
			}
		},
		created(){
			var self = this;
			setTimeout(function(){
				self.config.value = '1328888888';
				self.config.readonly = true;
			}, 2000)
		},
		methods:{
			oninput(val){
				this.phone = val;
			},
			onclear(){
				this.$toast({
					propsData: {
						message: '数据为空了，你可以把button置灰'
					}
				})
				this.txt = "执行清空操作次数："+ (++this.count);
			},
			onconfirm(code, codeStr, component){
				//this.$refs.sonPhone.verify();
				component.verify();
			},
			doSubmit(){
				let pass;
				for(var key in this.$refs){
					pass = this.$refs[key].verify();
					if(!pass){
						// this.$tip(this.$refs[key].attrs.title + "格式错误~");
						break;
					}
				}

				if(pass) {
					let s = [],
						sa = {};
					for(var key in this.$refs){
						s.push(this.$refs[key].getSerialize());
						Object.assign(sa,this.$refs[key].getSerializeArray());
					}
					this.$dialog({
						propsData: {
							message:"验证通过！ 序列化数据为："+s.join("&")
						},
						methods: {
							onConfirm: function(){
								this.remove();
								alert("序列化数组："+JSON.stringify(sa));
							}
						}
					});
				}
			}
		}
	}
</script>
<style lang="scss" scoped>
@function r($px){
	@return $px/18.75*1rem;
}
.cb{
	font-size: .75rem;
    margin: 1rem .8rem;
    line-height: 200%;
    .cb0{
    	color: #f66;
    }
}
#ta{
	color: #666;
	border: 1px solid #e5e5e5;
}
.tip{
	padding: .5rem .8rem;
	text-align: left;
	font-size: .8rem;
	color: #4080e8;
	background: rgb(245, 231, 185);
}
.btn{
	margin: r(50) auto;
	width: r(300);
	height: r(45);
	line-height: r(45);
	text-align: center;
	font-size: r(20);
	background: #4080e8;
	color: #fff;
	border-radius: 20px;
}	
</style>