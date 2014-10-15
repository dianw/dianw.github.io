---
layout: post
title: Konversi Nilai Resistor dengan JQuery
comments: true
---
&nbsp;&nbsp;&nbsp; Beberapa waktu yang lalu saya mendapatkan tugas dari Bapak Guru tercinta :p untuk membuat sebuah aplikasi yang dapat merubah inputan dari user berupa angka menjadi gelang warna pada resistor, sempat kepikiran untuk membangun aplikasi tersebut menggunakan swing, tapi ga tau kenapa tiba-tiba seperti ada bisikan di otak saya, ?gimana kalo makan dulu bro?? dan saat itu juga saya berpikir ?gmn kalo pake JQuery aja?? (g nyambung :D). Dan Walhasil, abrakadabra, jadilah sebuah mahakarya yang sangat dahsyat!
<div id="container" class="well">
 <style type="text/css">
            .gelang {
                background-color:transparent;
                width:20px;
                height:100px;
                margin:1px;
                float:left;
                display:none;
                border-style:solid;
                border-width:1px;
            }
            label {
                font-weight:bold;
            }
        </style>
 <script type="text/javascript">
            var warna = [
                "black",
                "#993300",
                "red",
                "#ff8a00",
                "yellow",
                "green",
                "blue",
                "purple",
                "silver",
                "white",
                "#f6fd2e",
                "#b6b6b6"
            ];
            function reset() {
                $('#1, #2, #3, #4, #5').hide().css("background-color", "#FFFFFF");
            }
            function check(str) {
                reset();
                var inp = str;
                var len = inp.length;
                var gel = $('#jml').val();
                $('#3').fadeIn();
                if(len <= 2) {
                    $('#1').css("background-color", warna[inp[0]]).fadeIn();
                    $('#2').css("background-color", warna[inp[1]]).fadeIn();
                  
                } else {
                    var temp = "";
                    for(i = 0 ; i < len ; i++) {
                        if(i < gel) {
                            temp += inp[i];
                        } else {
                            temp += '0';
                        }
                    }
                    $('#input').val(temp);
                    var nol = len - gel;
                    $('#1').css("background-color", warna[inp[0]]).fadeIn();
                    $('#2').css("background-color", warna[inp[1]]).fadeIn();
                  
                    if(gel == 2) {
                        $('#3').css("background-color", warna[nol]).fadeIn();
                    } else {
                        $('#3').css("background-color", warna[inp[2]]).fadeIn();
                        $('#4').css("background-color", warna[nol]).fadeIn();
                    }
                    //alert(len - gel);
                }
            }
            $(function() {
                // $('#input').focus();
                $('#input').keydown(function(e) {
                  
                    if(e.keyCode == 8) {
                        return true;
                    } else if(e.keyCode == 13) {
                        $('#submit').click();
                        return true;
                    } else if(e.keyCode >= 48 && e.keyCode <= 57) {
                        return true;
                    }
                    return false;
                });
                $('#jml').change(function() {
                    if($(this).val() == 2) {
                        $('#4').hide();
                    } else {
                        $('#4').show();
                    }
                    $('#submit').click();
                }).change();
                $('#toleransi').change(function() {
                    $('#submit').click();
                }).change();
                $('#submit').click(function() {
                    check($('#input').val());
                    $('#5').css("background-color", warna[$('#toleransi').val()]).fadeIn();
                });
            });
        </script>
 <div style="text-align: center;">
 
  <div>
  
   <label>
 Nilai Hambatan : &nbsp;
 <input type="text" size="10" id="input">
 &nbsp; Ω
 </label>
  </div>
  <div style="margin:20px;">
  
   <label>
 Toleransi : &nbsp;
 <select id="toleransi">
 <option value="1">1%</option>
 <option value="10">5%</option>
 <option value="11">10%</option>
 <option value="9">20%</option>
 </select>
 &nbsp;
 </label>
  </div>
  <div style="margin:20px;">
  
   <label>
 Jumlah Gelang Utama : &nbsp;
 <select id="jml">
 <option value="2">3</option>
 <option value="3">4</option>
 </select>
 </label>
  </div>
  <div style="margin:20px;text-align:center;">
  
   <button id="submit">Submit</button>
  </div>
 </div>
 <table align="center" id="tb">
 
  <tbody>
  
   <tr>
   
    <td>
     <div id="1" class="gelang"></div>
     <div id="2" class="gelang"></div>
     <div id="3" class="gelang"></div>
     <div id="4" class="gelang"></div>
     <div id="5" class="gelang"></div>
 </td>
   </tr>
  </tbody>
 </table>
