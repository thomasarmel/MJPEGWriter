# MJPEGWriter
OpenCV Video HTTP Streaming via MJPEG.
Based on the code found in 
[StackExchange -  CodeReview](http://codereview.stackexchange.com/questions/124321/multithreaded-mjpg-network-stream-server/156915#156915) and [Answers - OpenCV](http://answers.opencv.org/question/6976/display-iplimage-in-webbrowsers/)

## Example main

```C++
int main()
{
    MJPEG server(7777);
    VideoCapture cap;
    bool ok = cap.open(0);
    if (!ok)
    {
        cerr << "No cam found" << endl;
        return 1;
    }
    Mat frame;
    cap >> frame;
    server.write(frame);
    frame.release();
    server.start();
    while (cap.isOpened())
    {
        cap >> frame;
        server.write(frame);
        frame.release();
        mySleep(40);
    }
    cout << "Camera shutdown" << endl;
    server.stop();
    return 0;
}
```
Note: you have to write an image to the MJPEGWriter class before start the server.

## Compiling

On debian based systems you can install the required libraries with:

```sh
sudo apt-get install libopencv-core-dev libopencv-calib3d-dev libopencv-dnn-dev libopencv-objdetect-dev libopencv-photo-dev libopencv-stitching-dev libopencv-video-dev
```

Compile with C++11, OpenCV libraries and pthread:


```sh
g++ MJPEGWriter.cpp main.cpp -o MJPEG -lpthread -lopencv_core -lopencv_videoio -lopencv_imgcodecs -std=c++11
```

In case the following error appears:
`fatal error: opencv2/opencv.hpp: No such file or directory`, then add `-I/usr/include/opencv4/` to the compile command.

## Roadmap
You can follow the development and request new features at https://trello.com/b/OZVtAu05
