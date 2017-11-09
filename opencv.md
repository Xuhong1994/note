1. 在图像指定的像素位置画矩形和圆
	cv::rectangle(img,cvPoint(50,50),cvPoint(100,100),cvScalar(255,0,0),2);
2. 绘制Bezier曲线
~~~
                //绘制控制点
                cv::circle(img,cvPoint(120,70),5,cvScalar(0,0,255),2);
                cv::circle(img,cvPoint(50,50),5,cvScalar(0,0,255),2);
                cv::circle(img,cvPoint(100,100),5,cvScalar(0,0,255),2);
                //绘制Bezier曲线
                int  divs =50;
                CvPoint pt_start =cvPoint(50,50), pt_control=cvPoint(120,70),pt_end=cvPoint(100,100),pt_pre=cvPoint(50,50),pt_now;
                for( int i=1; i<=divs;i++)
                {
                    float t =(float) i/divs;
                    cout<<t<<endl;
                    pt_now.x = pt_start.x *pow(1-t,2) +2*t*(1-t)*pt_control.x +pow(t,2)*pt_end.x;
                    pt_now.y = pt_start.y *pow(1-t,2) +2*t*(1-t)*pt_control.y +pow(t,2)*pt_end.y;

                   cv::line(img,pt_pre,pt_now,cvScalar(255,0,0),2);

                   pt_pre = pt_now;
                }
~~~

3. 滤波
1. 线性滤波
	1. 均值滤波
	blur(img,img,Size(3,3)); //均值滤波	
	2. 高斯滤波
	  GaussianBlur(img,img,Size(9,9),0,0); //高斯滤波
2. 非线性滤波(中值滤波)
	medianBlur(img,img,7);
3. 自定义滤波卷积核
~~~
                float k[3][3] ={1,-2,-3,4,5,6,7,8,9};
               Mat mask = Mat(3,3,CV_32FC1,k);
      
                mask = mask/9;
               filter2D (img,img,img.depth (),mask);
~~~
4. 锐化
基本思想：将原图的每一个通道进行DFT变换得到F(u,v),在将其经过高斯高通滤波器H(u,v),得到F(u,v)*H(u,v)，在将其IDFT得到高通滤波后的空间域图，最后加到+0.8*原图即可。==该步骤实现了图像的预处理环节：滤波+锐化==
5. ==canny边缘检测==
在opencv2.4.13中Canny算子只能用来处理灰度图像
~~~
//高阶的canny用法
    Mat dst,edge,gray;
    cvtColor( image, gray, CV_RGB2GRAY );
     blur( gray, edge, Size(3,3) );
     Canny( edge, edge, 3, 9,3 );
     image.copyTo( dst, edge);
     imshow("", dst);
~~~
6. opencv2 == 使用鼠标绘制矩形并截取和保存矩形区域图像==
~~~
cv::Mat org,dst,tmp;
void on_mouse(int event,int x,int y,int flags,void *ustc)//event鼠标事件代号，x,y鼠标坐标，flags拖拽和键盘操作的代号
{
           static Point pre_pt(-1,-1);//初始坐标
           static Point cur_pt (-1,-1);//实时坐标
           static  int i=0;
        char temp[16];
        char file[100];
        if (event == CV_EVENT_LBUTTONDOWN)//左键按下，读取初始坐标，并在图像上该点处划圆
        {
            org.copyTo(tmp);//将原始图片复制到img中
            sprintf(temp,"(%d,%d)",x,y);
            pre_pt = Point(x,y);
            putText(tmp,temp,pre_pt,FONT_HERSHEY_SIMPLEX,0.5,Scalar(0,0,0,255),1,8);//在窗口上显示坐标
            circle(tmp,pre_pt,2,Scalar(255,0,0,0),CV_FILLED,CV_AA,0);//划圆
            imshow("img",tmp);

        }
        else if (event == CV_EVENT_MOUSEMOVE && !(flags & CV_EVENT_FLAG_LBUTTON))//左键没有按下的情况下鼠标移动的处理函数
        {
            org.copyTo(tmp);//将img复制到临时图像tmp上，用于显示实时坐标
            sprintf(temp,"(%d,%d)",x,y);
            cur_pt = Point(x,y);
            putText(tmp,temp,cur_pt,FONT_HERSHEY_SIMPLEX,0.5,Scalar(0,0,0,255));//只是实时显示鼠标移动的坐标
            imshow("img",tmp);
                }
        else if (event == CV_EVENT_MOUSEMOVE && (flags & CV_EVENT_FLAG_LBUTTON))//左键按下时，鼠标移动，则在图像上划矩形
        {
            org.copyTo(tmp);
            sprintf(temp,"(%d,%d)",x,y);
            cur_pt = Point(x,y);
            putText(tmp,temp,cur_pt,FONT_HERSHEY_SIMPLEX,0.5,Scalar(0,0,0,255));
            rectangle(tmp,pre_pt,cur_pt,Scalar(0,255,0,0),1,8,0);//在临时图像上实时显示鼠标拖动时形成的矩形
            imshow("img",tmp);
        }
        else if (event == CV_EVENT_LBUTTONUP)//左键松开，将在图像上划矩形
        {
            //截取矩形包围的图像，并保存到dst中
            int width = abs(pre_pt.x - cur_pt.x);
            int height = abs(pre_pt.y - cur_pt.y);
            if (width == 0 || height == 0)
            {
                printf("width == 0 || height == 0");
                return;
            }
            dst = org(Rect(min(cur_pt.x,pre_pt.x),min(cur_pt.y,pre_pt.y),width,height));
            imshow("dst",dst);
            sprintf(file,"%d%d%d.png",0,0,i);
            i++;
             imwrite(file,dst);
        }
}
~~~
7. opencv2== 模板匹配==
~~~
  cv::Mat templateImage= imread("./000.png");
  cv::Mat templateImage1= imread("./002.png");

 //模板匹配
   int result_cols =  image.cols - templateImage.cols + 1;
   int result_rows = image.rows - templateImage.rows + 1;
   int result1_cols1=  image.cols - templateImage1.cols + 1;
   int result1_rows1= image.rows - templateImage1.rows + 1;


   cv::Mat result = cv::Mat( result_rows, result_cols, CV_32FC1 );
   cv::Mat result1 = cv::Mat( result1_rows1, result1_cols1, CV_32FC1 );

   cv::matchTemplate( image, templateImage, result, CV_TM_SQDIFF_NORMED );
   cv::matchTemplate( image, templateImage1, result1, CV_TM_SQDIFF_NORMED );


   double minVal, maxVal;
   cv::Point minLoc, maxLoc, matchLoc;
   cv::minMaxLoc( result, &minVal, &maxVal, &minLoc, &maxLoc, Mat() );
   matchLoc = minLoc;
   double minVal1, maxVal1;
   cv::Point minLoc1, maxLoc1, matchLoc1;
   cv::minMaxLoc( result1, &minVal1, &maxVal1, &minLoc1, &maxLoc1, Mat() );
   matchLoc1 = minLoc1;


    cv::rectangle( image, cv::Rect(matchLoc, cv::Size(templateImage.cols, templateImage.rows) ), Scalar(0, 0, 255), 2, 8, 0 );
    cv::rectangle( image, cv::Rect(matchLoc1, cv::Size(templateImage1.cols, templateImage1.rows) ), Scalar(0, 255, 0), 2, 8, 0 );

   imshow("", image);
   waitKey(0);

~~~


