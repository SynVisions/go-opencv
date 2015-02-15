Go OpenCV binding
==================

A Golang binding for [OpenCV](http://opencv.org/).

OpenCV 1.x C API bindings through CGO, and OpenCV 2+ C++ API through SWIG.

[**DISCLAIMER**](https://github.com/lazywei/go-opencv#disclaimer)

## Install

### Linux & Mac OS X

Install Go and OpenCV, you might want to install both of them via `apt-get` or `homebrew`.

```
go get github.com/lazywei/go-opencv
cd ${GoOpenCVRoot}/samples && go run hellocv.go
```

### Windows

- Install Go and MinGw
- install OpenCV-2.4.x to MinGW dir

```
# libopencv*.dll --> ${MinGWRoot}\bin
# libopencv*.lib --> ${MinGWRoot}\lib
# include\opencv --> ${MinGWRoot}\include\opencv
# include\opencv2 --> ${MinGWRoot}\include\opencv2

go get code.google.com/p/go-opencv/trunk/opencv
cd ${GoOpenCVRoot}/trunk/samples && go run hellocv.go
```

## [WIP] OpenCV2

After OpenCV 2.x+, the core team no longer develop and maintain C API. Therefore, CGO will not be used in CV2 binding. Instead, we are using SWIG for wrapping. The support for OpenCV2 is currently under development.

If you want to use CV2's API, please refer to the code under `opencv2/` directory. There has no too much document for CV2 wrapper yet, but you can still find the usage example in `*_test.go`.

Note that the basic data structures in OpenCV (e.g., `cv::Mat`, `cv::Point3f`) hasn't been wrapped fully yet. For now, we have some specific wrappers. We will try to wrapped those data structures fully as soon as possible.

## Example

### OpenCV2's initCameraMatrix2D

```go
package main

import "github.com/lazywei/go-opencv/opencv2/gcv_utils"
import "github.com/lazywei/go-opencv/opencv2/gcv_calib3d"

func main() {
	objPts := gcv_utils.NewGcvPoint3fVector(int64(4))
	objPts.Set(0, gcv_utils.GetPoint3f(0, 25, 0))
	objPts.Set(1, gcv_utils.GetPoint3f(0, -25, 0))
	objPts.Set(2, gcv_utils.GetPoint3f(-47, 25, 0))
	objPts.Set(3, gcv_utils.GetPoint3f(-47, -25, 0))

	imgPts := gcv_utils.NewGcvPoint2fVector(int64(4))
	imgPts.Set(0, gcv_utils.GetPoint2f(1136.4140625, 1041.89208984))
	imgPts.Set(1, gcv_utils.GetPoint2f(1845.33190918, 671.39581299))
	imgPts.Set(2, gcv_utils.GetPoint2f(302.73373413, 634.79998779))
	imgPts.Set(3, gcv_utils.GetPoint2f(1051.46154785, 352.76107788))

	cameraMatrix := GcvInitCameraMatrix2D(objPts, imgPts)
}
```


### Resizing

```go
package main

import opencv "github.com/lazywei/go-opencv/opencv"

func main() {
	filename := "bert.jpg"
	srcImg := opencv.LoadImage(filename)
	if srcImg == nil {
		panic("Loading Image failed")
	}
	defer srcImg.Release()
	resized1 := opencv.Resize(srcImg, 400, 0, 0)
	resized2 := opencv.Resize(srcImg, 300, 500, 0)
	resized3 := opencv.Resize(srcImg, 300, 500, 2)
	opencv.SaveImage("resized1.jpg", resized1, 0)
	opencv.SaveImage("resized2.jpg", resized2, 0)
	opencv.SaveImage("resized3.jpg", resized3, 0)
}
```

### Webcam

Yet another cool example is created by @saratovsource which demos how to use webcam:

```
cd samples
go run webcam.go
```

### More

You can find more samples at: https://github.com/lazywei/go-opencv/tree/master/samples

## How to contribute

- Fork this repo
- Clone the main repo, and add your fork as a remote

  ```
  git clone https://github.com/lazywei/go-opencv.git
  cd go-opencv
  git remote rename origin upstream
  git remote add origin https://github.com/your_github_account/go-opencv.git
  ```

- Create new feature branch

  ```
  git checkout -b your-feature-branch
  ```

- Commit your change and push it to your repo 

  ```
  git commit -m 'new feature'
  git push origin your-feature-branch
  ```

- Open a pull request!

-------------------

## Disclaimer

This is a fork of [chai's go-opencv](https://github.com/chai2010/opencv), which has only OpenCV1 support through CGO. At the time of the fork (Dec 9, 2013) the original project was inactive, and hence I decide to host a fork on Github so people can contribute to this project easily. However, now it seems to be active again starting from Aug 25, 2014. Efforts to merge the two projects are very welcome.

