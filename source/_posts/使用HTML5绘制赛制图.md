title: 使用Canvas绘制赛制图
date: 2016-01-11 10:51:17
tags:
---
　　因工作需要，要做一个能够动态生成比赛赛制的对阵图，花了一天的时间，结合H5的Canvas实现了一下，不多说，直接上代码


	<html>
	<head>
		<title>
			<meta http-equiv="content-type" content="text/html;charset=utf-8">
		</title>
	 <script type="text/javascript">

		var width =100; // 格子宽度
    	var height= 30; // 格子高度
    	var splitWidth = 120; // 间隔宽度
		var splitHeight =5; // 间隔高度 
		var locationArr=new Array(); // 记录前一轮坐标

        function pageLoad(totalRows,colsIndex){
        	if(parseInt(totalRows)<=0){
        		return ;
        	}

	        var cans = document.getElementById('can');
        	var cans = can.getContext('2d');

            for(var i =1;i<=totalRows ;i++){
            	var x =width + colsIndex*splitWidth;
            	var y = (i)*height+(splitHeight)*i; 
            	// 绘制矩形边框
            	if(colsIndex>0){
            		y= GetY(i);
            		if(y>0){
        				// 斜线型
						// cans.moveTo(x,y+(height/2));
						// cans.lineTo(x-splitWidth+width,locationArr[2*i]+height/2);
						// cans.stroke();
						// cans.moveTo(x,y+(height/2));
						// cans.lineTo(x-splitWidth+width,locationArr[2*i-1]+height/2);
						// cans.stroke();

						// 直线型
						cans.moveTo(x,y+(height/2));
						cans.lineTo(x-splitWidth+width,y+(height/2));
						cans.stroke();

						cans.moveTo(x-splitWidth+width,locationArr[2*i] + height/2);
						cans.lineTo(x-splitWidth+width,locationArr[2*i-1] + height/2);
						cans.stroke();


            			cans.strokeText('x:'+ parseInt(width + colsIndex*splitWidth) + ' y:'+ parseInt(y) ,width+20+10 + colsIndex*splitWidth,y+20);
	  					cans.strokeRect(width + colsIndex*splitWidth ,y ,width,height);
	            		locationArr[i]=GetY(i);
            		}
				}else{
					// 绘制格子里的文字内容
					cans.strokeText('x:'+ parseInt(x) + ' y:'+ parseInt(y) ,width+30 + colsIndex*splitWidth,y+20);
					cans.strokeRect(x ,y,width,height);
					locationArr[i]= parseInt(y);
				}

            }
            colsIndex = colsIndex + 1
          	pageLoad(totalRows/2,colsIndex); // 递归法
        }

        function GetY(index)
        {
        	if(locationArr.length<1){
        		return 0;
        	}
        	var sum =0;
        	if(index==1){
        		for (var i=index;i <index+2; i++) {
	        		if(locationArr[i]!=null){
	        			sum = sum + locationArr[i];
	        		}
        		};
        	}else{
        		for (var i=2*index-1;i <2*index+1; i++) {
	        		if(locationArr[i]!=null){
	        			sum = sum + locationArr[i];
	        		}
        		};
        	}
        	return parseInt(sum/2);
        }
    </script>
	</head>
	<body onload="pageLoad(32,0)">
        <canvas id="can" width="1024px" height="960px"></canvas>
    </body>
	</html>


![](http://7xp62o.com1.z0.glb.clouddn.com/QQ%E5%9B%BE%E7%89%8720160112133704.png)
调用的pageLoad()第一个参数是对阵人数，第二个参数必须从0开始，是记录轮次的索引值。