#
# makefile for capture
# (c) tensorfield ag 2020
# 
#
#

CC=g++
WARNING_FLAGS=-Wall -Wextra -pedantic
#OPENCV_FLAGS=-I/usr/local/include/opencv4 -I/usr/local/include -L/usr/local/lib -lopencv_core -lopencv_highgui -lopencv_imgproc -lopencv_imgcodecs
#OPENCV_FLAGS=$(shell pkg-config opencv4 --libs --cflags)
OPENCV_FLAGS=-I/usr/local/include/opencv4 -L/usr/local/lib -lopencv_core -lopencv_highgui -lopencv_imgproc -lopencv_imgcodecs -lopencv_xphoto 
CFLAGS= -I../common -I../libsv/include -I../libsv/examples  -std=c++11 -pthread -Wl,-rpath=/usr/local/lib -L/usr/local/lib -lexiv2 -lstdc++fs -lboost_system -lboost_filesystem -lboost_program_options -ltiff -ltiffxx
MAGICK_FLAGS=-fopenmp -Wno-write-strings -I/usr/local/include/ImageMagick-7 -L/usr/local/lib -lMagick++-7.Q16HDRI -lMagickWand-7.Q16HDRI -lMagickCore-7.Q16HDRI
CSOURCES= ../libsv/examples/common_cpp/image_processor.cpp
EXIFOBJ = ../common/exif/ExifTool.o ../common/exif/ExifToolPipe.o ../common/exif/TagInfo.o

#PROFILE=-g -pg
PROFILE=


#../bin/capture: capture.cpp 
#	$(CC) -o $@ $< $(CSOURCES) $(CFLAGS)  $(OPENCV_FLAGS) $(PROFILE)
#
#

all: ../bin/capture
	echo done

capture.o: capture.cpp capture.hpp ../common/Imagedata.hpp
	$(CC) -g -c -o $@ $< $(CFLAGS)  $(OPENCV_FLAGS) $(MAGICK_FLAGS) $(PROFILE)

capture-helper.o: capture-helper.c
	$(CC) -g -c -o $@ $< $(CFLAGS)  $(PROFILE)

../bin/capture: capture.o capture-helper.o 
	$(CC) -o $@ $^  $(EXIFOBJ) $(CFLAGS) $(OPENCV_FLAGS) $(MAGICK_FLAGS) $(PROFILE)

watch:
	while true; do \
		make all; \
		../bin/capture --focus --verbose ; \
		inotifywait -e close_write capture.cpp capture.hpp capture-helper.c ../common/Imagedata.hpp; \
	done

