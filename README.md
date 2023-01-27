Huawei 4/5G WiFi LTE Band Selector

Enable LTE band selection within your browser. 2 parts..

# Working Environment
## Supported devices
Tested on these Huawei devices.
1. B535
2. E5778
3. E5885 (Global-one egg in Korea)

## Supported clients
Every Javascript supported modern browsers.

### Tested on these clients.
Mac OS Safari, Chrome
iOS Safari
Windows 10 Firefox,Edge, Chrome (Web Console)

#It is permanent even after reboot
#For reset band and setting, simple factory reset..

# How-to Part 1
1. First go on to your page. (Ex. 192.168.8.1)
2. Log-in to your device.
3. In home page Copy and paste this script on console [F12]. 
4. Setting Subnet and DNS Selection: recharge page (F5) after go to Advanced > Router > DHCP / Copy and paste this script on console [F12].

```javascript
javascript:ftb();function currentBand(){1!=suspend&&($("#dhcp_mask").show(),$("#dhcp_dns").show(),$.ajax({dataType:"text",type:"GET",async:!0,url:"/api/device/signal",error:err,success:function(n){for(signal=n,vars=["nrrsrq","nrrsrp","nrsinr","rssi","rsrp","rsrq","sinr","dlbandwidth","ulbandwidth","band","cell_id","plmn"],i=0;i<vars.length;i++)window[vars[i]]=extractXML(vars[i],n),$("#"+vars[i]).html(window[vars[i]]);nrdefined="undefined"!=typeof nrrsrp,$(".e5").toggle(nrdefined),nrdefined&&(setgraph("nrrsrp",nrrsrp,-130,-70),setgraph("nrrsrq",nrrsrq,-16,-3)),setgraph("rsrp",rsrp,-130,-70),setgraph("rsrq",rsrq,-16,-3),mp=cell_id.indexOf("-"),enbid=0<mp?Number(cell_id.substr(0,mp)):(hex=Number(cell_id).toString(16),hex2=hex.substring(0,hex.length-2),parseInt(hex2,16).toString()),$("#enbid").html(enbid),"22201"==plmn&&(plmn="2221"),"22299"==plmn&&(plmn="22288"),"22250"==plmn&&6==enbid.length&&(plmn="22288"),link_lte="https://lteitaly.it/internal/map.php#bts="+plmn+"."+enbid,$("#lteitaly").attr("href",link_lte)}}),getNetmode(),getStatus(),getAntenna())}function getAntenna(){$.ajax({dataType:"text",type:"GET",async:!0,url:"/api/device/antenna_type",error:err,success:function(n){antenna1type=extractXML("antenna1type",n),antenna2type=extractXML("antenna2type",n),"1"==antenna1type?$("#a1").html("EXT"):$("#a1").html("INT"),"1"==antenna2type?$("#a2").html("EXT"):$("#a2").html("INT")}})}function getNetmode(){$.ajax({type:"GET",dataType:"text",async:!0,url:"/api/net/net-mode",error:err,success:function(n){netmode=n,lteband=extractXML("LTEBand",n),$("#allowed").html(_4GType(lteband))}})}function getStatus(){$.ajax({type:"GET",dataType:"text",async:!0,url:"/api/monitoring/status",error:err,success:function(n){status=n,is4gp=1011==extractXML("CurrentNetworkTypeEx",n)?1:0,is4gp?$("#mode").html("4G+").css("color","red"):$("#mode").html("-").css("color","#aaa")}})}function err(n,e,r){alert("Communication Error"),console.log(n),console.log(e),console.log(r)}function extractXML(n,e){try{return e.split("</"+n+">")[0].split("<"+n+">")[1]}catch(n){return n.message}}function setgraph(n,r,t,a){r=parseInt(r.replace("dBm","").replace("dB")),x=(r-t)/(a-t)*100,xs=String(x)+String.fromCharCode(37),e="#"+n+"b",$(e).animate({width:xs}),$(e).html(n.replace("nr","5G - ")+" : "+window[n]),x<50?$(e).css("background-color","yellow").css("color","black"):(85<x?$(e).css("background-color","orange"):$(e).css("background-color","green")).css("color","white")}function _4GType(n){for(data_out="",x=0;x<90;x++)tb=Math.pow(2,x),color=BigInt("0x"+n)&BigInt(tb)?(data_out+="B"+String(x+1)+"+","#686"):"transparent",$("#cb"+String(x+1)).css("background-color",color);return data_out=data_out.replace(/\++$/,""),data_out}function ltebandselection(n){if(mainband=mainband&&null,0==arguments.length){if(null==(e=(e=prompt("Please input LTE bands number, separated by + char (example 1+3+20).If you want to use every supported bands, write 'AUTO'.","AUTO"))&&e.toLowerCase())||""===e)return}else var e=arguments[0];var n=e.split("+"),t=0;if("AUTO"===e.toUpperCase())t="7FFFFFFFFFFFFFFF";else{for(var r=0;r<n.length;r++){if(-1!=n[r].toLowerCase().indexOf("m")&&(n[r]=n[r].replace("m",""),mainband=n[r]),"AUTO"===n[r].toUpperCase()){t="7FFFFFFFFFFFFFFF";break}t+=Math.pow(2,parseInt(n[r])-1)}t=t.toString(16)}if(mainband)return _2ndrun=n,void ltebandselection(String(mainband));suspend=1,$("#t").html("! PLEASE WAIT !").show(),$.ajax({type:"GET",dataType:"text",async:!0,url:"/html/home.html",error:err,success:function(n){var n=n.split('name="csrf_token" content="'),e=n[n.length-1].split('"')[0],r="00";$("#force4g").is(":checked")&&(r="03"),setTimeout(function(){$.ajax({type:"POST",async:!0,url:"/api/net/net-mode",headers:{__RequestVerificationToken:e},contentType:"application/xml",data:"<request><NetworkMode>"+r+"</NetworkMode><NetworkBand>3FFFFFFF</NetworkBand><LTEBand>"+t+"</LTEBand></request>",success:function(n){$("#band").html('<span style="color:green;">OK</span>'),_2ndrun?window.setTimeout(function(){ltebandselection(_2ndrun.join("+")),_2ndrun=!1},2e3):(suspend=0,$("#t").hide(""))},error:err})},2e3)}})}function ftb(){$(".color_background_blue").css("background-color","#456"),$(".headcontainer").hide(),$("body").prepend('<style> #rsrq,#nrrsrq, #rsrp,#nrrsrp, #rssi, #enbid, #sinr,#nrsinr, #cell_id, #band, #allowed, #a1, #a2 {color: #b00; font-weight: strong; } .f {float: left; border: 1px solid #bbb; border-radius: 5px; padding: 10px; line-height: 2em; margin: 5px; } .f ul {margin: 0; padding: 0; } .f ul li {display: inline; margin-right: 10px; } #mode {margin-right: 0 !important; } #enbid {font-weight: bold; text-decoration: underline; } .p {border-bottom: 1px solid #ccc; width: auto; height: 20px; } .v {height: 20px; border-right: 1px solid #ccc; } .sb {padding: 10px; border-radius: 10px; display: inline-block; margin: 10px 0 10px 10px; } #t {color: white; background-color: #888; margin: 10px; padding: 25px; border-radius: 10px; display: none; text-align: center; font-weight: bolder; } .v {padding-left: 20px; } </style> <div class="p e5"> <div class="v" id="nrrsrpb"></div> </div> <div class="p e5"> <div class="v" id="nrrsrqb"></div> </div> <div class="p"> <div class="v" id="rsrpb"></div> </div> <div class="p"> <div class="v" id="rsrqb"></div> </div> <div style="display:block;overflow: auto;"> <div id="t"></div> <div class="f"> <ul> <li><a style="font-weight:bolder;background-color: #448;color:white;padding: 10px;border-radius:10px;" onclick="ltebandselection()">SET</a></li> <li><label>Force 4G</label><input id="force4g" type="checkbox"></li> </ul> </div> <div class="f"> <ul> <li>RSRP:<span id="rsrp"></span></li> <li>RSRQ:<span id="rsrq"></span></li> <li>RSSI:<span id="rssi"></span></li> <li>SINR:<span id="sinr"></span></li> <li>Ant:<span id="a1"></span>/<span id="a2"></span></li> </ul> </div> <div class="f e5"> <ul> <li>5-RSRP:<span id="nrrsrp"></span></li> <li>5-RSRQ:<span id="nrrsrq"></span></li> <li>5-SINR:<span id="nrsinr"></span></li> </ul> </div> <div class="f"> <ul> <li id="mode">Che la banda sia con te! Miononno &#9829;</li> </ul> </div> <div class="f"> <ul> <li>ENB ID:<a id="lteitaly" target="lteitaly" href="#"><span id="enbid">#</span></a></li> <li>CELL ID:<span id="cell_id">#</span></li> <li>MAIN:<span id="band"></span>(<span id="dlbandwidth"></span>/<span id="ulbandwidth"></span>)</li> <li>ALLOWED:<span id="allowed"></span></li> </ul> </div>')}mainband=null,_2ndrun=null,suspend=0,status="",netmode="",signal="",version="4.0b",console.log("Code by K3rn3l - p"+version),console.log("type: netmode , signal , status"),window.setInterval(currentBand,2500);
```

