python使用PIL给图片添加文字生成海报
====================================

`it书童 <https://www.jianshu.com/p/2698af6c5892>`_

前言
----------

曾经，我也算半个所谓的文学青年。大学前两年大部分时间泡在图书馆看各种文学类的书。

那时的我，对于未来有很多遐想：写小说、写时评、写诗歌... 总而言之，就是成为一个文字工作者

现在我确实成为了一个文字工作者，只不过写的是代码...

在某个月黑风高的晚上，看着满屏花花绿绿的代码，揉着酸涩的眼睛，打了一个长长的哈欠。突然进入了禅定时刻：

"还记得年少时的梦吗？“

我又开始想写作了，一个写了几年代码的老男人，在被生活粗暴地摁在地上摩擦几回后，突然触发了内心的柔软，想写些东西。

要写些什么？如何写？在我看来，写作比写代码更难，详见 编程与写作

那就先从最简单的每天更新一篇随笔开始吧，内容包括当天的阅读与思考。这种文章主要是让自己练习写作，类似于编程的刷题。

干巴巴的随笔看起来没什么意思，需要有一张配图，对当天的阅读、写作进行概括性总结。这张图是统一的模板，只是内容不同，如果每张图都要用ps来处理，太繁琐了。作为一个以懒惰为美德的程序员，肯定是想着用程序自动生成图片。

python生成图片海报
----------------------

1. 设计图片模板
~~~~~~~~~~~~~~~

.. image:: https://upload-images.jianshu.io/upload_images/1864602-ad1e252aad73cfb7.png?imageMogr2/auto-orient/strip|imageView2/2/w/870/format/webp
   :alt: 模板

2. 安装python库
~~~~~~~~~~~~~~~

环境：python3

安装Pillow库

.. code:: sh

   pip install Pillow

2. 具体代码实现
~~~~~~~~~~~~~~~

新建 ``index.py``

.. code:: py

   # -*- coding:utf-8 -*-

   from PIL import Image, ImageDraw, ImageFont
   import time

   # 安装库：pip install Pillow

   header = '001'
   title = '日思录第001篇'
   books = ['中国史纲五十讲', '再见拖延症', '心流']
   writes = ['日思录第001篇', 'python给图片加文字']
   summary = '习惯在一个任务开始之前，先给自己设立一个看起来不太可能达到的完美标准，并因为这个标准而迟迟无法动手，那你可能也是一个完美主义者'
   n = 18
   summary_list = [summary[i:i + n] for i in range(0, len(summary), n)]

   # 图片名称
   img = './test.png'  # 图片模板
   new_img = 'text.png'  # 生成的图片
   compress_img = 'compress.png'  # 压缩后的图片

   # 设置字体样式
   font_type = '/System/Library/Fonts/STHeiti Light.ttc'
   font_medium_type = '/System/Library/Fonts/STHeiti Medium.ttc'
   header_font = ImageFont.truetype(font_medium_type, 55)
   title_font = ImageFont.truetype(font_medium_type, 45)
   font = ImageFont.truetype(font_type, 24)
   color = "#000000"

   # 打开图片
   image = Image.open(img)
   draw = ImageDraw.Draw(image)
   width, height = image.size

   # header头
   header_x = 130
   header_y = 690
   draw.text((header_x, height - header_y), u'%s' % header, color, header_font)

   # 标题
   title_x = header_x
   title_y = header_y - 80
   draw.text((title_x, height - title_y), u'%s' % title, color, title_font)

   # 当前时间
   cur_time = time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
   cur_time_x = 590
   cur_time_y = title_y - 25
   cur_time_font = ImageFont.truetype(font_type, 25)
   draw.text((cur_time_x, height - cur_time_y), u'%s' % cur_time, color, cur_time_font)

   # 阅读
   book_x = title_x + 5
   book_start_y = title_y - 190
   book_y = 0
   book_line = 50
   for num, book in enumerate(books):
       y = book_start_y - num * book_line
       book_num = num + 1
       draw.text((book_x, height - y), u'%s. %s' % (book_num, book), color, font)

   # 写作
   write_x = book_x
   write_y = title_y - 450
   write_line = 40

   for num, write in enumerate(writes):
       write_num = num + 1
       y = write_y - num * write_line
       draw.text((write_x, height - y), u'%s. %s' % (write_num, write), color, font)

   # 哲思
   summary_x = book_x + 460
   summary_y = book_start_y
   summary_line = 35
   for num, summary in enumerate(summary_list):
       y = summary_y - num * summary_line
       draw.text((summary_x, height - y), u'%s' % summary, color, font)

   # 生成图片
   image.save(new_img, 'png')

   # 压缩图片
   sImg = Image.open(new_img)
   w, h = sImg.size
   width = int(w / 2)
   height = int(h / 2)
   dImg = sImg.resize((width, height), Image.ANTIALIAS)
   dImg.save(compress_img)

执行结果
--------------

.. code:: sh

   ☁  python  python index.py

.. image:: https://upload-images.jianshu.io/upload_images/1864602-cf2c10b009dbd541.png?imageMogr2/auto-orient/strip|imageView2/2/w/540/format/webp
   :alt: 结果
