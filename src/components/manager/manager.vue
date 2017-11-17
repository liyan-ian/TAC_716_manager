<template>
<div>
	<div class="background-wrapper">
		<div class="header-wrapper" @click="test">
			我是标题
		</div>
		<div class="body-wrapper">
			<div class="menu-wrapper">
				<div class="topo-button-wrapper" @click="creat">
					<div class="topo">点击生成拓扑图</div>
				</div>
				<div class="change-edge-wrapper" @click="change_edge">
					<div class="change-button">连线改变</div>
				</div>
				<div class="change-label-wrapper" @click="change_label">
					<div class="change-label">连线提示信息</div>
				</div>
				<div class="create-data-wrapper" @click="create_data">
					<div class="create-data">生成拓扑图</div>
				</div>
				<div class="create-data-wrapper" @click="socket">
					<div class="create-data">连接</div>
				</div>
			</div>
			<div class="content-wrapper">
				<div class="topo-wrapper">
					<div id="canvas" class="topo"></div>
				</div>
			</div>
		</div>
		<div class="content-wrapper">
			<div class="table">
				<table class="table-overview">
					<tr>
						<th>节点名称</th>
						<th>策略名</th>
						<th>度量结果</th>
					</tr>
					<tr>
						<td>
							{{trust_data.node_name}}
						</td>
						<td>
							{{trust_data.trust_policy}}
						</td>
						<td>
							{{trust_data.measure_value}}
						</td>
					</tr>
				</table>
				<table class="table-detail">
					<tr>
						<th>
							{{trust_data.hardware.measure_value}}
						</th>
						<th>
							名称
						</th>
						<th>
							domain
						</th>
						<th>
							level
						</th>
					</tr>
					<tr>
						<td rowspan= "3">
							hardware
						</td>
					</tr>
					<tr v-for="hardware in trust_data.hardware.layers">
						<td>
							{{hardware.layer_name}}
						</td>
						<td>
							{{hardware.trust_level.domain}}
						</td>
						<td>
							{{hardware.trust_level.level}}
						</td>
					</tr>
					<tr>
						<th>
							{{trust_data.system.measure_value}}
						</th>
						<th>
							名称
						</th>
						<th>
							domain
						</th>
						<th>
							level
						</th>
					</tr>
					<tr>
						<td rowspan= "5">
							hardware
						</td>
					</tr>
					<tr v-for="system in trust_data.system.layers">
						<td>
							{{system.layer_name}}
						</td>
						<td>
							{{system.trust_level.domain}}
						</td>
						<td>
							{{system.trust_level.level}}
						</td>
					</tr>
				</table>
			</div>
		</div>
	</div>
</div>
</template>

