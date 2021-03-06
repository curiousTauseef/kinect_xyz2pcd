

## kinect_xyz2pcd

This code allows data captured with the OpenKinect 'record' program or
my modified [kinect_record](https://github.com/mankoff/kinect_record)
program to be used with the Point Cloud Library (PCL) KinFu
algorithm. It uses the outputs of
[kinect_register](https://github.com/mankoff/libfreenect/tree/offline_register)
as the inputs.  

## Dependencies and Other Useful Tools

* https://github.com/mankoff/libfreenect
* https://github.com/mankoff/libfreenect/tree/offline_register
* https://github.com/mankoff/kinect_record/
* http://pointclouds.org/

## NOTE

You should consider collecting data directly with PCL and using KinFu
in realtime or using the PCL PCD-grabber code if possible. The only
reason to use this setup is if you need to use libfreenect for some
reason. Most likely, this reason would be that PCL requires a much
more powerful laptop, while libfreenect can run on a smaller and
lighter computer for field data collection.

## Usage

To use this code, call the program with 4 arguments: The x, y, and z file and then
name of the PCD file you want to create:
```bash
kinect_xyz2pcd foo.x foo.y foo.z foo.pcd
kinect_xyz2pcd foo.{x,y,z,pcd}			# compact Bash notation
```

A more complete workflow might look like this:
```bash
# collect data AND registration paramaters with Kinect
kinect_record some_folder           # save the data into a folder
cd some_folder
kinect_register -s Kx.regdump.dat   # save registration parameters for Kinect #x

# register the data 
for f in *.pgm; do kinect_register -a Kx.regdump.dat $f; done
rm *.ply                   # clean up the PLY files. We don't use them

# now, use this code to convert the x, y, and z files to PCD
for f in *.z; do kinect_xyz2pcd ${f%.*}.{x,y,z,pcd}; done
rm *.{x,y,z}                           # clean up the x,y,z files

# process the PCD files with KinFu
pcl_kinfu_largeScale -pcd . -pcd_fps 0
```

## Installation

```bash
cmake .
make
```

## License

The MIT License (MIT)

Copyright (c) 2013 Ken Mankoff

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
