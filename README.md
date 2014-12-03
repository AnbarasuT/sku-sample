sku-sample
==========

<!DOCTYPE html>
<html>
<head>
<script src="jquery-2.0.2.js" type="text/javascript"></script>
<script src="jquery-ui.js"  type="text/javascript"></script>
<style>
#container
{
position:absolute;
margin:0px;
padding:0px;
width:765px;
height:415px;
margin:auto;
left:0px;
right:0px;
top:0px;
border:1px solid  #000000;
font-family: myFirstFont;
}
@font-face
{
font-family: myFirstFont;
src: url('comic.ttf');
}
#topdiv
{
position:absolute;
margin:0px;
padding:0px;
width:700px;
height:100px;
margin-left:50px;
top:130px;
}
#drop1
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
background:#E6EFF6;
left:120px;
top:10px;
border-radius:5px;
border:3px dotted #AECBE5;

}
#drop2
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
background:#E6EFF6;
left:230px;
top:10px;
border-radius:5px;
border:3px dotted #AECBE5;

}
#drop3
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
background:#E6EFF6;
left:340px;
top:10px;
border-radius:5px;
border:3px dotted #AECBE5;

}
#drop4
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
background:#E6EFF6;
left:450px;
top:10px;
border-radius:5px;
border:3px dotted #AECBE5;

}

#bottomdiv
{
position:absolute;
margin:0px;
padding:0px;
width:700px;
height:150px;
margin:auto;
top:250px;
margin-left:50px;


}

#drag1
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
left:10px;
top:20px;
border:1px solid #FFFFFF;
border-radius:5px;
}
#drag2
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
left:120px;
top:20px;
border:1px solid #FFFFFF;
border-radius:5px;
}
#drag3
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
left:230px;
top:20px;
border:1px solid #FFFFFF;
border-radius:5px;
}
#drag4
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
left:340px;
top:20px;
border:1px solid #FFFFFF;
border-radius:5px;
}
#drag5
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
left:450px;
top:20px;
border:1px solid #FFFFFF;
border-radius:5px;
}
#drag6
{
position:absolute;
margin:0px;
padding:0px;
width:80px;
height:100px;
left:560px;
top:20px;
border:1px solid #FFFFFF;
border-radius:5px;
}
.audio
{
position:absolute;
margin:0px;
padding:0px;
display: inline-block;
width:20px;
height:20px;
border-radius:20px;
margin-left:70px;
top:88px;

}
</style>
<script type="application/javascript" src="content.js"></script>
<script>
    var loadedContOrder = [];
    var dragObjTemp = ["<div id=\"drag",
                       "\" class=\"dragobj ",
                       "\"><img src=\"images/",
                       "\" width=\"80\" height=\"100\" /> <div class=\"audio\" onClick=\"audioFunc(this)\" id=\"audio01\"><audio id=\"audio1\" src=\"audio/",
                       "\" type=\"audio/mpeg\"></audio><img src=\"images/audio_icon.png\" width=\"30\" height=\"30\" /></div></div>"];
    
    function ready() {
        loadContent();
        bindEvents();
    }
    function loadContent(isRepeat) {
        if (loadedContOrder.length == inputOrder.length && !isRepeat) {
            alert("completed");
            return;
        }
        var type;
        if (isRepeat) {
            type = loadedContOrder[loadedContOrder.length-1];
        }
        else{
            type = inputOrder[Math.floor(Math.random()*inputOrder.length)];
            while (loadedContOrder.indexOf(type) >= 0) {
                type = inputOrder[Math.floor(Math.random()*inputOrder.length)];
            }
            loadedContOrder.push(type);
        }
        clearExisting();
        $("#nextBtn").hide();
        var opWithCont = inputSource[type+"_set"];
        if (opWithCont != null) {
            var itemToTake = Math.floor(Math.random()*opWithCont.length);
            var q2Ld = opWithCont[itemToTake].objSet;
            var cont = "";
            var shuffledOrder = [];
            for (var ind=0; ind<q2Ld.length; ind++) {
                var val = Math.floor(Math.random()*q2Ld.length);
                while(shuffledOrder.indexOf(val) >= 0)val = Math.floor(Math.random()*q2Ld.length);
                shuffledOrder.push(val);
                var currItem = val;
                var res = (q2Ld[currItem].value.toLowerCase().lastIndexOf(type) == q2Ld[currItem].value.toLowerCase().length-type.length);
                cont += dragObjTemp[0]+(ind+1)+dragObjTemp[1]+res+dragObjTemp[2]+q2Ld[currItem].imagePath+dragObjTemp[3]+q2Ld[currItem].audioPath+dragObjTemp[4];
            }
            $("#bottomdiv").html(cont);
        }
        $("#headingQuest").html("-"+type);
    }
    
    function bindEvents(){
        $('.dragobj').draggable({containment:'#container',revert:'invalid', drag:function(){$('#'+this.id).find('.audio').hide()}, stop: function(){
        if($(this).parent().parent().attr('id') == 'topdiv') $('#'+this.id).find('.audio').hide();
        else $('#'+this.id).find('.audio').show()
        }, stack:'.dragobj'});
        
        $('.droppableArea').droppable({
            drop:function(event, ui){
                $(event.target).append(ui.draggable);
                $(ui.draggable).css({"top":"0px","left":"0px"});
                $(event.target).droppable("option", "disabled", true);
            }
        });
        
        $('#bottomdiv').droppable({
            drop:function(event, ui){
                $(event.target).append(ui.draggable);
                $(ui.draggable).attr({"style":""});
            }
        });
    }
    
    function beforeDrop(draggable, event) {
        for (var i=0; i<$(".droppableArea").length; i++) {
            if($($(".droppableArea")[i]).children().length == 0) $($(".droppableArea")[i]).droppable("option", "disabled", false);
        }
    }
    function clearExisting() {
        //for (var i=0; i<$(".droppableArea").length; i++) $($(".droppableArea")[i]).children().remove();
        $(".dragobj").remove();
    }
    function validate() {
        var result = false;
        for (var i=0; i<$(".droppableArea").length; i++) {
            if($($(".droppableArea")[i]).children().length == 0){
                result = false;
                break;
            }
            else{
                if($($(".droppableArea")[i]).children()[0].className.indexOf("true") >=0) result = true;
                else{result = false; break;}
            }
        }
        if (result){
            alert("correct");
        }
        else{
            alert("Wrong");
            showCorrect();
        }
        $("#nextBtn").show();
    }
    
    function showCorrect() {
        $(".audio").hide();
        for (var i=0; i<$(".dragobj").length; i++) {
            $($(".dragobj")[i]).draggable('disable');
        }
        for (var i=0; i<$(".droppableArea").length; i++) {
            if($($(".droppableArea")[i]).children().length > 0){
                if($($(".droppableArea")[i]).children()[0].className.indexOf("true") < 0){
                    $($($(".droppableArea")[i]).children()[0]).attr({"style":""});
                    $("#bottomdiv").append($($(".droppableArea")[i]).children()[0]);
                }
            }
            if($($(".droppableArea")[i]).children().length == 0){
                for (var j=0; j<$("#bottomdiv").children().length;j++) {
                    if ($("#bottomdiv").children()[j].className.toString().indexOf("true") >= 0) {
                        $($("#bottomdiv").children()[j]).css({"top":"0px","left":"0px"});
                        $($(".droppableArea")[i]).append($("#bottomdiv").children()[j]);
                        break;
                    }
                }
            }
        }
    }
    
    function loadNext() {
        loadContent();
        bindEvents();
    }
