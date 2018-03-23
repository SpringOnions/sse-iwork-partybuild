<template>
<div class="party-container">
    <div id="map_container"></div>
	<!-- <div class="bottom-circle"></div>
    <div class="bottom-rectangle"></div> -->
    <div></div>
</div>
</template>

<script>
import markerImg from "../../../static/marker.png";
var self =this;
var current_lng=0;
var current_lat=0;
// function OD(a, b, c) {
//     while (a > c) a -= c - b;
//     while (a < b) a += c - b;
//     return a;
// }
// function SD(a, b, c) {
//     b != null && (a = Math.max(a, b));
//     c != null && (a = Math.min(a, c));
//     return a;
// }
// function getDistance(a_lat,a_lng,b_lat,b_lng) {
//     var a = Math.PI * OD(a_lat, -180, 180) / 180;
//     var b = Math.PI * OD(b_lat, -180, 180) / 180;
//     var c = Math.PI * SD(a_lng, -74, 74) / 180;
//     var d = Math.PI * SD(b_lng, -74, 74) / 180;
//     return 6370996.81 * Math.acos(Math.sin(c) * Math.sin(d) + Math.cos(c) * Math.cos(d) * Math.cos(b-a));
// }
function fD(a, b, c) {
	for (; a > c;)
		a -= c - b;
	for (; a < b;)
		a += c - b;
	return a;
};
function jD(a, b, c) {
	b != null && (a = Math.max(a, b));
	c != null && (a = Math.min(a, c));
	return a;
};
function yk(a) {
	return Math.PI * a / 180
};
function Ce(a, b, c, d) {
	var dO = 6370996.81;
	return dO * Math.acos(Math.sin(c) * Math.sin(d) + Math.cos(c) * Math.cos(d) * Math.cos(b - a));
};
function getDistance(a, b,c,d) {
	if (!a || !b)
		return 0;
	a = fD(a, -180, 180);
	b = jD(b, -74, 74);
	c = fD(c, -180, 180);
	d = jD(d, -74, 74);
	return Ce(yk(a), yk(c), yk(b), yk(d));
};
export default {
  components: {},
  data() {
    return {
        current_lng:0,
        current_lat:0,
    };
  },
  methods: {
      getCurrentPositionB(){
        // 百度地图定位功能
        var map = new BMap.Map("map_container");
        var point = new BMap.Point(121.509213,31.21252);
        map.centerAndZoom(point,12);
        window.map = map;//将map变量存储在全局

        map.enableScrollWheelZoom();//启用地图滚轮放大缩小

        var geolocation = new BMap.Geolocation();
        geolocation.getCurrentPosition(function(r){
            if(this.getStatus() == BMAP_STATUS_SUCCESS){
                var mk = new BMap.Marker(r.point);
                map.addOverlay(mk);
                map.panTo(r.point);
                //alert('您的位置：'+r.point.lng+','+r.point.lat);
                current_lng=r.point.lng;
                current_lat=r.point.lat;
                console.log("this.current_lng11",current_lng)
            }
            else {
                alert('failed'+this.getStatus());
            }        
        },{enableHighAccuracy: true})

        this.setCheckPosition();
      },
      setCheckPosition(){
        var point1 = new BMap.Point(121.506377, 31.245105);
        var point2 = new BMap.Point(121.509213, 31.21252);
        var myIcon = new BMap.Icon(markerImg, new BMap.Size(40, 40), {
        // 指定定位位置。
        // 当标注显示在地图上时，其所指向的地理位置距离图标左上
        // 角各偏移10像素和25像素。您可以看到在本例中该位置即是
        // 图标中央下端的尖角位置。
        anchor: new BMap.Size(10, 25),
        // 设置图片偏移。
        // 当您需要从一幅较大的图片中截取某部分作为标注图标时，您
        // 需要指定大图的偏移位置，此做法与css sprites技术类似。
        imageOffset: new BMap.Size(0, 0) // 设置图片偏移
        });
        // 创建标注对象并添加到地图
        var marker1 = new BMap.Marker(point1, { icon: myIcon });
        map.addOverlay(marker1);
        var marker2 = new BMap.Marker(point2, { icon: myIcon });
        map.addOverlay(marker2);
        console.log("this.current_lng2",current_lng)
        marker1.addEventListener("click", function() {
            console.log("this.current_lng",current_lng)
            let dis =(getDistance(current_lng,current_lat,121.506377,31.245105)/1000).toFixed(2);
            console.log(dis)
        });
      },
    OD(a, b, c) {
        while (a > c) a -= c - b;
        while (a < b) a += c - b;
        return a;
    },
    SD(a, b, c) {
        b != null && (a = Math.max(a, b));
        c != null && (a = Math.min(a, c));
        return a;
    },
    getDistance(a_lat,a_lng,b_lat,b_lng) {
        var a = Math.PI * OD(a_lat, -180, 180) / 180;
        var b = Math.PI * OD(b_lat, -180, 180) / 180;
        var c = Math.PI * SD(a_lng, -74, 74) / 180;
        var d = Math.PI * SD(b_lng, -74, 74) / 180;
        return 6370996.81 * Math.acos(Math.sin(c) * Math.sin(d) + Math.cos(c) * Math.cos(d) * Math.cos(b-a));
    }

  },
  mounted() {
    
    this.getCurrentPositionB();
    
   

    
    // var geolocationControl = new BMap.GeolocationControl();
    // geolocationControl.location();
    // var myLocation = geolocationControl.getAddressComponent();
    // console.log("myLocation",myLocation)

 //定位成功事件
    // var myKeys='';
    // var lng='';         //精度
    // var lat='';         //纬度
    // // 百度地图API功能
    // var map = new BMap.Map("map_container");
    // var point = new BMap.Point(108.95,34.27);
    // map.centerAndZoom(point,12);
    // var geolocation = new BMap.Geolocation();
    // geolocation.getCurrentPosition(function(r){console.log(r.point)
    //     if(this.getStatus() == BMAP_STATUS_SUCCESS){
    //         var mk = new BMap.Marker(r.point);
    //         map.addOverlay(mk);//标出所在地
    //         map.panTo(r.point);//地图中心移动
    //         //alert('您的位置：'+r.point.lng+','+r.point.lat);
    //         lng=r.point.lng;
    //         lat=r.point.lat;
    //         var point = new BMap.Point(r.point.lng,r.point.lat);    //用所定位的经纬度查找所在地省市街道等信息
    //         var gc = new BMap.Geocoder();
    //         gc.getLocation(point, function(rs){
    //            var addComp = rs.addressComponents;
    //            console.log(rs.address);//地址信息
    //            //myKeys = [rs.address];     当有其他信息时，可以定义成数组
    //             myKeys = rs.address;
    //            //alert(myKeys);         //弹出所在地址
    //            var local = new BMap.LocalSearch(map, {
    //                renderOptions:{map:map,panel:"r-result"},
    //                pageCapacity:3
    //             });
    //             local.searchInBounds(myKeys, map.getBounds());
    //         });
    //     }else {
    //         alert('failed'+this.getStatus());
    //     }        
    // },{enableHighAccuracy: true})


  }
};
let articleList = [{
    articleID: 1,
    article: "四个意识",
    totalNum: 4,
    getNum: 0,
    wordsList: [{
            wordsID: 101,
            words: "政治意识",
            isGet: false
        },
        {
            wordsID: 102,
            words: "大局意识",
            isGet: false
        },
         {
            wordsID: 103,
            words: "核心意识",
            isGet: false
        },
         {
            wordsID: 104,
            words: "看齐意识",
            isGet: false
        }
    ]
},
{
    articleID: 2,
    article: "四个自信",
    totalNum: 4,
    getNum: 0,
    wordsList: [{
            wordsID: 201,
            words: "道路自信",
            isGet: false
        },
        {
            wordsID: 202,
            words: "理论自信",
            isGet: false
        },
         {
            wordsID: 203,
            words: "制度自信",
            isGet: false
        },
         {
            wordsID: 204,
            words: "文化自信",
            isGet: false
        }
    ]
},
{
    articleID: 3,
    article: "“五位一体”总体布局",
    totalNum:5,
    getNum: 0,
    wordsList: [{
            wordsID: 301,
            words: "经济建设",
            isGet: false
        },
        {
            wordsID: 302,
            words: "政治建设",
            isGet: false
        },
         {
            wordsID: 303,
            words: "文化建设",
            isGet: false
        },
         {
            wordsID: 304,
            words: "社会建设",
            isGet: false
        },
         {
            wordsID: 305,
            words: "生态文明建设",
            isGet: false
        }
    ]
},{
    articleID: 4,
    article: "“四个全面”战略布局",
    totalNum:4,
    getNum: 0,
    wordsList: [{
            wordsID: 401,
            words: "全面建成小康",
            isGet: false
        },
        {
            wordsID: 402,
            words: "全面深化改革",
            isGet: false
        },
         {
            wordsID: 403,
            words: "全面依法治国",
            isGet: false
        },
         {
            wordsID: 404,
            words: "全面从严治党",
            isGet: false
        }
    ]
},
{
    articleID: 5,
    article: "四个伟大",
    totalNum:4,
    getNum: 0,
    wordsList: [{
            wordsID: 501,
            words: "进行伟大斗争",
            isGet: false
        },
        {
            wordsID: 502,
            words: "建设伟大工程",
            isGet: false
        },
         {
            wordsID: 503,
            words: "推进伟大事业",
            isGet: false
        },
         {
            wordsID: 504,
            words: "实现伟大梦想",
            isGet: false
        }
    ]
},
{
    articleID: 6,
    article: "党的六大纪律",
    totalNum:6,
    getNum: 0,
    wordsList: [{
            wordsID: 601,
            words: "政治纪律",
            isGet: false
        },
        {
            wordsID: 602,
            words: "组织纪律",
            isGet: false
        },
         {
            wordsID: 603,
            words: "廉洁纪律",
            isGet: false
        },
         {
            wordsID: 604,
            words: "群众纪律",
            isGet: false
        },
         {
            wordsID: 605,
            words: "工作纪律",
            isGet: false
        },
         {
            wordsID: 606,
            words: "生活纪律",
            isGet: false
        }
    ]
},
{
    articleID: 7,
    article: "上交所文化价值体系",
    totalNum:3,
    getNum: 0,
    wordsList: [{
            wordsID: 701,
            words: "价值口号：至诚至公 全球融通 建世界领先交易所",
            isGet: false
        },
        {
            wordsID: 702,
            words: "使命愿景：汇聚资本力量，服务实体经济，服务投资大众，服务发展大局；有效维护市场公开公平公正，有效配置国内国际资源，有效防范市场风险；建设成为安全高效，功能完备，与我国社会主义现代化强国相匹配的世界领先交易所。",
            isGet: false
        },
         {
            wordsID: 703,
            words: "核心价值观：高效、透明、稳健  服务、进取、奉献",
            isGet: false
        }
         
    ]
},
]
</script>
<style lang="less">
.party-container {
  height: 100%;
}
#map_container {
  width: 100%;
  height: 100%;
  overflow: hidden;
  margin: 0;
  font-family: "微软雅黑";
}
.bottom-circle {
  position: fixed;
  bottom: -20px;
  height: 160px;
  width: 100%;
  background: white;
  border-radius: 50%;
}
.bottom-rectangle {
  position: fixed;
  bottom: 0;
  height: 60px;
  width: 100%;
  background: white;
}
</style>