## Follow the screen!
![Alt text](/img1.1.PNG)

## Selecting "SET" (top left) you can change the desired bands. (Ex: 1+3+7)

![Alt text](/img1.2.PNG)

## Selecting ENB ID: "code" will open the link of your cell location on the map. I use the LTEItaly website, but if you prefer, just replace "https://lteitaly.it/internal/map.php#bts=" in the javascript and set what you want! Remember to create an account on the site!

## Advise:
If the first setting is on the single main band (Ex:3) and the second setting is with the other aggregates (Ex: 3+7+20) there is a greater possibility of always having the main band set to 3..
This is because the javascript code forces the LTE router to set the first band (the best band for us) and the second setting confirms the main + the other aggregates..

This is a tip that works for me, it doesn't mean that it can work for everyone.. LTE router manufacturers always advise to keep the bands on AUTO.. if you are not sure.. at your own risk!

Assuming that your Main is 7 and the aggregate is 3, if we wanted to set it the same way it would be 7+3 and send..
But what if we wanted 3 to be Main?
Simply just add "m" Ex: m3+7.

For the more technical:There are 3 commands usable on the console (once the script is run)
netmode, signal and status. Have fun!

## Setting Subnet and DNS Selection
![Alt text](/img1.3.PNG)

# How-to Part 2 (OLD)
1. First go on to your page. (Ex. 192.168.8.1)
2. Log-in to your device.
3. Run these javascript.
If you add this script to "Bookmark", you can use this tool in iOS Safari.

