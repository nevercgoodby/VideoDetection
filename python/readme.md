原理：将图片转换为YCbCr模式，在图片中寻找图片色值像素，如果在皮肤色值内的像素面积超过整个画面的1/3，就认为是黄色图片。

申明：简单场景还是够用了，稍微复杂一点就不准确了，例如：整幅画面是人的头像，皮肤色值的像素必然超过50%，被误认为黄色图片就太武断了。

需要安装python图片库PIL支持

porn_detect.py

import sys,PIL.Image as Image
img = Image.open(sys.argv[1]).convert('YCbCr')
w, h = img.size
data = img.getdata()
cnt = 0
for i, ycbcr in enumerate(data):
    y, cb, cr = ycbcr
    if 86 <= cb <= 117 and 140 <= cr <= 168:
        cnt += 1
print '%s %s a porn image.'%(sys.argv[1], 'is' if cnt > w * h * 0.3 else 'is not')
运行： 
python porn_detect.py myphoto.png


https://my.oschina.net/waterbear/blog/149867
