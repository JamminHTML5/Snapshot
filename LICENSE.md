<!DOCTYPE html5>
<html>
<head>
<title>Video Snapshot</title>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">

<script>

// ProgressBar - 1024

var bar, slider, w=0;

function initProgressBar()
{
v1 = document.getElementById('video1');
pp = document.getElementById('pp');

setInterval(readout, 30);

bar = document.getElementById('bar');
slider = document.getElementById('slider');
bar.addEventListener('mousedown', startSlide, false);	
bar.addEventListener('mouseup', stopSlide, false);
}
 
function startSlide(event)
{
var set_w = event.clientX - bar.offsetLeft.toFixed(0);
bar.addEventListener('mousemove', moveSlide, false);	
set_ct = video1.currentTime;
set_tt = video1.duration;
video1.currentTime = set_w/966 * set_tt;  // Set currentTime, & the green bar will go to it.
document.getElementById('disc').style.left = set_w - 12;
}
 
function moveSlide(event)
{
var set_w = event.clientX - bar.offsetLeft.toFixed(0);
set_tt = video1.duration;
video1.currentTime = set_w/966 * set_tt;  // Set currentTime, & the green bar will go to it.

document.getElementById('disc').style.left = set_w - 12;
}

function stopSlide(event)
{
var set_w = event.clientX - bar.offsetLeft.toFixed(0);
bar.removeEventListener('mousemove', moveSlide, false);
set_tt = video1.duration;
video1.currentTime = set_w/966 * set_tt;  // Set currentTime, & the green bar will go to it.
document.getElementById('disc').style.left = set_w - 12;
}

function readout()
{
var ct = video1.currentTime, tt = video1.duration, ct_readout='', tt_readout='';
w = ct/tt * 966;

if(ct > 59.999999) ct_readout = '' + Math.floor(ct / 60) + ':'; // 59.000001 to 59.999999
if(ct > 59 && ct % 60 < 10) ct_readout += '0';
ct_readout += Math.floor(ct % 60);

if(tt > 59) tt_readout = '' + Math.floor(tt / 60) + ':';
if(tt > 59 && tt % 60 < 10) tt_readout += '0';
tt_readout += Math.floor(tt % 60);

bothTimesWhole.style.marginRight="60px";
if(ct <= tt)  // Without the if, NAN shows briefly, & the right side of the cell jumps.
  document.getElementById('bothTimesWhole').innerHTML='' + ct_readout + ' / ' + tt_readout;

slider.style.width = w;
disc.style.left = w - 12;
}

// Video Player Controls

v1 = document.getElementById('video1');
pp = document.getElementById('pp');
var isShown = new Boolean();  // Volume Slider

function halt() { 
v1.currentTime=0;
v1.pause();
pp.innerHTML="&#9658;";
}

function play_pause() { 
  if (v1.paused) {
    v1.play();
    pp.style.paddingLeft="1px"
    pp.style.top="-4px";
    pp.style.fontSize="9pt";
    pp.innerHTML="&#9612;&#9612;";
  } 
  else {
    v1.pause();
    pp.style.paddingLeft="0px"
    pp.style.top="0px";
    pp.style.fontSize="15pt";
    pp.innerHTML="&#9658;";
  } 
}

function show_hide_vol() {
if(isShown) { document.getElementById("vol_div").style.display = "none"; isShown = false; }
else { document.getElementById("vol_div").style.display="inherit"; isShown = true; }
}
</script>

<style type='text/css'>

/* Snapshot */

#bar{
    background-color:white;
    border:1px solid black;
    position:relative;
    top:8px;
    margin-left:19px;
    width:966px; height:11px;
    border-radius: 13px}

#slider{
	background-color:lime;
	position:absolute;
	top:0px;
	left:0px;
	width:0px;
	height:100%;
    border-radius: 10px; /* (height of inner div) / 2 + padding */
}

#disc{
	position:absolute;
	width:20px;
	height:20px;
	top:-4px;
	left:-10px;
    background-image:url('https://lh3.googleusercontent.com/-RGZnjDrICzA/VPeADKsbRjI/AAAAAAAAAEE/jLP6zDCDnFc/s512/smiley_red.png');
    background-size:20px 20px;
    background-repeat:no-repeat;
    background-position:0px 0px;
}

.disp {
    display:inline;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

.ptr {cursor:default}

#cs {position:relative; top:2px; cursor:default}
#st {position:relative; top:-1px; font-size:17pt; cursor:default}
#pp {position:relative; top:0px; font-size:15pt; cursor:default}
#sf {position:relative; top:-1px; font-size:13pt; cursor:default}
#sb {position:relative; top:-1px; font-size:13pt; cursor:default}
#sp {position:relative; top:5px; cursor:default}

#vol_div {
position:absolute;
left:-55px;
top:-85px;
background-color:lightblue;
width:0px;
}

.rotateLeft {
-ms-transform: rotate(-90deg); /* IE 9 */
-webkit-transform: rotate(-90deg); /* Chrome, Safari, Opera */
transform: rotate(-90deg);
}

/* Snapshot */