## Show current LTE band & signal
![Alt text](/img2.png)
```javascript
javascript:currentBand();function currentBand(){$.ajax({type:"GET",async:true,url:"/api/device/signal",error:function(request,status,error){alert("Signal Error:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error)},success:function(data){var report="";report=report+"RSRQ/RSRP/SINR :"+extractXML("rsrq",data)+"/"+extractXML("rsrp",data)+"/"+extractXML("sinr",data);report=report+"\nBand : "+extractXML("band",data)+" - "+extractXML("dlbandwidth",data);report=report+"\nEARFCN : "+extractXML("earfcn",data);alert(report)}})}function extractXML(tag,data){try{return data.split("</"+tag+">")[0].split("<"+tag+">")[1]}catch(err){return err.message}}function ltebandselection(){if(arguments.length==0){var band=prompt("Please input desirable LTE band number. If you want to use multiple LTE bands, write down multiple band number joined with '+'. If you want to use every supported bands, write down 'ALL'. (Ex. SKT 1+3+5+7 / KT 1+3+8 / LG U+ 1+5+7)","ALL");if(band==null||band===""){return}}else var band=arguments[0];if(!window.location.href.includes("/html/home.html")){alert("You can use this function only in main page.");return}else{var bs=band.split("+");var ltesum=0;if(band.toUpperCase()==="ALL"){ltesum="7FFFFFFFFFFFFFFF"}else{for(var i=0;i<bs.length;i++){ltesum=ltesum+Math.pow(2,parseInt(bs[i])-1)}ltesum=ltesum.toString(16);console.log("LTEBand:"+ltesum)}$.ajax({type:"GET",async:true,url:"/html/home.html",error:function(request,status,error){alert("Token Error:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error)},success:function(data){var datas=data.split('name="csrf_token" content="');var token=datas[datas.length-1].split('"')[0];setTimeout(function(){$.ajax({type:"POST",async:true,url:"/api/net/net-mode",headers:{__RequestVerificationToken:token},contentType:"application/xml",data:"<request><NetworkMode>03</NetworkMode><NetworkBand>3FFFFFFF</NetworkBand><LTEBand>"+ltesum+"</LTEBand></request>",success:function(nd){alert(nd)},error:function(request,status,error){alert("Net Mode Error:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error)}})},2e3)}})}}
```

