<template>
    <div id="map">
        <script>initialize1();</script>
    </div>
</template>

<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyAWn2P6KSq0O7akTgkAYCvlSy_W1hDTgNQ&"></script>
<script>
export default {

}
var map;
var map2;
var pmarker = [];
var pinfowin = [];
var amarker = [];
var ainfowin = [];
var dmarker = [];
var dinfowin = [];
//現在地表示
function setMarker(map) {
    navigator.geolocation.getCurrentPosition(
        // 取得成功した場合
        function(position) {
            var mylat = position.coords.latitude;
            var mylng = position.coords.longitude;
            //alert("緯度:"+position.coords.latitude+",経度"+position.coords.longitude);
            var myLatLng = new google.maps.LatLng(mylat, mylng);
            var marker = new google.maps.Marker({
                position: myLatLng,
                map: map,
                title: "現在地",
            })
            var infowin = new google.maps.InfoWindow({content:"現在地"});
            // mouseoverイベントを取得するListenerを追加
            google.maps.event.addListener(marker, 'mouseover', function(){
                infowin.open(map, marker);
            });

            // mouseoutイベントを取得するListenerを追加
            google.maps.event.addListener(marker, 'mouseout', function(){
                infowin.close();
            });
        },
        // 取得失敗した場合
        function(error) {
            switch(error.code) {
                case 1: //PERMISSION_DENIED
                    console.log("位置情報の利用が許可されていません");
                    break;
                case 2: //POSITION_UNAVAILABLE
                    console.log("現在位置が取得できませんでした");
                    break;
                case 3: //TIMEOUT
                    console.log("タイムアウトになりました");
                    break;
                default:
                    console.log("その他のエラー(エラーコード:"+error.code+")");
                    break;
            }
        }
    );
}

//map表示
function initialize1() {
    var MyLatLng = new google.maps.LatLng(36.149285, 136.768005); //表示する地図の位置
    var Options = {
        zoom: 13,      //地図の縮尺値
        center: MyLatLng,    //地図の中心座標
        mapTypeId: 'hybrid',   //地図の種類
        //disableDefaultUI: true コントローラーを表示しない
    };
    map = new google.maps.Map(document.getElementById('map'), Options);
    // 現在地表示
    setMarker(map);
    //参道の道を引く
    for (var i = 0; i < MountainRoad.length; i++){
        setPolyline(MountainRoad[i], map);
    }
    //登録された情報のマーカーを設置
    for (var i = 0; i < plant.length; i++) {
        plant_marker(plant[i], i, map);
    }
    for (var i = 0; i < animal.length; i++) {
        animal_marker(animal[i], i, map);
    }
    for (var i = 0; i < disaster.length; i++) {
        disaster_marker(disaster[i], i, map);
    } 
}

//植物マーカ設置
function plant_marker(plantinfo, count, setmap) {
    // マーカー位置セット
    var pmarkerLatLng = new google.maps.LatLng({
        lat: parseFloat(plantinfo[3]),
        lng: parseFloat(plantinfo[4])
    });
    // マーカーのセット
    pmarker[count] = new google.maps.Marker({
        position: pmarkerLatLng,          // マーカーを立てる位置を指定
        map: setmap,                        // マーカーを立てる地図を指定
        icon: {
            url: 'img/plant-t.png',
            scaledSize: new google.maps.Size(50, 50)
        }
    });
    //InformationWindowセット
    pinfowin[count] = new google.maps.InfoWindow({
        content: "<img src=" + plantinfo[5] + " width='150px'><br>" + "植物名：" + plantinfo[1] + "<br>" + "撮影日：" + plantinfo[2]
    });
    //マーカーのイベント作成
    google.maps.event.addListener(pmarker[count], 'mouseover', showInfo(count));
    function showInfo(index) {
        return function () {
            pinfowin[index].open(setmap, pmarker[index]);
        }
    }
    google.maps.event.addListener(pmarker[count], 'mouseout', closeInfo(count));
    function closeInfo(index) {
        return function () {
            pinfowin[index].close(setmap, pmarker[index]);
        }
    }
}