</script>
</head>
<body onload="ready();">
<div id="container">
<div style="position:absolute; width:20px; height:20px; border-radius:20px; top:45px; margin-left:45px;">
<img style=" position:absolute; left:-20px; top:-20px;" src="images/arow1.png" width="10" height="15" />
<img style=" position:absolute; left:0px; top:5px;" src="images/audio_icon.png" width="30" height="35" /></div>
<p style="position:absolute; font-size:24px; margin-left:40px; top:-10px; color:#404040;">Word Families Set A</p>
<p style="position:absolute; top:30px; margin-left:80px; font-size:20px; color:#808080; ">Say the name of each picture. Choose the pictures that belong in the word family.<p>
<p id="headingQuest" style=" position:absolute; font-size:60px; color:#FF0000; top:-5px; margin-left:330px;"> -at</p>
<div id="topdiv">
    <div id="drop1" class="droppableArea"></div>
    <div id="drop2" class="droppableArea"></div>
    <div id="drop3" class="droppableArea"></div>
    <div id="drop4" class="droppableArea"></div>
</div>

<div id="bottomdiv"></div>

<p style="position:absolute; left:620px; top:383px;  color:#339900; font-size:10px; cursor: pointer" onClick="validate()"><b>CHECK MY WORK</b></p>
<img  style="position:absolute; left:590px; top:387px; border-radius:25px; width:25px; height:25px; cursor: pointer; " src="images/pencil.png" width="30" height="30" onClick="validate()"\>
<div id="nextBtn" style="position:absolute; left:723px; top:387px; border-radius:25px; width:25px; height:25px; cursor: pointer;" onClick="loadNext();">Next</div>
</div>
</body>
</html>



content.js
============

var inputOrder = ["an","at","en","in","it","og","ug","y"];
var inputSource = {
    at_set:[
            {
                objSet:[
                    {
                        value:"hat",
                        imagePath:"img-01.png",
                        audioPath:"hat.mp3"
                    },
                    {
                        value:"bat",
                        imagePath:"img-02.png",
                        audioPath:"bat.mp3"
                    },
                    {
                        value:"knit",
                        imagePath:"img-03.png",
                        audioPath:"knit.mp3"
                    },
                    {
                        value:"tan",
                        imagePath:"img-04.png",
                        audioPath:"tan.mp3"
                    },
                    {
                        value:"flat",
                        imagePath:"img-05.png",
                        audioPath:"flat.mp3"
                    },
                    {
                        value:"rat",
                        imagePath:"img-06.png",
                        audioPath:"rat.mp3"
                    },
                ],
            },
        ],
    it_set:[
            {
                objSet:[
                    {
                        value:"july",
                        imagePath:"JULY.png",
                        audioPath:"july.mp3"
                    },
                    {
                        value:"fin",
                        imagePath:"FIN.png",
                        audioPath:"fin.mp3"
                    },
                    {
                        value:"split",
                        imagePath:"SPLIT.png",
                        audioPath:"knit.mp3"
                    },
                    {
                        value:"sit",
                        imagePath:"SIT.png",
                        audioPath:"tan.mp3"
                    },
                    {
                        value:"knit",
                        imagePath:"KNIT.png",
                        audioPath:"flat.mp3"
                    },
                    {
                        value:"rabbit",
                        imagePath:"RABBIT.png",
                        audioPath:"rat.mp3"
                    },
                ],
            },
        ],
    };