## LTE Band Selection
![Alt text](/img1.png)
```javascript
javascript:ltebandselection();function currentBand(){$.ajax({type:"GET",async:true,url:"/api/device/signal",error:function(request,status,error){alert("Signal Error:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error)},success:function(data){var report="";report=report+"RSRQ/RSRP/SINR :"+extractXML("rsrq",data)+"/"+extractXML("rsrp",data)+"/"+extractXML("sinr",data);report=report+"\nBand : "+extractXML("band",data)+" - "+extractXML("dlbandwidth",data);report=report+"\nEARFCN : "+extractXML("earfcn",data);alert(report)}})}function extractXML(tag,data){try{return data.split("</"+tag+">")[0].split("<"+tag+">")[1]}catch(err){return err.message}}function ltebandselection(){if(arguments.length==0){var band=prompt("Please input desirable LTE band number. If you want to use multiple LTE bands, write down multiple band number joined with '+'. If you want to use every supported bands, write down 'ALL'. (Ex. SKT 1+3+5+7 / KT 1+3+8 / LG U+ 1+5+7)","ALL");if(band==null||band===""){return}}else var band=arguments[0];if(!window.location.href.includes("/html/home.html")){alert("You can use this function only in main page.");return}else{var bs=band.split("+");var ltesum=0;if(band.toUpperCase()==="ALL"){ltesum="7FFFFFFFFFFFFFFF"}else{for(var i=0;i<bs.length;i++){ltesum=ltesum+Math.pow(2,parseInt(bs[i])-1)}ltesum=ltesum.toString(16);console.log("LTEBand:"+ltesum)}$.ajax({type:"GET",async:true,url:"/html/home.html",error:function(request,status,error){alert("Token Error:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error)},success:function(data){var datas=data.split('name="csrf_token" content="');var token=datas[datas.length-1].split('"')[0];setTimeout(function(){$.ajax({type:"POST",async:true,url:"/api/net/net-mode",headers:{__RequestVerificationToken:token},contentType:"application/xml",data:"<request><NetworkMode>03</NetworkMode><NetworkBand>3FFFFFFF</NetworkBand><LTEBand>"+ltesum+"</LTEBand></request>",success:function(nd){alert(nd)},error:function(request,status,error){alert("Net Mode Error:"+request.status+"\n"+"message:"+request.responseText+"\n"+"error:"+error)}})},2e3)}})}}
```

## Thanks to 
https://github.com/0xAA/Huawei_Tool

https://lteitaly.it/
