
<html>
<html lang="en">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>无标题文档</title>
    <style type="text/css">
        *{ padding:0; margin:0; list-style:none; border:0;}
        .all{
            width:400px;
            height:600px;
            padding:7px;
            border:1px solid #ccc;
            margin:100px auto;
            position:relative;
        }
        .screen{
            width:400px;
            height:600px;
            overflow:hidden;
            position:relative;
        }
        .screen li{ width:400px; height:600px; overflow:hidden; float:left;}
        .screen ul{ position:absolute; left:0; top:0px; width:3000px;}
        .all ol{ position:absolute; right:10px; bottom:10px; line-height:20px; text-align:center;}
        .all ol li{ float:left; width:20px; height:20px; background:#fff; border:1px solid #ccc; margin-left:10px; cursor:pointer;}
        .all ol li.current{ background:yellow;}
 
        #arr {display: none;}
        #arr span{ width:40px; height:40px; position:absolute; left:5px; top:50%; margin-top:-20px; background:#000; cursor:pointer; line-height:40px; text-align:center; font-weight:bold; font-family:'黑体'; font-size:30px; color:#fff; opacity:0.3; border:1px solid #fff;}
        #arr #right{right:5px; left:auto;}
    </style>
 
    <script>
        window.onload = function () {
            //需求：无缝滚动。
            //思路：赋值第一张图片放到ul的最后，然后当图片切换到第五张的时候
            //     直接切换第六章，再次从第一张切换到第二张的时候先瞬间切换到
            //     第一张图片，然后滑动到第二张
            //步骤：
            //1.获取事件源及相关元素。（老三步）
            //2.复制第一张图片所在的li,添加到ul的最后面。
            //3.给ol中添加li，ul中的个数-1个，并点亮第一个按钮。
            //4.鼠标放到ol的li上切换图片
            //5.添加定时器
            //6.左右切换图片（鼠标放上去隐藏，移开显示）
            var screen = document.getElementById("screen");
            var ul = screen.children[0];
            var ol = screen.children[1];
            var div = screen.children[2];
            var imgWidth = screen.offsetWidth;
 
            //2
            var tempLi = ul.children[0].cloneNode(true);
            ul.appendChild(tempLi);
            //3
            for(var i = 0; i < ul.children.length - 1; i++) {
                var newOlLi = document.createElement("li");
                newOlLi.innerHTML = i + 1;
                ol.appendChild(newOlLi);
            }
            var olLiArr = ol.children;
            olLiArr[0].className = "current";
            //4
            for(var i = 0, len = olLiArr.length; i < len; i++) {
                olLiArr[i].index = i;
                olLiArr[i].onmouseover = function (ev) {
                    for(var j = 0; j < len; j++) {
                        olLiArr[j].className = "";
                    }
                    this.className = "current";
                    key = square = this.index;
                    animate(ul, -this.index * imgWidth);
                }
            }
            //5
            var key = 0;
            var square = 0;
            var timer = setInterval(autoPlay, 5000);
            screen.onmouseover = function (ev) {
                clearInterval(timer);
                div.style.display = "block";
            }
            screen.onmouseout = function (ev) {
                timer = setInterval(autoPlay, 5000);
                div.style.display = "none";
            }
            //6
            var divArr = div.children;
            divArr[0].onclick = function (ev) {
                key--;
                if(key < 0) {
                    ul.style.left = -(ul.children.length-1) * imgWidth + "px";
                    key = 4;
                }
                animate(ul, -key * imgWidth);
                square--;
                if(square < 0) {
                    square = 4;
                }
                for(var k = 0; k < len; k++) {
                    olLiArr[k].className = "";
                }
                olLiArr[square].className = "current";
            }
            divArr[1].onclick = autoPlay;
            function autoPlay() {
                key++;
                //当不满足下面的条件是时候，轮播图到了最后一个孩子，进入条件中后，瞬移到
                //第一张，继续滚动。
                if(key > ul.children.length - 1) {
                    ul.style.left = 0;
                    key = 1;
                }
                animate(ul, -key * imgWidth);
                square++;
                if(square > 4) {
                    square = 0;
                }
                for(var k = 0; k < len; k++) {
                    olLiArr[k].className = "";
                }
                olLiArr[square].className = "current";
            }
            function animate(ele,target){
                    clearInterval(ele.timer);
                    var speed = target>ele.offsetLeft?10:-10;
                    ele.timer = setInterval(function () {
                        var val = target - ele.offsetLeft;
                        ele.style.left = ele.offsetLeft + speed + "px";
                        if(Math.abs(val)<Math.abs(speed)){
                            ele.style.left = target + "px";
                            clearInterval(ele.timer);
                        }
                    },10)  
            }
 
        }
    </script>
<style>

.div {

    display: inline-block;

    padding: .3em .5em;

    background-image: linear-gradient(#ddd, #bbb);

    border: 1px solid rgba(0,0,0,.2);

    border-radius: .3em;

    box-shadow: 0 1px white inset;

    text-align: center;

    text-shadow: 0 1px 1px black;

    color:white;

    font-weight: bold;

}

.div:active{

    box-shadow: .05em .1em .2em rgba(0,0,0,.6) inset;

    border-color: rgba(0,0,0,.3);

    background: #bbb;

}

</style>
</head>

<body>
<body bgcolor="grey">

<script type="text/javascript" src="mouse.js">
</script>
<h1 align="center">这是属于衣锦夜行的地方</h1>
<button type="button" class="div" onclick="document.getElementById('demo1').innerHTML = Date()">
Time and date
</button>
<p id="demo1"></p>
<p> <font size="3" face="Times"> DR宝宝！ </font> </p>
<body>
<div class="all" id='all'>
    <div class="screen" id="screen">
        <ul id="ul">
            <li><img src="lin1.jpg" width="400" height="600" /></li>
            <li><img src="lin2.jpg" width="400" height="600" /></li>
            <li><img src="lin3.jpg" width="400" height="600" /></li>
            <li><img src="shu1.jpg" width="400" height="600" /></li>
            <li><img src="shu2.jpg" width="400" height="600" /></li>
            <li><img src="shu3.jpg" width="400" height="600" /></li>
        </ul>
        <ol>
 
        </ol>
        <div id="arr">
            <span id="left"><</span>
            <span id="right">></span>
        </div>
    </div>
</div>
</body>



<hr />  
<h2>三个重要的基因组数据库</h2>
    <ul>
      <li> <a href="https://www.ncbi.nlm.nih.gov/">NCBI</a></li>
      <li> <a href="https://phytozome.jgi.doe.gov/pz/portal.html">Phytozome</a></li>
      <li> <a href="http://plants.ensembl.org/index.html">Ensembl Plant</a></li>
    </ul>
</body>  

</html>