#master{ border:1px solid black; text-align:center }
#video1{ max-height:320px }

.ProgBarHolder{
background-color:blue;
border-top: 1px solid black;
height:30px;
}

#thumboutput {margin-top:5px; margin-bottom:5px}
</style>
</head>

<body onload='initProgressBar(); document.getElementById("vol_div").style.display="none"; isShown=false'>

<div id="master">

<video id="video1" poster="Kodak - Crop.jpg" src="" onended="halt()"></video>

<div class="ProgBarHolder">
  <div id='bar'>
    <div id='slider'></div>
    <div id='disc'></div>
  </div>
</div>

<table cellspacing="0" cellpadding="0" style="background-color:blue;color:white;width:100%">
  <tr>
    <td style="text-align:right; width:100px">
      <input type="file" id="filesToUpload" accept="video/*" style="display:none" onChange="makeFile()" />
        <button style="position:relative;top:-2px" onClick="filesToUpload.click()">Select Video</button>
    </td>
    <td style="text-align:center; width:120px" id="bothTimesWhole">&nbsp;</td>
    <td id="vidtitle" style="color:white;font-size:20px;text-align:center">Snapshot</td>
      <td class="tdata" style="text-align:right; width:180px" valign="top">
        <div id="menu" style='height:30px'>
          <li id="cs" class="disp" onclick="this.blur();shoot()"><img src="camera-icon.png" />&nbsp;</li>
          <li id="st" class="disp" onclick="this.blur();halt()">&#8718;&nbsp;</li>
          <li id="pp" class="disp" onclick="this.blur();play_pause()">&#9658;</li>
          <li id="sf" class="disp" onclick="this.blur();video1.currentTime -= 0.03;">&#10703;</li>
          <li id="sb" class="disp" onclick="this.blur();video1.currentTime += 0.03;">&#10704;&nbsp;</li>
          <li id="sp" class="disp" onclick="this.blur(); show_hide_vol()">
            <div id="vol_div">
              <input class="rotateLeft" type="range" min="0" max="100" step="1" value="100" onchange="video1.volume = this.value * 0.01" />
            </div>
            <img src="https://lh5.googleusercontent.com/-7UAEJHjZktA/VPeADO6pquI/AAAAAAAAAEI/XSYsJKEkL4I/s512/volume-full-icon-64.png" height="24"/>
            &nbsp;&nbsp;
          </li>
        </div> 
      </td>
  </tr>
</table>

</div>  <!-- End master -->

<div id="durloutput"></div>
<div style='background-color:red; color:white; margin-top:5px; margin-bottom:5px; text-align:center'>
Click a thumbnail to view it. Then, right click to save it.</div>
<div style='text-align:center'><img id="dURL" src='' height='240px'/></div>
<div id="canvasoutput"></div>

<script>
var thumbs = [];
var v1, pp, n=0;

function capture(video) {
var w = video.videoWidth;
var h = video.videoHeight;
var canvas = document.createElement('canvas');
    canvas.width  = w;
    canvas.height = h;
var ctx = canvas.getContext('2d');
    ctx.drawImage(video, 0, 0, w, h);

        return canvas;
} 
 
function shoot() {
var video1  = document.getElementById('video1');
var durloutput = document.getElementById('durloutput');

var thumbcanvas = capture(video1);
var dataURL = thumbcanvas.toDataURL();
var oImg = new Image();
oImg.src = dataURL;

oImg.id = '' + n;
oImg.style.marginTop='5px';
oImg.style.marginRight='5px';
oImg.style.border='2px solid blue';
oImg.style.height='60px';

oImg.onclick = function() {
  var idnum = this.id;  // 'this' refers to thumbcanvas.

  for(k=0; k < thumbs.length; k++) {
    durloutput.appendChild(thumbs[k]);
    thumbs[k].style.border=' 2px solid yellow';
  }

  this.style.border='2px solid blue';

//  thumbsoutput.innerHTML='';
  dURL.src = thumbs[idnum].src;
};

for(j=0; j < thumbs.length; j++) {
  durloutput.appendChild(thumbs[j]);
  thumbs[j].style.border=' 2px solid yellow';
  
}
    
thumbs.push(oImg);
oImg.style.height='60px';
durloutput.appendChild(oImg);
    
document.getElementById('dURL').src=dataURL;
n++;
}

function makeFile() {
var input = document.getElementById('filesToUpload');

for(var i = 0; i < input.files.length; i++) {
  var oFilename = input.files[i].name;
  var samplepos=oFilename.lastIndexOf('.');
  var ext = oFilename.substr(samplepos);
  if(ext=='.mp4' || ext=='.webm' || ext=='.ogv') {
    var oTitle = oFilename.substr(0, samplepos);
    document.getElementById('vidtitle').innerHTML=oTitle;
    
    video1.poster='';

    // createObjectURL creates the blob for the file!
    var oBlob = URL.createObjectURL(event.target.files[i]);
    video1.src = oBlob;
  }  // End if(ext ...
}    // End for(var i = 0; ...
document.getElementById('pp').innerHTML="&#9658;";
}    // End makeFile()
</script>
</body>
</html>
