---
layout: post
status: publish
published: true
title: '[翻译]iOS 高级编程之 The Image picker'
author: JohnnieFucker
excerpt: "<p>哥好多年不写，不翻译，不转载技术文章了。想想其实挺可耻的：）自己接触ios开发时间也不长，而且按照哥的风格也不是原样直译作者的文章，如果有错误欢迎订正我的邮箱是：crazysperm@gmail.com</p>\r\n<p>原文<a
  href=\"http://mobileorchard.com/ios-advanced-programming-the-image-picker/\" target=\"_blank\">《iOS
  Advanced Programming: The Image picker》</a>  作者：OSCARVGG </p>\r\n"
wordpress_id: 374
wordpress_url: http://www.oushit.com/?p=374
date: '2011-07-29 01:35:23 +0800'
date_gmt: '2011-07-28 17:35:23 +0800'
category: Some Words
tags:
- 翻译
- ios
- imagepicker
comments:
- id: 262
  author: oBlank
  author_email: dyh1919@gmail.com
  author_url: http://oblank.com
  date: '2011-08-14 15:42:38 +0800'
  date_gmt: '2011-08-14 07:42:38 +0800'
  content: "收藏。。。。。\r\n我买的书还没看啊啊啊 :em08:"
---
<p>哥好多年不写，不翻译，不转载技术文章了。想想其实挺可耻的：）自己接触ios开发时间也不长，而且按照哥的风格也不是原样直译作者的文章，如果有错误欢迎订正我的邮箱是：crazysperm@gmail.com</p>
<p>原文<a href="http://mobileorchard.com/ios-advanced-programming-the-image-picker/" target="_blank">《iOS Advanced Programming: The Image picker》</a>  作者：OSCARVGG </p>
<p><!--break--><a id="more-374"></a></p>
<blockquote><p>
<strong>写在前面</strong></p>
<p>当你阅读这篇教程之前，你应当评估一下你自己是否真的已经足够了解iphone开发的基础知识以便顺利的学习这个“高级编程”系列。如果你还不太确定的话，我建议你阅读“how to make an iphone app”（如何开发iphone应用）这个基础系列的教程。</p>
<p>在本系列教程中，我将为你讲解一些需要更多编程经验的iphone开发技巧。我们将循序渐进，由浅入深。我将假设你已经熟练掌握了在“how to make an iphone app”系列教程中所学到的基础的obj c编程知识。</p>
<p><strong>The Image Picker</strong></p>
<p>imgae picker是一个让你可以从相册缩略图列表中选取图片或是用摄像头拍摄照片并做修改的控件。在这个教程中我们将选取已经存在的图片或者重新拍摄一张(如果设备上有摄像头)并且将其读取到一个image view中。</p>
<p>打开xcode并且创建一个view-based工程，你可以随便命名，在教程中我创建的工程叫做“imagePickerApp”。首先我们打开imagePickerAppViewController类的头文件（.h）我们需要在这个文件中声明ImagePickerController以及我们用来处理所选择图片的view。你需要声明你的类为ImagePickerController 以及NavigationController的代理（delegate）。</p>
<p>类的声明如下：</p>
<p>@interface imagePickerAppViewController : UIViewController {</p>
<p>{</p>
<p>UIImagePickerController *picker;</p>
<p>IBOutlet UIImageView * selectedImage;</p>
<p>}</p>
<p>在interface builder的图形界面中我们添加一个按钮用来调用image picker，并且添加一个按钮的click响应事件，别忘了添加一个uiimageview的对象属性，来获取或者设置所选择的图片。代码如下：</p>
<p>@property (nonatomic, retain) UIImageView * selectedImage;</p>
<p>- (IBAction) buttonClicked;</p>
<p>@end</p>
<p>现在让我们编辑我们的实现文件（.m）添加</p>
<p>@synthesize selectedImage;</p>
<p><strong>图片选择的来源</strong></p>
<p>我们添加按钮click事件的响应，在这个响应函数中实现让用户通过image picker选取他所想要的图片。首先我们初始化image picker并且设置它的代理为self，然后我们判断设备是否有摄像头。如果有，我们通过摄像头拍摄一张照片来使用，否则我们打开相册中图片的列表给用户选择其中的图片。如果你没有判断图片的来源是否存在（是否拥有摄像头），如果你没有在事前判断，那么apple可能会拒绝你的应用。你的应用程序会崩溃（没有摄像头却调用拍照的方法）。</p>
<p>按钮click方法详细的代码如下</p>
<p>-(IBAction) buttonClicked {</p>
<p>picker = [[UIImagePickerController alloc] init];</p>
<p>picker.delegate = self;</p>
<p>if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera])</p>
<p>{</p>
<p>picker.sourceType = UIImagePickerControllerSourceTypeCamera;</p>
<p>} else</p>
<p>{</p>
<p>picker.sourceType = UIImagePickerControllerSourceTypePhotoLibrary;</p>
<p>}</p>
<p>[self presentModalViewController:picker animated:YES];</p>
<p>}</p>
<p>是不是很简单？让我们接着往下。</p>
<p><strong>选取图片</strong></p>
<p>image picker有两个动作：选择一张图片，取消。你可以实现两个内建的方法magePickerControllerDidCancel（取消）和 imagePickerControllerdidFinishPickingMediaWithInfo（选择）来处理这两个动作。虽然方法的名字很长，但是却很清晰的表达了各自所处理的事情。</p>
<p>如果用户取消，我们只需要解散picker并且释放它的对象。</p>
<p>- (void)imagePickerControllerDidCancel:(UIImagePickerController *) Picker {</p>
<p>[[Picker parentViewController] dismissModalViewControllerAnimated:YES];</p>
<p>[Picker release];</p>
<p>}</p>
<p>如果用户选择或是拍摄了一张图片，我们将它分配给“selectedImage” 这个对象，可以通过selectedImage的image属性来访问它，然后我们释放picker对象。</p>
<p>- (void)imagePickerController:(UIImagePickerController *) Picker</p>
<p>didFinishPickingMediaWithInfo:(NSDictionary *)info {</p>
<p>selectedImage.image = [info objectForKey:UIImagePickerControllerOriginalImage];</p>
<p>[[Picker parentViewController] dismissModalViewControllerAnimated:YES];</p>
<p>[Picker release];</p>
<p>}</p>
<p><strong>创建界面</strong><br />
现在我们启动interface builder来编辑resources文件夹下的imagePickerAppViewController.xib（新的xcode4已经不会默认的将xib放在resources文件夹下，并且interface builder已经集成到了xcode中）<br />
拖一个imageview和一个button控件到应用的窗口中<br />
<img src="http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-1.png" alt="" /><br />
然后关联按钮的点击事件<br />
<img src="http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-2.png" alt="" /><br />
同样的连接imageview控件，把他和selectedImage关联起来<br />
保存并关闭interface builder，编译并运行这个应用。<br />
由于模拟器没有摄像头，除非你在真机上测试，否则你将无法体验到拍照的特性。同样如果你的相册中没有图片，你也无法体验选择已存在图片的特性，你可以通过点击home按钮并且拖动一些图片到你的iphone模拟器里来解决这个问题。<br />
<img src="http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-3.png" alt="" /><br />
模拟器将通过safari读取图片，你可以点击并按住图片然后选择保存图片。之后你就能在模拟器的相册中看到你所保存的图片。<br />
<img src="http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-4.png" alt="" /><br />
再次编译并运行你的应用，点击按钮选择一个相册。<br />
<img src="http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-5.png" alt="" /><br />
然后选择一张图片。<br />
http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-6.png<br />
然后你将看到picker消失并且选择的图片显示在你的imageview上了。<br />
<img src="http://mobileorchard.com/wp-content/uploads/2010/11/iPhone-Advance-Programming-The-image-Picker-image-7.png" alt="" /><br />
现在你应该学会如果在你的应用之中选取图片了。
</p></blockquote>
<p>话说，哥当时也是一时兴起，边看边翻译的，翻完了才发现，真tm坑爹啊，完全就是imagepicker的基础教程，怎么好意思标榜高级技巧，我擦。哎，sorry，至少作者的分享精神是可贵的，请原谅我的吐槽。翻都翻了，那就还是发出来吧，虽然翻的不好，转载的话还是麻烦注明一下出处。谢谢~~~</p>