//動物マーカ設置
function animal_marker(animalinfo, count, setmap) {
    // マーカー位置セット
    var amarkerLatLng = new google.maps.LatLng({
        lat: parseFloat(animalinfo[3]),
        lng: parseFloat(animalinfo[4])
    });
    // マーカーのセット
    amarker[count] = new google.maps.Marker({
        position: amarkerLatLng,          // マーカーを立てる位置を指定
        map: setmap,                        // マーカーを立てる地図を指定
        icon: {
            url: 'img/animal-t.png',
            scaledSize: new google.maps.Size(50, 50)
        }
    });
    //InformationWindowセット
    ainfowin[count] = new google.maps.InfoWindow({
        content: "<img src=" + animalinfo[5] + " width='150px'><br>" + "動物名：" + animalinfo[1] + "<br>" + "撮影日：" + animalinfo[2]
    });
    //マーカーのイベント作成
    google.maps.event.addListener(amarker[count], 'mouseover', showInfo(count));
    function showInfo(index) {
        return function () {
            ainfowin[index].open(setmap, amarker[index]);
        }
    }
    google.maps.event.addListener(amarker[count], 'mouseout', closeInfo(count));
    function closeInfo(index) {
        return function () {
            ainfowin[index].close(setmap, amarker[index]);
        }
    }
}

//災害マーカ設置
function disaster_marker(disasterinfo, count, setmap) {
    // マーカー位置セット
    var dmarkerLatLng = new google.maps.LatLng({
        lat: parseFloat(disasterinfo[3]),
        lng: parseFloat(disasterinfo[4])
    });
    // マーカーのセット
    dmarker[count] = new google.maps.Marker({
        position: dmarkerLatLng,          // マーカーを立てる位置を指定
        map: setmap,                        // マーカーを立てる地図を指定
        icon: {
            url: 'img/disaster-t.png',
            scaledSize: new google.maps.Size(50, 50)
        }
    });
    //InformationWindowセット
    dinfowin[count] = new google.maps.InfoWindow({
        content: "<img src=" + disasterinfo[5] + " width='150px'><br>" + "詳細：" + disasterinfo[1] + "<br>" + "撮影日：" + disasterinfo[2]
    });
    //マーカーのイベント作成
    google.maps.event.addListener(dmarker[count], 'mouseover', showInfo(count));
    function showInfo(index) {
        return function () {
            dinfowin[index].open(setmap, dmarker[index]);
        }
    }
    google.maps.event.addListener(dmarker[count], 'mouseout', closeInfo(count));
    function closeInfo(index) {
        return function () {
            dinfowin[index].close(setmap, dmarker[index]);
        }
    }
}

function setPolyline(road, map) {
    const flightPath = new google.maps.Polyline({
        path: road,     //パスを調整
        geodesic: true,         //測地線を対応させる
        strokeColor: '#ff0000', //線色
        strokeOpacity: 0.5,     //透明度
        strokeWeight: 6         //px指定
    });
    flightPath.setMap(map);
}