<script type="text/esmascript-6">
export default {
	data() {
		return{
			edge1: "",
			edge2: "",
			edge3: "",
			edge4: "",
			edge5: "",
			edge6: "",
			edge7: "",
			edge8: "",
			wired_device3: "",
			color1: "#88AAEE",
			graph: "",
			layouter: "",
			con_node_array: [],
			con_node_name: [],
			con_edge_array: [],
			con_data: {
						"node_num":3,
						"nodes":[
							{
								"name":"trust_server",
								"types":"SERVER"
							},      
							{
								"name":"trust_client01",
								"type":"CLIENT"
							},
							{
								"name":"trust_client02",
								"type":"CLIENT"
							},
							{
								"name":"trust_client03",
								"type":"CLIENT"
							},
							{
								"name":"trust_switch",
								"type":"switch"
							}],
						"connection":[

							{
								"type":"Ethernet",
								"peers":[
									"trust_switch",
									"trust_server"
								]       
							},
							{
								"type":"Ethernet",
								"peers":[
									"trust_client02",
									"trust_switch"
								]       
							},
							{
								"type":"Ethernet",
								"peers":[
									"trust_client03",
									"trust_switch"
								]       
							}
						]       
					},
			trust_data: {
				"node_name": "trust_server",
				"trust_policy": "Server_policy",
				"measure_value": "这里显示度量结果",
				"hardware": {
					"measure_value": "这里显示hardware的度量结果",
					"layers": [{
						"layer_name": "Dell PRECISON 490",
						"trust_level": {
							"domain": "Hardware",
							"level": 1
						}
					},{
						"layer_name": "TPCM_V0.1",
						"trust_level": {
							"domain": "Trust",
							"level": 1
						}
					}]
				},
				"system": {
					"measure_value": "这里显示system的度量结果",
					"layers": [{
						 "layer_name":"Ubuntu 16.04.2 LTS",
						 "trust_level": {
						 	"domain":"System",
						 	"level":1
						}
					},{
						"layer_name": "cube frame 1.3",
						"trust_level": {
							"domain":"Trust",
							"level":1
						}
					},{
						"layer_name": "TAC-716-server-v0.9",
						"trust_level": {
							"domain":"Trust",
							"level":2
						}
					},{
						"layer_name": "trust server app",
						"trust_level": {
							"domain":"System",
							"level":2
						}
					}]
				}
			},
			wsock: "",
			PM: "",
			AC: "",
			AR1: "",
			AR2: "",
			PM_INFO: "",
			AC_INFO: "",
			AR1_INFO: "",
			AR2_INFO: ""
		};
	},
	methods: {
		creat() {
			// let graph = new Q.Graph('canvas');
			// let manager_center = graph.createNode("Manager Center", 0, -100);
			// let wired_switch = graph.createNode("Wired Network", -100, -50);
			// let wireless_switch = graph.createNode("Wireless Network", 100, -50);
			// let wired_device1 = graph.createNode("wired1", -170, 50);
			// let wired_device2 = graph.createNode("wired2", -100, 50);
			// this.wired_device3 = graph.createNode("wired3", -30, 50);
			// let wireless_device1 = graph.createNode("wireless1", 30, 50);
			// let wireless_device2 = graph.createNode("wireless2", 100, 50);
			// let wireless_device3 = graph.createNode("wireless3", 170, 50);
			// manager_center.image = 'Q-server';
			// wired_switch.image = 'Q-exchanger2';
			// wireless_switch.image = 'Q-exchanger2';
			// this.edge1 = graph.createEdge(manager_center,wired_switch,"");
			// this.edge1.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge1.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge1.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge2 = graph.createEdge(manager_center,wireless_switch,"");
			// this.edge2.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge2.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge2.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge3 = graph.createEdge(wired_switch,wired_device1,"");
			// this.edge3.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge3.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge3.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge4 = graph.createEdge(wired_switch,wired_device2,"");
			// this.edge4.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge4.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge4.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge5 = graph.createEdge(wired_switch,this.wired_device3,"");
			// this.edge5.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge5.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge5.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge6 = graph.createEdge(wireless_switch,wireless_device1,"");
			// this.edge6.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge6.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge6.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge7 = graph.createEdge(wireless_switch,wireless_device2,"");
			// this.edge7.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge7.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge7.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);
			// this.edge8 = graph.createEdge(wireless_switch,wireless_device3,"");
			// this.edge8.setStyle(Q.Styles.EDGE_COLOR, this.color1);
			// this.edge8.setStyle(Q.Styles.EDGE_LINE_DASH, [2, 1]);
			// this.edge8.setStyle(Q.Styles.ARROW_TO_LINE_DASH, [2, 1]);

			// var group = graph.createGroup("G");
			// group.addChild(wired_device1);
			// group.addChild(wired_device2);
			// group.setStyle(Q.Styles.GROUP_BACKGROUND_COLOR, Q.toColor(0xCCfcfb9b));
			// group.setStyle(Q.Styles.GROUP_BACKGROUND_GRADIENT, Q.Gradient.LINEAR_GRADIENT_HORIZONTAL);
			// group.setStyle(Q.Styles.GROUP_STROKE, 2);
			// group.setStyle(Q.Styles.GROUP_STROKE_STYLE, "#2898E0");
			// group.setStyle(Q.Styles.GROUP_STROKE_LINE_DASH, [3,2]);

			// var label2 = new Q.LabelUI();
			// label2.position = Q.Position.CENTER_TOP;
			// label2.anchorPosition = Q.Position.CENTER_BOTTOM;
			// label2.border = 1;
			// label2.padding = new Q.Insets(2, 5);
			// label2.showPointer = true;
			// label2.offsetY = -10;
			// label2.backgroundColor = "yellow";
			// label2.fontSize = 16;
			// label2.fontStyle = "italic 100";
			// this.wired_device3.addUI(label2, [{
			// 		property : "label2",
			// 		propertyType : Q.Consts.PROPERTY_TYPE_CLIENT,
			// 		bindingProperty : "data"
			// 	}, {
			// 		property : "label2.color",
			// 		propertyType : Q.Consts.PROPERTY_TYPE_CLIENT,
			// 		bindingProperty : "color"
			// 	}]);
			// this.wired_device3.set("label2", "another label");
			// this.wired_device3.set("label2.color", "#333");
			// this.wired_device3.set("label2","不安全");
			let self=this;
			self.graph = new Q.Graph('canvas');
			self.PM = self.graph.createNode("PM", -100, -200);
			self.PM.image = 'Q-server';
			var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
			var shape = new Q.ImageUI(circle);
			shape.position = Q.Position.CENTER_MIDDLE;
			shape.layoutByAnchorPoint = false;
			shape.name = "A";
			shape.lineWidth = 1;
			shape.strokeStyle = "#000";
			shape.fillColor = Q.toColor(0xCCFFFF00);
			shape.zIndex = -1;
			self.PM.addUI(shape);



			self.AC = self.graph.createNode("AC", -100, -100);
			self.AC.image = 'Q-exchanger2';
			var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
			var shape = new Q.ImageUI(circle);
			shape.position = Q.Position.CENTER_MIDDLE;
			shape.layoutByAnchorPoint = false;
			shape.name = "A";
			shape.lineWidth = 1;
			shape.strokeStyle = "#000";
			shape.fillColor = Q.toColor(0xFF0000);
			shape.zIndex = -1;
			self.AC.addUI(shape);
			self.edge1 = self.graph.createEdge(self.PM,self.AC,"");
		},
		change_edge() {
			this.edge1.setStyle(Q.Styles.EDGE_COLOR, "red");
			this.edge1.setStyle(Q.Styles.ARROW_TO, "red");
		},
		change_label() {
			this.wired_device3.set("label2","安全");
		},
		get_node(node_name){
			let node;
			let array_num = this.con_node_name.indexOf(node_name);
			node = this.con_node_array[array_num];
			return node;
		},
		create_data() {
			this.graph = new Q.Graph('canvas');
			this.layouter = new Q.TreeLayouter(this.graph);
			this.layouter.doLayout({x: 10, y: 10, byAnimate: false});
			//生成node节点
			for(let create_node=0;create_node<this.con_data.nodes.length;create_node++){
				this.con_node_array[create_node] = this.graph.createNode(this.con_data.nodes[create_node].name);
				this.con_node_name[create_node] = this.con_data.nodes[create_node].name;
			}

			//生成连接线
			for(let create_edge=0;create_edge<this.con_data.connection.length;create_edge++){
				this.con_edge_array[create_edge] = this.graph.createEdge(this.get_node(this.con_data.connection[create_edge].peers[0]),this.get_node(this.con_data.connection[create_edge].peers[1]));
			}
		},
		test() {
			this.graph.clear();
			this.con_data = {
						"node_num":3,
						"nodes":[
							{
								"name":"trust_server",
								"types":"SERVER"
							},      
							{
								"name":"trust_client01",
								"type":"CLIENT"
							},
							{
								"name":"trust_switch",
								"type":"switch"
							}],
						"connection":[
							{
								"type":"Ethernet",
								"peers":[
									"trust_client01",
									"trust_switch"
								]       
							}
						]       
					};
			this.layouter.doLayout({x: 10, y: 10, byAnimate: false});
			//生成node节点
			for(let create_node=0;create_node<this.con_data.nodes.length;create_node++){
				this.con_node_array[create_node] = this.graph.createNode(this.con_data.nodes[create_node].name);
				this.con_node_name[create_node] = this.con_data.nodes[create_node].name;
			}

			//生成连接线
			for(let create_edge=0;create_edge<this.con_data.connection.length;create_edge++){
				this.con_edge_array[create_edge] = this.graph.createEdge(this.get_node(this.con_data.connection[create_edge].peers[0]),this.get_node(this.con_data.connection[create_edge].peers[1]));
			}
		},
		socket() {
			let netaddr = document.getElementById("addr_id");
			let netport = document.getElementById("port_id");
			console.log(this.wsock);
			//this.wsock = new WebSocket('ws://'+netaddr.value+':'+netport.value, 'cube-wsport');
			this.wsock = new WebSocket('ws://192.168.1.50:15999', 'cube-wsport');
			let self=this;
			self.wsock.onopen = function(e) {
				alert("连接成功！");
				self.graph = new Q.Graph('canvas');
			};
			self.wsock.onclose = function(e) {
				alert("onclose");
			};
			self.wsock.onerror = function(e) {
				alert("error");
			};
			self.wsock.onmessage = function(e) {
				//alert("I have a message");
				let msg;
				msg = e.data;
				console.log(msg);
				if(msg.replace(/(^s*)|(s*$)/g, "").length != 0){
					msg = JSON.parse(msg);
					console.log(msg);
					if(msg.RECORD[0].type=="PM") {
						if(self.PM_INFO == "") {
							self.PM = self.graph.createNode("PM", -100, -200);
							self.PM.image = 'Q-server';
						}
						self.PM_INFO = msg.RECORD[0];
						var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
						var shape = new Q.ImageUI(circle);
						shape.position = Q.Position.CENTER_MIDDLE;
						shape.layoutByAnchorPoint = false;
						shape.name = "A";
						shape.lineWidth = 1;
						shape.strokeStyle = "#000";
						switch(self.PM_INFO.peer_state) {
							case "UNKNOWN":
								shape.fillColor = Q.toColor(0x808080);//灰色
								break;
							case "LOGIN":
								shape.fillColor = Q.toColor(0xFFFF00);//黄色
								break;
							case "LOGOUT":
								shape.fillColor = Q.toColor(0x3333FF);//蓝色
								break;
							case "TRUST":
								shape.fillColor = Q.toColor(0x66FF00);//绿色
								break;
							case "LOGFAIL":
								shape.fillColor = Q.toColor(0xFF9900);//橙色
								break;
							case "VERIFYFAIL":
								shape.fillColor = Q.toColor(0xFF0000);//红色
								break;
							default:
								shape.fillColor = Q.toColor(0xFFFFF);//白色
								break;
						}
						shape.zIndex = -1;
						self.PM.addUI(shape);
						console.log(self.PM_INFO);
					}
					else if(msg.RECORD[0].type=="AC") {
						if(self.AC_INFO == "") {
							self.AC = self.graph.createNode("AC", -100, -100);
							self.AC.image = 'Q-exchanger2';
							self.edge1 = self.graph.createEdge(self.PM,self.AC,"");
						}
						self.AC_INFO = msg.RECORD[0];
						var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
						var shape = new Q.ImageUI(circle);
						shape.position = Q.Position.CENTER_MIDDLE;
						shape.layoutByAnchorPoint = false;
						shape.name = "A";
						shape.lineWidth = 1;
						shape.strokeStyle = "#000";
						switch(self.AC_INFO.peer_state) {
							case "UNKNOWN":
								shape.fillColor = Q.toColor(0x808080);//灰色
								break;
							case "LOGIN":
								shape.fillColor = Q.toColor(0xFFFF00);//黄色
								break;
							case "LOGOUT":
								shape.fillColor = Q.toColor(0x3333FF);//蓝色
								break;
							case "TRUST":
								shape.fillColor = Q.toColor(0x66FF00);//绿色
								break;
							case "LOGFAIL":
								shape.fillColor = Q.toColor(0xFF9900);//橙色
								break;
							case "VERIFYFAIL":
								shape.fillColor = Q.toColor(0xFF0000);//红色
								break;
							default:
								shape.fillColor = Q.toColor(0xFFFFF);//白色
								break;
						}
						shape.zIndex = -1;
						self.AC.addUI(shape);
						console.log(self.AC_INFO);
					}
					else if(msg.RECORD[0].type=="AR") {
						if(self.AR1_INFO == "") {
							self.AR1_INFO = msg.RECORD[0];
							self.AR1 = self.graph.createNode("AR1", -150, 0);
							self.edge2 = self.graph.createEdge(self.AC,self.AR1,"");
							var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
							var shape = new Q.ImageUI(circle);
							shape.position = Q.Position.CENTER_MIDDLE;
							shape.layoutByAnchorPoint = false;
							shape.name = "A";
							shape.lineWidth = 1;
							shape.strokeStyle = "#000";
							switch(self.AR1_INFO.peer_state) {
								case "UNKNOWN":
									shape.fillColor = Q.toColor(0x808080);//灰色
									break;
								case "LOGIN":
									shape.fillColor = Q.toColor(0xFFFF00);//黄色
									break;
								case "LOGOUT":
									shape.fillColor = Q.toColor(0x3333FF);//蓝色
									break;
								case "TRUST":
									shape.fillColor = Q.toColor(0x66FF00);//绿色
									break;
								case "LOGFAIL":
									shape.fillColor = Q.toColor(0xFF9900);//橙色
									break;
								case "VERIFYFAIL":
									shape.fillColor = Q.toColor(0xFF0000);//红色
									break;
								default:
									shape.fillColor = Q.toColor(0xFFFFF);//白色
									break;
							}
							shape.zIndex = -1;
							self.AR1.addUI(shape);
							console.log("AR1");
						}
						if(msg.RECORD[0].machine_uuid === self.AR1_INFO.machine_uuid) {
							self.AR1_INFO = msg.RECORD[0];
							var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
							var shape = new Q.ImageUI(circle);
							shape.position = Q.Position.CENTER_MIDDLE;
							shape.layoutByAnchorPoint = false;
							shape.name = "A";
							shape.lineWidth = 1;
							shape.strokeStyle = "#000";
							switch(self.AR1_INFO.peer_state) {
								case "UNKNOWN":
									shape.fillColor = Q.toColor(0x808080);//灰色
									break;
								case "LOGIN":
									shape.fillColor = Q.toColor(0xFFFF00);//黄色
									break;
								case "LOGOUT":
									shape.fillColor = Q.toColor(0x3333FF);//蓝色
									break;
								case "TRUST":
									shape.fillColor = Q.toColor(0x66FF00);//绿色
									break;
								case "LOGFAIL":
									shape.fillColor = Q.toColor(0xFF9900);//橙色
									break;
								case "VERIFYFAIL":
									shape.fillColor = Q.toColor(0xFF0000);//红色
									break;
								default:
									shape.fillColor = Q.toColor(0xFFFFF);//白色
									break;
							}
							shape.zIndex = -1;
							self.AR1.addUI(shape);
						}
						if(self.AR1_INFO.machine_uuid != msg.RECORD[0].machine_uuid) {
							self.AR2_INFO = msg.RECORD[0];
							self.AR2 = self.graph.createNode("AR2", -50, 0);
							var circle = Q.Shapes.getShape(Q.Consts.SHAPE_CIRCLE, -30, -5, 60, 30);
							var shape = new Q.ImageUI(circle);
							shape.position = Q.Position.CENTER_MIDDLE;
							shape.layoutByAnchorPoint = false;
							shape.name = "A";
							shape.lineWidth = 1;
							shape.strokeStyle = "#000";
							switch(self.AR2_INFO.peer_state) {
								case "UNKNOWN":
									shape.fillColor = Q.toColor(0x808080);//灰色
									break;
								case "LOGIN":
									shape.fillColor = Q.toColor(0xFFFF00);//黄色
									break;
								case "LOGOUT":
									shape.fillColor = Q.toColor(0x3333FF);//蓝色
									break;
								case "TRUST":
									shape.fillColor = Q.toColor(0x66FF00);//绿色
									break;
								case "LOGFAIL":
									shape.fillColor = Q.toColor(0xFF9900);//橙色
									break;
								case "VERIFYFAIL":
									shape.fillColor = Q.toColor(0xFF0000);//红色
									break;
								default:
									shape.fillColor = Q.toColor(0xFFFFF);//白色
									break;
							}
							shape.zIndex = -1;
							self.AR2.addUI(shape);
							self.edge3 = self.graph.createEdge(self.AC,self.AR2,"");
							console.log("AR2");
						}
					}
				}
			};
		}
	}
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.background-wrapper
	position: absolute
	display: flex
	flex-direction: column
	width: 100%
	height: 100%
	background: #4A374A
	.header-wrapper
		position: relative
		flex: 0 0 100px
		height: 200px
		text-align: center
		color: #fff
		background: #666666
	.body-wrapper
		flex: 2
		display: flex
		flex-direction: row_reverse
		.menu-wrapper
			flex: 0 0 200px
			background: #99CCFF
			.topo-button-wrapper
				border: 1px solid black
				text-align: center
			.change-edge-wrapper
				border: 1px solid black
				text-align: center
			.change-label-wrapper
				border: 1px solid black
				text-align: center
			.create-data-wrapper
				border: 1px solid black
				text-align: center
		.content-wrapper
			flex: 1
			background: #ffffff
			.topo-wrapper
				height: 100%
				.topo
					height: 100%
	.content-wrapper
		flex: 1
		background: #fff
		.table
			.table-overview
				width: 100%
				align-items: center
				text-align: center
				border-collapse:collapse
				border: solid #000 1px
				& td
					border: solid #000 1px
				& th
					border: solid #000 1px
			.table-detail
				width: 100%
				align-items: center
				text-align: center
				border-collapse:collapse
				border: solid #000 1px
				& td
					border: solid #000 1px
				& th
					border: solid #000 1px
</style>
