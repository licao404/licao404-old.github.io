---
title: 学习 ECharts（一）：初窥
date: 2016-04-19 00:12:50
categories:
	- 数据可视化
tags:
	- 库
	- 图表库
	- ECharts
	- 可视化
---
{% fullimage http://7xrvo9.com1.z0.glb.clouddn.com/ECharts/example.png, alt,   %}

<blockquote class = "blockquote-center">ECharts，一个纯 Javascript 的图表库，可以流畅的运行在 PC 和移动设备上，兼容当前绝大部分浏览器（IE8/9/10/11，Chrome，Firefox，Safari等），底层依赖轻量级的 Canvas 类库 [ZRender][1]，提供直观，生动，可交互，可高度个性化定制的数据可视化图表。</blockquote>

<!--more-->
### 入门 Demo 01
>如下面 demo 展示，我们尝试插入一个简单的柱形图：

<div id="wrap0" style="width: 100%;height: 400px;"></div><!--step2：先为echarts准备有宽高的容器-->
<script src="http://7xrvo9.com1.z0.glb.clouddn.com/Echarts%E5%AE%8C%E6%95%B4%E7%89%88/echarts.min.js"></script><!--step1：引入 ECharts-->
<script type="text/javascript">
	//step3：基于准备好的dom，初始化echarts实例
	var myChart = echarts.init(document.getElementById('wrap0'));
	//step4：指定图表的配置项和数据
	var option = {
		title: {
			text: '开始学习ECharts'
		},
		tooltip: {},
		legend: {
			data: ['决定因素']
		},
		xAxis: {
			data: ["天赋","努力","兴趣","交流","心态","好学"]
		},
		yAxis: {
			// data: ['点数']
		},
        backgroundColor: '#fff',
		series: [{
			name: '决定因素',
			type: 'bar',
			data: [5, 20, 36, 10, 10, 20]
		}]

	}; 

	//step5: 使用刚指定的配置项和数据显示图表。
	myChart.setOption(option);
</script>

1. 下载 [ECharts][2] ，由于是初学，随便下什么版本。然后像普通引入外部 JS 文件一样引入 Echarts ;

2. 在 `body` 里面写一个具有宽高的容器，准备往里面塞 ECharts ：
```html
<div id="wrap" style="width: 100%;height: 400px;"></div>
```

3. 基于准备好的 `dom` ，用 `echarts.init()` 初始化一个 ECharts 实例:
```javascript
var myChart = echarts.init(document.getElementById('wrap'));
```

4. 指定图表配置和数据,具体见 [配置项手册][4]：
```javascript
var option = {
	//......
};
```
5. 通过 [setOption][3] 方法生成一个简单的柱状图，下面是完整代码。


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>demo1</title>
</head>
<body>
	<div id="wrap1" style="width: 100%;height: 400px;"></div>
	<!--step2：先为echarts准备有宽高的容器-->
    <script src="echarts.min.js"></script>
    <!--step1：引入 ECharts,src里面写你放ECharts的JS文件地址，可以是url-->
	<script type="text/javascript">
		//step3：基于准备好的dom，初始化echarts实例
		var myChart = echarts.init(document.getElementById('wrap1'));
		//step4：指定图表的配置项和数据
		var option = {
			title: {
				text: '开始学习ECharts'
			},
			tooltip: {},
			legend: {
				data: ['决定因素']
			},
			xAxis: {
				data: ["天赋","努力","兴趣","交流","心态","好学"]
			},
			yAxis: {
				// data: ['点数']
			},
            backgroundColor: '#91C7AE',
			series: [{
				name: '决定因素',
				type: 'bar',
				data: [5, 20, 36, 10, 10, 20]
			}]
		}; 
		//step5: 使用刚指定的配置项和数据显示图表。
		myChart.setOption(option);
	</script>
</body>
</html>
```

### 入门 Demo 02
<div id="wrap2" style="width: 100%;height: 400px;"></div><!--step2：先为echarts准备有宽高的容器-->
<!-- <script src="echarts.min.js"></script> -->
<script type="text/javascript">
	var myChart = echarts.init(document.getElementById('wrap2'));
	myChart.setOption({
	    series : [
	        {
	            name: '访问来源',
	            type: 'pie',
	            radius: '55%',
	            roseType: 'angle',
	            data:[
	                {value:235, name:'视频广告'},
	                {value:274, name:'联盟广告'},
	                {value:310, name:'邮件营销'},
	                {value:335, name:'直接访问'},
	                {value:400, name:'搜索引擎'}
	            ]
	        }
	    ]
})
</script>

>看看上面的demo，我们尝试画一个进化的饼图 —— 南丁格尔图

1. 先画一个饼图：
```javascript
var myChart = echarts.init(document.getElementById('wrap2'));
myChart.setOption({
    series : [
        {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            data:[
                {value:400, name:'搜索引擎'},
                {value:335, name:'直接访问'},
                {value:310, name:'邮件营销'},
                {value:274, name:'联盟广告'},
                {value:235, name:'视频广告'}
            ]
        }
    ]
})
```

2. 这里 `data` 属性值不像入门教程里那样每一项都是单个数值，而是一个包含 `name` 和 `value` 属性的对象，ECharts 中的数据项都是既可以只设成数值，也可以设成一个包含有名称、该数据图形的样式配置、标签配置的对象，具体见 [data][5] 文档。
- ECharts 中的饼图也支持通过设置 [roseType][6] 显示成南丁格尔图。
```javascript
roseType: 'angle'
```
- 南丁格尔图会通过半径表示数据的大小。

>还不够漂亮？试试美化一下吧

添加阴影，使用到 [itemStyle][7]:
```javascript
roseType: 'angle',//饼图转换成南丁格尔图
//添加阴影
itemStyle: {
    	normal: {
        // 阴影的大小
        shadowBlur: 200,
        // 阴影水平方向上的偏移
        shadowOffsetX: 0,
        // 阴影垂直方向上的偏移
        shadowOffsetY: 0,
        // 阴影颜色
        shadowColor: 'rgba(0, 0, 0, 0.5)'
    }
}
```

<div id="wrap3" style="width: 100%;height: 400px;"></div><!--step2：先为echarts准备有宽高的容器-->
<script src="echarts.min.js"></script>
<script type="text/javascript">
	var myChart = echarts.init(document.getElementById('wrap3'));
	myChart.setOption({
	    series : [
	        {
	            name: '访问来源',
	            type: 'pie',
	            radius: '55%',
	            data:[
	                {value:235, name:'视频广告'},
	                {value:274, name:'联盟广告'},
	                {value:310, name:'邮件营销'},
	                {value:335, name:'直接访问'},
	                {value:400, name:'搜索引擎'}
	            ],
	            roseType: 'angle',//饼图转换成南丁格尔图
	            //添加阴影
	            itemStyle: {
				    	normal: {
				        // 阴影的大小
				        shadowBlur: 200,
				        // 阴影水平方向上的偏移
				        shadowOffsetX: 0,
				        // 阴影垂直方向上的偏移
				        shadowOffsetY: 0,
				        // 阴影颜色
				        shadowColor: 'rgba(0, 0, 0, 0.5)'
				    }
				}
	        }
	    ]
})
</script>

`itemStyle` 都会有 `normal` 和 `emphasis` 两个选项，`normal` 选项是正常展示下的样式，`emphasis` 是鼠标 `hover` 时候的高亮样式。这个示例里是正常的样式下加阴影，但是可能更多的时候是 `hover` 的时候通过阴影突出。

```javascript
itemStyle: {
    emphasis: {
        shadowBlur: 200,
        shadowColor: 'rgba(0, 0, 0, 0.5)'
    }
}
```

<div id="wrap4" style="width: 100%;height: 400px;"></div><!--step2：先为echarts准备有宽高的容器-->
<script src="echarts.min.js"></script>
<script type="text/javascript">
	var myChart = echarts.init(document.getElementById('wrap4'));
	myChart.setOption({
	    series : [
	        {
	            name: '访问来源',
	            type: 'pie',
	            radius: '55%',
	            data:[
	                {value:235, name:'视频广告'},
	                {value:274, name:'联盟广告'},
	                {value:310, name:'邮件营销'},
	                {value:335, name:'直接访问'},
	                {value:400, name:'搜索引擎'}
	            ],
	            roseType: 'angle',//饼图转换成南丁格尔图
	            //添加阴影
				itemStyle: {
				    emphasis: {
				        shadowBlur: 200,
				        shadowColor: 'rgba(0, 0, 0, 0.5)'
				    }
				}
	        }
	    ]
})
</script>

>设置深色背景和浅色标签

1. 背景色是全局的，所以直接在 `option` 下设置 [backgroundColor][8] :
```javascript
setOption({
    backgroundColor: '#2c343c'
})
```

2. 文本的样式可以设置全局的 [textStyle][9]
```javascript
setOption({
    textStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
    }
})
```

3. 也可以每个系列分别设置，每个系列的文本设置在 [label.normal.textStyle][10]
```javascript
label: {
    normal: {
        textStyle: {
            color: 'rgba(255, 255, 255, 0.3)'
        }
    }
}
```
4. 饼图的话还要将标签的视觉引导线的颜色设为浅色。
```javascript
labelLine: {
    normal: {
        lineStyle: {
            color: 'rgba(255, 255, 255, 0.3)'
        }
    }
}
```

<div id="wrap5" style="width: 100%;height: 400px;"></div>
<!--step2：先为echarts准备有宽高的容器-->
<script src="echarts.min.js"></script>
<script type="text/javascript">
	var myChart = echarts.init(document.getElementById('wrap5'));
	myChart.setOption({
		// // 背景色是全局的，所以直接在 option 下设置 backgroundColor
		backgroundColor: '#2c343c',
		// // 文本的样式可以设置全局的 textStyle
	    // textStyle: {
	    //     color: 'rgba(255, 255, 255, 0.3)'
	    // },//
	    series : [
	        {
	            name: '访问来源',
	            type: 'pie',
	            radius: '55%',
	            data:[
	                {value:235, name:'视频广告'},
	                {value:274, name:'联盟广告'},
	                {value:310, name:'邮件营销'},
	                {value:335, name:'直接访问'},
	                {value:400, name:'搜索引擎'}
	            ],
	            roseType: 'angle',//饼图转换成南丁格尔图
	            // 添加阴影
	            itemStyle: {
				    	normal: {
				        // 阴影的大小
				        shadowBlur: 200,
				        // 阴影水平方向上的偏移
				        shadowOffsetX: 0,
				        // 阴影垂直方向上的偏移
				        shadowOffsetY: 0,
				        // 阴影颜色
				        shadowColor: 'rgba(0, 0, 0, 0.5)'
				    }
				},	
				// itemStyle: {
				//     emphasis: {
				//         shadowBlur: 200,
				//         shadowColor: 'rgba(0, 0, 0, 0.5)'
				//     }
				// }	
				

				//每个系列分别设置文本样式，每个系列的文本设置在 label.normal.textStyle				            
				label: {
	                normal: {
	                    textStyle: {
	                        color: 'rgba(255, 255, 255, 0.3)'
	                    }
	                }
	            },	

	            //饼图的话还要将标签的视觉引导线的颜色设为浅色。
			    labelLine: {
				    normal: {
				        lineStyle: {
				            color: 'rgba(255, 255, 255, 0.3)'
				        }
				    }
				}	   
	        }
	    ]
})
</script>

>设置扇形的颜色

1. 扇形的颜色也是在 itemStyle 中设置：
```javascript
itemStyle: {
    normal: {
        // 设置扇形的颜色
        color: '#c23531',
        shadowBlur: 200,
        shadowColor: 'rgba(0, 0, 0, 0.5)'
    }
}
```

<div id="wrap6" style="width: 100%;height: 400px;"></div>
<!--step2：先为echarts准备有宽高的容器-->
<script src="echarts.min.js"></script>
<script type="text/javascript">
	var myChart = echarts.init(document.getElementById('wrap6'));
	myChart.setOption({
		// // 背景色是全局的，所以直接在 option 下设置 backgroundColor
		backgroundColor: '#2c343c',
		// // 文本的样式可以设置全局的 textStyle
	    // textStyle: {
	    //     color: 'rgba(255, 255, 255, 0.3)'
	    // },//
	    series : [
	        {
	            name: '访问来源',
	            type: 'pie',
	            radius: '55%',
	            data:[
	                {value:235, name:'视频广告'},
	                {value:274, name:'联盟广告'},
	                {value:310, name:'邮件营销'},
	                {value:335, name:'直接访问'},
	                {value:400, name:'搜索引擎'}
	            ],
	            roseType: 'angle',//饼图转换成南丁格尔图
	            // 添加阴影
	            itemStyle: {
				    	normal: {
				        // 阴影的大小
				        shadowBlur: 200,
				        // 阴影水平方向上的偏移
				        shadowOffsetX: 0,
				        // 阴影垂直方向上的偏移
				        shadowOffsetY: 0,
				        // 阴影颜色
				        shadowColor: 'rgba(0, 0, 0, 0.5)',
				        // 设置扇形的颜色
				        color: '#c23531',
				        shadowBlur: 200,
				        shadowColor: 'rgba(0, 0, 0, 0.5)'				        
				    }
				},	
				// itemStyle: {
				//     emphasis: {
				//         shadowBlur: 200,
				//         shadowColor: 'rgba(0, 0, 0, 0.5)'
				//     }
				// }	
				

				//每个系列分别设置文本样式，每个系列的文本设置在 label.normal.textStyle				            
				label: {
	                normal: {
	                    textStyle: {
	                        color: 'rgba(255, 255, 255, 0.3)'
	                    }
	                }
	            },	

	            //饼图的话还要将标签的视觉引导线的颜色设为浅色。
			    labelLine: {
				    normal: {
				        lineStyle: {
				            color: 'rgba(255, 255, 255, 0.3)'
				        }
				    }
				}	   
	        }
	    ]
})
</script>

2. ECharts 中每个扇形颜色的可以通过分别设置 `data` 下的数据项实现。
```javascript
data: [{
    value:400,
    name:'搜索引擎',
    itemStyle: {
        normal: {
            color: 'c23531'
        }
    }
}, ...]
```

3. 但是这次因为只有明暗度的变化，所以有一种更快捷的方式是通过 [visualMap][11] 组件将数值的大小映射到明暗度。
```javascript
visualMap: {
    // 不显示 visualMap 组件，只用于明暗度的映射
    show: false,
    // 映射的最小值为 80
    min: 80,
    // 映射的最大值为 600
    max: 600,
    inRange: {
        // 明暗度的范围是 0 到 1
        colorLightness: [0, 1]
    }
}
```

<div id="wrap7" style="width: 100%;height: 400px;"></div>
<!--step2：先为echarts准备有宽高的容器-->
<script src="echarts.min.js"></script>
<script type="text/javascript">
	var myChart = echarts.init(document.getElementById('wrap7'));
	myChart.setOption({
		// // 背景色是全局的，所以直接在 option 下设置 backgroundColor
		backgroundColor: '#2c343c',
		// // 文本的样式可以设置全局的 textStyle
	    // textStyle: {
	    //     color: 'rgba(255, 255, 255, 0.3)'
	    // },//
	    
		visualMap: {
		    // 不显示 visualMap 组件，只用于明暗度的映射
		    show: false,
		    // 映射的最小值为 80
		    min: 80,
		    // 映射的最大值为 600
		    max: 600,
		    inRange: {
		        // 明暗度的范围是 0 到 1
		        colorLightness: [0, 1]
		    }
		},

	    series : [
	        {
	            name: '访问来源',
	            type: 'pie',
	            radius: '55%',
	            data:[
	                {value:235, name:'视频广告'},
	                {value:274, name:'联盟广告'},
	                {value:310, name:'邮件营销'},
	                {value:335, name:'直接访问'},
	                {value:400, name:'搜索引擎'}
	            ],
	            roseType: 'angle',//饼图转换成南丁格尔图
	            // 添加阴影
	            itemStyle: {
				    	normal: {
				        // 阴影的大小
				        shadowBlur: 200,
				        // 阴影水平方向上的偏移
				        shadowOffsetX: 0,
				        // 阴影垂直方向上的偏移
				        shadowOffsetY: 0,
				        // 阴影颜色
				        shadowColor: 'rgba(0, 0, 0, 0.5)',
				        // 设置扇形的颜色
				        color: '#c23531',
				        shadowBlur: 200,
				        shadowColor: 'rgba(0, 0, 0, 0.5)'				        
				    }
				},	
				// itemStyle: {
				//     emphasis: {
				//         shadowBlur: 200,
				//         shadowColor: 'rgba(0, 0, 0, 0.5)'
				//     }
				// }	
				

				//每个系列分别设置文本样式，每个系列的文本设置在 label.normal.textStyle				            
				label: {
	                normal: {
	                    textStyle: {
	                        color: 'rgba(255, 255, 255, 0.3)'
	                    }
	                }
	            },	

	            //饼图的话还要将标签的视觉引导线的颜色设为浅色。
			    labelLine: {
				    normal: {
				        lineStyle: {
				            color: 'rgba(255, 255, 255, 0.3)'
				        }
				    }
				}	   
	        }
	    ]
})
</script>






---

[1]:https://github.com/ecomfe/zrender
[2]:http://echarts.baidu.com/download.html
[3]:http://echarts.baidu.com/api.html#echartsInstance.setOption
[4]:http://echarts.baidu.com/option.html#title
[5]:http://echarts.baidu.com/option.html#series-pie.data
[6]:http://echarts.baidu.com/option.html#series-pie.roseType
[7]:http://echarts.baidu.com/tutorial.html#series-pie.itemStyle
[8]:http://echarts.baidu.com/option.html#backgroundColor
[9]:http://echarts.baidu.com/option.html#textStyle
[10]:http://echarts.baidu.com/option.html#series-pie.label.normal.textStyle
[11]:http://echarts.baidu.com/tutorial.html#option.html#visualMap