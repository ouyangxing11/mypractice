<?php
header("Content-type: image/png; charset='utf-8'");
function vcode($immc=10,$number=4,$size=20,$widht=0,$height=0,$num="0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ",$font=array('ariali.ttf')){
	if($width==0){
		$width = ($number+1)*$size;
	}
	if($height==0){
		$height = $size*2.5;
	}
	
	$im = imagecreatetruecolor($width,$height);//创建画布
	$imcolor = imagecolorallocate($im,rand(160,255),rand(160,255),rand(160,255));
	imagefilledrectangle($im,0,0,$width,$height,$imcolor);//画矩形，填充背景颜色

	$vcode = '';//用来存储验证码
	$fontMax = count($font)-1;//获取字体类型个数
	for($i=0;$i<$number;$i++){	
		$x = $size*0.5+$size*$i;
		$y = rand($size*1.5,$size*2);
		$strMax = strlen($num)-1;//获取字符串长度
		$code = $num[rand(0,$strMax)];//获取随机数
		$vcode .= $code;//将获取出的数拼接在一起
		$randcolor = imagecolorallocate($im,rand(0,130),rand(0,130),rand(0,130));
		imagettftext($im,$size,rand(-10,10),$x,$y,$randcolor,$font[rand(0,$fontMax)],$code);	//向图像写入文本
	}
	$pn = $size*10;//点的个数
	for($j=0;$j<$pn;$j++){
		$imcolor1=imagecolorallocate($im,rand(0,80),rand(0,80),rand(0,80));
		$wx=rand(0,$width);//点的x位置
		$wy=rand(0,$height);//点的y位置
		imagesetpixel($im,$wx,$wy,$imcolor1);//在画布添加点
	}
	for($k=0;$k<$immc;$k++){
		$imcolor2 = imagecolorallocate($im,rand(0,80),rand(0,80),rand(0,80));
		$cx = rand(0,200);
		$cy = rand(0,200);
		$w = rand(0,250);//椭圆的宽度
		$y = rand(0,150);//椭圆的高度
		$s = rand(0,360);//起始点角度
		$e = rand(0,360);//终点角度
		imagearc($im,$cx,$cy,$w,$y,$s,$e,$imcolor2);//在画布添加弧线
		
		
	}
		if(!isset($_SESSION)){
			session_start();
		}
		ob_clean();//清除之前输出
		imagepng($im);
		imagedestroy($im);//销毁图片资源
}
   vcode(10,4);
?>