</div>
<br>
Dan berikut source codenya :
{% codeblock lang:html %}
<title>Converter</title>
<style type="text/css">
            .gelang {
                background-color:transparent;
                width:20px;
                height:100px;
                margin:1px;
                float:left;
                display:none;
                border-style:solid;
                border-width:1px;
            }
            label {
                font-weight:bold;
            }
        </style>
<script type="text/javascript" src="http://code.jquery.com/jquery-1.4.2.min.js"></script>
<script type="text/javascript">
            var warna = [
                "black",
                "#993300",
                "red",
                "#ff8a00",
                "yellow",
                "green",
                "blue",
                "purple",
                "silver",
                "white",
                "#f6fd2e",
                "#b6b6b6"
            ];
            function reset() {
                $('#1, #2, #3, #4, #5').hide().css("background-color", "#FFFFFF");
            }
            function check(str) {
                reset();
                var inp = str;
                var len = inp.length;
                var gel = $('#jml').val();
                $('#3').fadeIn();
                if(len <= 2) {
                    $('#1').css("background-color", warna[inp[0]]).fadeIn();
                    $('#2').css("background-color", warna[inp[1]]).fadeIn();
                  
                } else {
                    var temp = "";
                    for(i = 0 ; i < len ; i++) {
                        if(i < gel) {
                            temp += inp[i];
                        } else {
                            temp += '0';
                        }
                    }
                    $('#input').val(temp);
                    var nol = len - gel;
                    $('#1').css("background-color", warna[inp[0]]).fadeIn();
                    $('#2').css("background-color", warna[inp[1]]).fadeIn();
                  
                    if(gel == 2) {
                        $('#3').css("background-color", warna[nol]).fadeIn();
                    } else {
                        $('#3').css("background-color", warna[inp[2]]).fadeIn();
                        $('#4').css("background-color", warna[nol]).fadeIn();
                    }
                    //alert(len - gel);
                }
            }
            $(function() {
                $('#input').focus();
                $('#input').keydown(function(e) {
                  
                    if(e.keyCode == 8) {
                        return true;
                    } else if(e.keyCode == 13) {
                        $('#submit').click();
                        return true;
                    } else if(e.keyCode >= 48 && e.keyCode <= 57) {
                        return true;
                    }
                    return false;
                });
                $('#jml').change(function() {
                    if($(this).val() == 2) {
                        $('#4').hide();
                    } else {
                        $('#4').show();
                    }
                    $('#submit').click();
                }).change();
                $('#toleransi').change(function() {
                    $('#submit').click();
                }).change();
                $('#submit').click(function() {
                    check($('#input').val());
                    $('#5').css("background-color", warna[$('#toleransi').val()]).fadeIn();
                });
            });
        </script>
<div style="text-align:center;width:100%;height:100px;">
</div>
<div style="text-align:center;width:100%;">
 <div>
 
  <label>
 Nilai Hambatan : &nbsp;
 <input type="text" size="10" id="input">
 &nbsp; Ω
 </label>
 </div>
 <div style="margin:20px;">
 
  <label>
 Toleransi : &nbsp;
 <select id="toleransi">
 <option value="1">1%</option>
 <option value="10">5%</option>
 <option value="11">10%</option>
 <option value="9">20%</option>
 </select>
 &nbsp;
 </label>
 </div>
 <div style="margin:20px;">
 
  <label>
 Jumlah Gelang Utama : &nbsp;
 <select id="jml">
 <option value="2">3</option>
 <option value="3">4</option>
 </select>
 </label>
 </div>
 <div style="margin:20px;text-align:center;">
 
  <button id="submit">Submit</button>
 </div>
</div>
<table align="center" id="tb">
 <tbody>
  <tr>
  
   <td>
    <div id="1" class="gelang"></div>
    <div id="2" class="gelang"></div>
    <div id="3" class="gelang"></div>
    <div id="4" class="gelang"></div>
    <div id="5" class="gelang"></div>
 </td>
  </tr>
 </tbody>
</table>
 {% endcodeblock %}