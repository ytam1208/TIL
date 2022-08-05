
### CV_8UC3 타입일때, Mat 픽셀 접근 및 변경

---
```
    for(int i = 0; i < Output_Map.cols; i++)
        for(int j = 0; j < Output_Map.rows; j++){
            uchar blue = Output_Map.at<cv::Vec3b>(j, i)[0];
            uchar green = Output_Map.at<cv::Vec3b>(j, i)[1];
            uchar red = Output_Map.at<cv::Vec3b>(j, i)[2];
            if(blue == 0 && green == 0 && red == 0){
                Output_Map.at<cv::Vec3b>(j, i)[0] = 127;
                Output_Map.at<cv::Vec3b>(j, i)[1] = 127;
                Output_Map.at<cv::Vec3b>(j, i)[2] = 127;
            }
        }
```