const MountainRoad = [
    [
        //大汝峰の円道
        { lat: 36.166108, lng: 136.764507 },
        { lat: 36.164774, lng: 136.765441 },
        { lat: 36.164133, lng: 136.763531 },
        { lat: 36.162539, lng: 136.762877 },
        { lat: 36.159969, lng: 136.763728 },
        { lat: 36.159195, lng: 136.766267 },
        { lat: 36.159308, lng: 136.766664 },
        { lat: 36.159672, lng: 136.766374 },
        { lat: 36.159724, lng: 136.766567 },
        { lat: 36.161404, lng: 136.766267 },
        { lat: 36.161508, lng: 136.766567 },
        { lat: 36.164514, lng: 136.765902 },
        { lat: 36.164739, lng: 136.765451 },
        { lat: 36.164774, lng: 136.765441 }
    ],
    [
        //大汝峰の円道の下から東に抜ける道
        { lat: 36.159308, lng: 136.766664 },
        { lat: 36.160105, lng: 136.767340 },
        { lat: 36.160867, lng: 136.768026 },
        { lat: 36.162002, lng: 136.770537 },
        { lat: 36.162782, lng: 136.770891 },
        { lat: 36.163111, lng: 136.771824 },
        { lat: 36.162937, lng: 136.772629 },
        { lat: 36.163206, lng: 136.773670 },
        { lat: 36.163085, lng: 136.773874 },
        { lat: 36.163474, lng: 136.774775 },
        { lat: 36.163734, lng: 136.774904 },
        { lat: 36.163786, lng: 136.776556 },
        { lat: 36.164982, lng: 136.778873 },
        { lat: 36.165553, lng: 136.780633 },
        { lat: 36.166090, lng: 136.780890 },
        { lat: 36.166532, lng: 136.782618 },
        { lat: 36.166030, lng: 136.783819 },
        { lat: 36.165995, lng: 136.786180 },
        { lat: 36.169286, lng: 136.790814 },
        { lat: 36.169338, lng: 136.791222 },
        { lat: 36.169702, lng: 136.791587 },
        { lat: 36.170083, lng: 136.793754 },
        { lat: 36.170430, lng: 136.795256 },
        { lat: 36.170326, lng: 136.795449 },
        { lat: 36.170551, lng: 136.796222 },
        { lat: 36.170603, lng: 136.797338 },
        { lat: 36.171573, lng: 136.797681 },
        { lat: 36.172612, lng: 136.798668 },
        { lat: 36.174673, lng: 136.799419 }
    ],
    [
        //大汝峰の円道の下に抜けて室堂平に行く道
        { lat: 36.159308, lng: 136.766664 },
        { lat: 36.158390, lng: 136.766685 },
        { lat: 36.158251, lng: 136.766557 },
        { lat: 36.157541, lng: 136.766835 },
        { lat: 36.156484, lng: 136.766739 },
        { lat: 36.156233, lng: 136.766685 },
        { lat: 36.156129, lng: 136.766782 },
        { lat: 36.155938, lng: 136.766642 },
        { lat: 36.155826, lng: 136.766793 },
        { lat: 36.155427, lng: 136.766535 },
        { lat: 36.154925, lng: 136.766288 },
        { lat: 36.154492, lng: 136.766288 },
        { lat: 36.154154, lng: 136.766438 },
        { lat: 36.153799, lng: 136.767200 },
        { lat: 36.153417, lng: 136.767436 },
        { lat: 36.153275, lng: 136.767586 },
        { lat: 36.153145, lng: 136.767501 },
        { lat: 36.152651, lng: 136.767404 },
        { lat: 36.152231, lng: 136.767554 },
        { lat: 36.151720, lng: 136.767833 },
        { lat: 36.151260, lng: 136.767645 },
        { lat: 36.151191, lng: 136.767721 },
        { lat: 36.151018, lng: 136.767699 },
        { lat: 36.150975, lng: 136.767924 },
        { lat: 36.150871, lng: 136.768048 },
        { lat: 36.150775, lng: 136.768295 },
        { lat: 36.150238, lng: 136.768654 }
    ],
    [
        //室堂平に行く道の東側の道
        { lat: 36.157541, lng: 136.766835 },
        { lat: 36.157688, lng: 136.767093 },
        { lat: 36.158078, lng: 136.767254 },
        { lat: 36.158217, lng: 136.767554 },
        { lat: 36.158546, lng: 136.767683 },
        { lat: 36.158658, lng: 136.767790 },
        { lat: 36.158970, lng: 136.767833 },
        { lat: 36.159065, lng: 136.768155 },
        { lat: 36.159369, lng: 136.768177 },
        { lat: 36.159611, lng: 136.768391 },
        { lat: 36.159637, lng: 136.768616 },
        { lat: 36.158398, lng: 136.768777 },
        { lat: 36.158208, lng: 136.769142 },
        { lat: 36.158156, lng: 136.769528 },
        { lat: 36.157550, lng: 136.770355 },
        { lat: 36.156935, lng: 136.770215 },
        { lat: 36.156839, lng: 136.770076 },
        { lat: 36.156371, lng: 136.770440 },
        { lat: 36.156276, lng: 136.770011 },
        { lat: 36.156138, lng: 136.770183 },
        { lat: 36.156068, lng: 136.769732 },
        { lat: 36.156094, lng: 136.769410 },
        { lat: 36.155730, lng: 136.769700 },
        { lat: 36.155678, lng: 136.769684 },
        { lat: 36.155193, lng: 136.770682 },
        { lat: 36.155150, lng: 136.770864 },
        { lat: 36.155163, lng: 136.770971 },
        { lat: 36.155029, lng: 136.771422 },
        { lat: 36.154812, lng: 136.771594 },
        { lat: 36.154769, lng: 136.771765 },
        { lat: 36.154691, lng: 136.771615 },
        { lat: 36.154483, lng: 136.771964 },
        { lat: 36.154379, lng: 136.771851 },
        { lat: 36.154232, lng: 136.772162 },
        { lat: 36.154162, lng: 136.771932 },
        { lat: 36.154059, lng: 136.772103 },
        { lat: 36.153911, lng: 136.771148 },
        { lat: 36.153864, lng: 136.771132 },
        { lat: 36.153820, lng: 136.770800 },
        { lat: 36.153734, lng: 136.770843 },
        { lat: 36.153699, lng: 136.770768 },
        { lat: 36.153552, lng: 136.770837 },
        { lat: 36.153469, lng: 136.770692 },
        { lat: 36.153413, lng: 136.770741 },
        { lat: 36.153288, lng: 136.770698 },
        { lat: 36.152967, lng: 136.770794 },
        { lat: 36.152841, lng: 136.770746 },
        { lat: 36.152711, lng: 136.770832 },
        { lat: 36.152629, lng: 136.770827 },
        { lat: 36.152577, lng: 136.770725 },
        { lat: 36.152573, lng: 136.770194 },
        { lat: 36.152174, lng: 136.769931 },
        { lat: 36.152070, lng: 136.769893 },
        { lat: 36.151741, lng: 136.769469 },
        { lat: 36.151663, lng: 136.769502 },
        { lat: 36.151598, lng: 136.769341 },
        { lat: 36.151486, lng: 136.769351 },
        { lat: 36.151217, lng: 136.769003 },
        { lat: 36.150992, lng: 136.768928 },
        { lat: 36.150775, lng: 136.768826 },
        { lat: 36.150741, lng: 136.768858 },
        { lat: 36.150238, lng: 136.768654 },
        { lat: 36.149285, lng: 136.768005 }
    ],
    [
        //室堂平に行く道の西側の道
        { lat: 36.156484, lng: 136.766739 },
        { lat: 36.156458, lng: 136.766320 },
        { lat: 36.156363, lng: 136.766299 },
        { lat: 36.156112, lng: 136.766063 },
        { lat: 36.156094, lng: 136.765451 },
        { lat: 36.156216, lng: 136.764336 },
        { lat: 36.156129, lng: 136.764336 },
        { lat: 36.156112, lng: 136.764057 },
        { lat: 36.155523, lng: 136.764271 },
        { lat: 36.155341, lng: 136.764261 },
        { lat: 36.154882, lng: 136.763273 },
        { lat: 36.154760, lng: 136.763231 },
        { lat: 36.154142, lng: 136.762336 },
        { lat: 36.153417, lng: 136.762190 },
        { lat: 36.152231, lng: 136.763134 },
        { lat: 36.151806, lng: 136.764282 },
        { lat: 36.151520, lng: 136.764969 },
        { lat: 36.151503, lng: 136.765290 },
        { lat: 36.151260, lng: 136.765527 },
        { lat: 36.150862, lng: 136.766406 },
        { lat: 36.150732, lng: 136.766481 },
        { lat: 36.150637, lng: 136.766632 },
        { lat: 36.150533, lng: 136.766664 },
        { lat: 36.150533, lng: 136.766793 },
        { lat: 36.150316, lng: 136.766814 },
        { lat: 36.150325, lng: 136.766921 },
        { lat: 36.150065, lng: 136.766943 },
        { lat: 36.149692, lng: 136.767308 },
        { lat: 36.149649, lng: 136.767436 },
        { lat: 36.149285, lng: 136.768005 }
    ],
    [
        //南竜山荘の中心の2つの道
        { lat: 36.149285, lng: 136.768005 },
        { lat: 36.146574, lng: 136.765001 },
        { lat: 36.145849, lng: 136.765723 },
        { lat: 36.145404, lng: 136.765952 },
        { lat: 36.144925, lng: 136.765916 },
        { lat: 36.143596, lng: 136.765044 },
        { lat: 36.142753, lng: 136.764972 },
        { lat: 36.142106, lng: 136.765580 },
        { lat: 36.141343, lng: 136.766045 },
        { lat: 36.140714, lng: 136.766081 },
        { lat: 36.139917, lng: 136.766210 },
        { lat: 36.139275, lng: 136.766460 },
        { lat: 36.138848, lng: 136.766546 },
        { lat: 36.138577, lng: 136.766932 },
        { lat: 36.138201, lng: 136.766746 },
        { lat: 36.136936, lng: 136.766531 },
        { lat: 36.137144, lng: 136.767375 },
        { lat: 36.136601, lng: 136.767182 },
        { lat: 36.136751, lng: 136.767540 },
        { lat: 36.136566, lng: 136.767504 },
        { lat: 36.136532, lng: 136.767640 },
        { lat: 36.136399, lng: 136.767590 },
        { lat: 36.136376, lng: 136.767755 },
        { lat: 36.136205, lng: 136.767687 },
        { lat: 36.136292, lng: 136.767876 },
        { lat: 36.136211, lng: 136.768498 },
        { lat: 36.136361, lng: 136.768463 },
        { lat: 36.136523, lng: 136.768570 },
        { lat: 36.136552, lng: 136.768956 },
        { lat: 36.136431, lng: 136.769099 },
        { lat: 36.136223, lng: 136.770215 },
        { lat: 36.136049, lng: 136.770301 },
        { lat: 36.135795, lng: 136.770859 },
        { lat: 36.135859, lng: 136.771574 },
        { lat: 36.135501, lng: 136.772439 },
        { lat: 36.135475, lng: 136.772468 },
        { lat: 36.136535, lng: 136.772718 },
        { lat: 36.136939, lng: 136.772432 },
        { lat: 36.136968, lng: 136.772168 },
        { lat: 36.137366, lng: 136.772203 },
        { lat: 36.137430, lng: 136.772082 },
        { lat: 36.138060, lng: 136.772282 },
        { lat: 36.138204, lng: 136.772232 },
        { lat: 36.138886, lng: 136.772382 },
        { lat: 36.139353, lng: 136.772189 },
        { lat: 36.139850, lng: 136.771066 },
        { lat: 36.140393, lng: 136.770837 },
        { lat: 36.140913, lng: 136.770988 },
        { lat: 36.141600, lng: 136.771080 },
        { lat: 36.141826, lng: 136.770945 },
        { lat: 36.141982, lng: 136.771152 },
        { lat: 36.142114, lng: 136.771009 },
        { lat: 36.142704, lng: 136.771030 },
        { lat: 36.142790, lng: 136.770952 },
        { lat: 36.143062, lng: 136.771302 },
        { lat: 36.143281, lng: 136.771474 },
        { lat: 36.143558, lng: 136.771367 },
        { lat: 36.143605, lng: 136.771216 },
        { lat: 36.144165, lng: 136.771259 },
        { lat: 36.144800, lng: 136.771545 },
        { lat: 36.145280, lng: 136.771445 },
        { lat: 36.146007, lng: 136.771631 },
        { lat: 36.146660, lng: 136.771438 },
        { lat: 36.146966, lng: 136.771102 },
        { lat: 36.146978, lng: 136.770802 },
        { lat: 36.147538, lng: 136.770337 },
        { lat: 36.148202, lng: 136.770065 },
        { lat: 36.148676, lng: 136.769264 },
        { lat: 36.148676, lng: 136.769264 },
        { lat: 36.149285, lng: 136.768005 }
    ],
    [
        //南竜山荘の東側の道
        { lat: 36.149285, lng: 136.768005 },
        { lat: 36.148838, lng: 136.770101 },
        { lat: 36.148916, lng: 136.770734 },
        { lat: 36.148846, lng: 136.770977 },
        { lat: 36.148858, lng: 136.771313 },
        { lat: 36.148823, lng: 136.771420 },
        { lat: 36.149002, lng: 136.772500 },
        { lat: 36.149274, lng: 136.773072 },
        { lat: 36.149349, lng: 136.773387 },
        { lat: 36.149562, lng: 136.773516 },
        { lat: 36.149695, lng: 136.773752 },
        { lat: 36.149580, lng: 136.773895 },
        { lat: 36.149372, lng: 136.773895 },
        { lat: 36.149256, lng: 136.774088 },
        { lat: 36.149066, lng: 136.774782 },
        { lat: 36.148569, lng: 136.775254 },
        { lat: 36.147991, lng: 136.775376 },
        { lat: 36.147385, lng: 136.775798 },
        { lat: 36.146299, lng: 136.775919 },
        { lat: 36.146270, lng: 136.775962 },
        { lat: 36.145652, lng: 136.775769 },
        { lat: 36.144821, lng: 136.775876 },
        { lat: 36.144572, lng: 136.776012 },
        { lat: 36.144133, lng: 136.776012 },
        { lat: 36.144168, lng: 136.776277 },
        { lat: 36.144064, lng: 136.776384 },
        { lat: 36.143844, lng: 136.776384 },
        { lat: 36.143810, lng: 136.776592 },
        { lat: 36.143238, lng: 136.777557 },
        { lat: 36.142689, lng: 136.777836 },
        { lat: 36.142458, lng: 136.778144 },
        { lat: 36.141961, lng: 136.778415 },
        { lat: 36.140824, lng: 136.778444 },
        { lat: 36.140737, lng: 136.778251 },
        { lat: 36.141072, lng: 136.777922 },
        { lat: 36.140852, lng: 136.777629 },
        { lat: 36.140639, lng: 136.777521 },
        { lat: 36.140171, lng: 136.777436 },
        { lat: 36.139882, lng: 136.777292 },
        { lat: 36.139764, lng: 136.777074 },
        { lat: 36.139544, lng: 136.776903 },
        { lat: 36.139483, lng: 136.776592 },
        { lat: 36.139322, lng: 136.776545 },
        { lat: 36.139299, lng: 136.776674 },
        { lat: 36.138917, lng: 136.776695 },
        { lat: 36.138744, lng: 136.776316 },
        { lat: 36.138692, lng: 136.775901 },
        { lat: 36.138421, lng: 136.775751 },
        { lat: 36.138403, lng: 136.775486 },
        { lat: 36.138190, lng: 136.774914 },
        { lat: 36.138039, lng: 136.774914 },
        { lat: 36.137433, lng: 136.774993 },
        { lat: 36.137294, lng: 136.774921 },
        { lat: 36.136976, lng: 136.774979 },
        { lat: 36.136283, lng: 136.774564 },
        { lat: 36.135937, lng: 136.774414 },
        { lat: 36.135752, lng: 136.774063 },
        { lat: 36.135423, lng: 136.773884 },
        { lat: 36.135278, lng: 136.773412 },
        { lat: 36.135475, lng: 136.772468 }
    ],
    [
        //南竜山荘の西側の道
        { lat: 36.146574, lng: 136.765001 },
        { lat: 36.146221, lng: 136.764446 },
        { lat: 36.145615, lng: 136.764103 },
        { lat: 36.145615, lng: 136.764103 },
        { lat: 36.145066, lng: 136.763781 },
        { lat: 36.144852, lng: 136.763273 },
        { lat: 36.144223, lng: 136.761986 },
        { lat: 36.144162, lng: 136.761993 },
        { lat: 36.143787, lng: 136.761571 },
        { lat: 36.143700, lng: 136.761650 },
        { lat: 36.143619, lng: 136.761442 },
        { lat: 36.143509, lng: 136.761460 },
        { lat: 36.143469, lng: 136.761310 },
        { lat: 36.143365, lng: 136.761274 },
        { lat: 36.143296, lng: 136.761303 },
        { lat: 36.143232, lng: 136.761260 },
        { lat: 36.143134, lng: 136.761253 },
        { lat: 36.142978, lng: 136.761146 },
        { lat: 36.142862, lng: 136.761181 },
        { lat: 36.142851, lng: 136.761346 },
        { lat: 36.142770, lng: 136.761475 },
        { lat: 36.142487, lng: 136.761503 },
        { lat: 36.142268, lng: 136.761639 },
        { lat: 36.142175, lng: 136.761775 },
        { lat: 36.142034, lng: 136.761743 },
        { lat: 36.141953, lng: 136.761800 },
        { lat: 36.141866, lng: 136.761857 },
        { lat: 36.141785, lng: 136.761850 },
        { lat: 36.141572, lng: 136.761879 },
        { lat: 36.141392, lng: 136.761614 },
        { lat: 36.141283, lng: 136.761893 },
        { lat: 36.141312, lng: 136.762022 },
        { lat: 36.141289, lng: 136.762172 },
        { lat: 36.141173, lng: 136.762236 },
        { lat: 36.140971, lng: 136.762344 },
        { lat: 36.140705, lng: 136.762902 },
        { lat: 36.140584, lng: 136.762902 },
        { lat: 36.140370, lng: 136.763216 },
        { lat: 36.140099, lng: 136.763266 },
        { lat: 36.139833, lng: 136.763510 },
        { lat: 36.139740, lng: 136.763524 },
        { lat: 36.139683, lng: 136.763459 },
        { lat: 36.139590, lng: 136.763459 },
        { lat: 36.139544, lng: 136.763524 },
        { lat: 36.139446, lng: 136.763524 },
        { lat: 36.139105, lng: 136.763331 },
        { lat: 36.138793, lng: 136.763352 },
        { lat: 36.138626, lng: 136.763159 },
        { lat: 36.138481, lng: 136.763173 },
        { lat: 36.138343, lng: 136.763359 },
        { lat: 36.138002, lng: 136.763374 },
        { lat: 36.137794, lng: 136.763817 },
        { lat: 36.137505, lng: 136.764182 },
        { lat: 36.137262, lng: 136.764289 },
        { lat: 36.137043, lng: 136.764768 },
        { lat: 36.135882, lng: 136.766242 },
        { lat: 36.135963, lng: 136.767157 },
        { lat: 36.136107, lng: 136.767336 },
        { lat: 36.136205, lng: 136.767687 }
    ],
    [
        //白山山頂郵便局から東へ抜ける道
        { lat: 36.149692, lng: 136.773770 },
        { lat: 36.149791, lng: 136.773956 },
        { lat: 36.150022, lng: 136.774142 },
        { lat: 36.149854, lng: 136.774757 },
        { lat: 36.149802, lng: 136.775236 },
        { lat: 36.149814, lng: 136.775608 },
        { lat: 36.149843, lng: 136.775873 },
        { lat: 36.149750, lng: 136.776087 },
        { lat: 36.149444, lng: 136.776674 },
        { lat: 36.149005, lng: 136.777554 }
    ],
    [
        //南竜ヶ馬場野営場1
        { lat: 36.135278, lng: 136.773412 },
        { lat: 36.135200, lng: 136.773405 },
        { lat: 36.134992, lng: 136.773562 },
        { lat: 36.134963, lng: 136.773477 },
        { lat: 36.134986, lng: 136.773312 },
        { lat: 36.134963, lng: 136.773033 },
        { lat: 36.134767, lng: 136.772575 },
        { lat: 36.134628, lng: 136.772332 },
        { lat: 36.134467, lng: 136.772132 },
        { lat: 36.134195, lng: 136.771975 },
        { lat: 36.134114, lng: 136.771846 },
        { lat: 36.133964, lng: 136.771781 },
        { lat: 36.133831, lng: 136.771617 },
        { lat: 36.133773, lng: 136.771545 },
        { lat: 36.133640, lng: 136.771467 },
        { lat: 36.133519, lng: 136.771467 },
        { lat: 36.133271, lng: 136.771166 },
        { lat: 36.133092, lng: 136.770959 },
        { lat: 36.132976, lng: 136.770794 },
        { lat: 36.132942, lng: 136.770973 },
        { lat: 36.132959, lng: 136.771245 },
        { lat: 36.133126, lng: 136.771653 },
        { lat: 36.133040, lng: 136.771753 },
        { lat: 36.133126, lng: 136.771975 },
        { lat: 36.133253, lng: 136.772082 },
        { lat: 36.133392, lng: 136.772346 },
        { lat: 36.133952, lng: 136.772890 },
        { lat: 36.134386, lng: 136.773334 },
        { lat: 36.134963, lng: 136.773570 }
    ],
    [
        //南竜ヶ馬場野営場2
        { lat: 36.134992, lng: 136.773562 },
        { lat: 36.132843, lng: 136.770651 },
        { lat: 36.132682, lng: 136.770608 },
        { lat: 36.132502, lng: 136.770358 },
        { lat: 36.132266, lng: 136.770208 },
        { lat: 36.132150, lng: 136.770029 },
        { lat: 36.132075, lng: 136.769986 },
        { lat: 36.131965, lng: 136.769958 },
    ],
    [
        //延命水から甚之助谷砂防堰堤群の円道
        { lat: 36.144187, lng: 136.761954 },
        { lat: 36.144351, lng: 136.761063 },
        { lat: 36.143277, lng: 136.759132 },
        { lat: 36.142428, lng: 136.758585 },
        { lat: 36.141579, lng: 136.758574 },
        { lat: 36.141492, lng: 136.758113 },
        { lat: 36.141267, lng: 136.758274 },
        { lat: 36.141137, lng: 136.755860 },
        { lat: 36.140782, lng: 136.755270 },
        { lat: 36.140955, lng: 136.754626 },
        { lat: 36.139837, lng: 136.754562 },
        { lat: 36.139369, lng: 136.753081 },
        { lat: 36.139135, lng: 136.751450 },
        { lat: 36.136779, lng: 136.746955 },
        { lat: 36.134456, lng: 136.743650 },
        { lat: 36.132736, lng: 136.742814 },
        { lat: 36.132806, lng: 136.743801 },
        { lat: 36.131939, lng: 136.744037 },
        { lat: 36.131402, lng: 136.744101 },
        { lat: 36.131107, lng: 136.743736 },
        { lat: 36.130691, lng: 136.744273 },
        { lat: 36.129704, lng: 136.742513 },
        { lat: 36.128057, lng: 136.741290 },
        { lat: 36.127589, lng: 136.741998 },
        { lat: 36.127624, lng: 136.742857 },
        { lat: 36.127069, lng: 136.742749 },
        { lat: 36.126584, lng: 136.741891 },
        { lat: 36.126376, lng: 136.741054 },
        { lat: 36.125821, lng: 136.741376 },
        { lat: 36.125163, lng: 136.740582 },
        { lat: 36.125041, lng: 136.740754 },
        { lat: 36.124643, lng: 136.742030 },
        { lat: 36.125059, lng: 136.742749 },
        { lat: 36.125284, lng: 136.744251 },
        { lat: 36.127277, lng: 136.744852 },
        { lat: 36.127052, lng: 136.745152 },
        { lat: 36.126653, lng: 136.745131 },
        { lat: 36.126879, lng: 136.745324 },
        { lat: 36.126012, lng: 136.745195 },
        { lat: 36.126879, lng: 136.746740 },
        { lat: 36.126809, lng: 136.748779 },
        { lat: 36.127139, lng: 136.748779 },
        { lat: 36.127381, lng: 136.749744 },
        { lat: 36.127485, lng: 136.749380 },
        { lat: 36.127797, lng: 136.750002 },
        { lat: 36.128300, lng: 136.750216 },
        { lat: 36.128594, lng: 136.750603 },
        { lat: 36.128837, lng: 136.751997 },
        { lat: 36.129305, lng: 136.752319 },
        { lat: 36.129340, lng: 136.751912 },
        { lat: 36.130189, lng: 136.752491 },
        { lat: 36.130570, lng: 136.752212 },
        { lat: 36.130986, lng: 136.752942 },
        { lat: 36.131281, lng: 136.752942 },
        { lat: 36.131281, lng: 136.753757 },
        { lat: 36.131419, lng: 136.754143 },
        { lat: 36.131627, lng: 136.754272 },
        { lat: 36.131610, lng: 136.754422 },
        { lat: 36.131818, lng: 136.754637 },
        { lat: 36.131506, lng: 136.754873 },
        { lat: 36.131662, lng: 136.755044 },
        { lat: 36.131939, lng: 136.755002 },
        { lat: 36.131887, lng: 136.755152 },
        { lat: 36.132043, lng: 136.755645 },
        { lat: 36.132442, lng: 136.755924 },
        { lat: 36.132338, lng: 136.756268 },
        { lat: 36.132390, lng: 136.756525 },
        { lat: 36.132234, lng: 136.757083 },
        { lat: 36.132546, lng: 136.758199 },
        { lat: 36.132875, lng: 136.758263 },
        { lat: 36.132979, lng: 136.758885 },
        { lat: 36.133135, lng: 136.759164 },
        { lat: 36.133308, lng: 136.759207 },
        { lat: 36.133395, lng: 136.759529 },
        { lat: 36.134140, lng: 136.759207 },
        { lat: 36.134504, lng: 136.759229 },
        { lat: 36.135041, lng: 136.760023 },
        { lat: 36.135336, lng: 136.760044 },
        { lat: 36.135942, lng: 136.760473 },
        { lat: 36.135804, lng: 136.761053 },
        { lat: 36.135717, lng: 136.761503 },
        { lat: 36.136341, lng: 136.761546 },
        { lat: 36.136896, lng: 136.760795 },
        { lat: 36.137086, lng: 136.760774 },
        { lat: 36.137156, lng: 136.761117 },
        { lat: 36.137346, lng: 136.761096 },
        { lat: 36.137415, lng: 136.761761 },
        { lat: 36.137589, lng: 136.761696 },
        { lat: 36.137519, lng: 136.762125 },
        { lat: 36.137901, lng: 136.761975 },
        { lat: 36.138005, lng: 136.762276 },
        { lat: 36.137987, lng: 136.763306 },
        { lat: 36.138002, lng: 136.763374 }
    ],
    [
        //延命水から甚之助谷砂防堰堤群の円道の脇道
        { lat: 36.132736, lng: 136.742814 },
        { lat: 36.132238, lng: 136.742224 },
        { lat: 36.132056, lng: 136.738608 },
        { lat: 36.130609, lng: 136.736451 },
        { lat: 36.130133, lng: 136.736451 },
        { lat: 36.129916, lng: 136.734842 },
        { lat: 36.127143, lng: 136.728469 }
    ]
]

</script>