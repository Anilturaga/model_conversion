> Mail copy

Hi Naresh,


The deployment of PyTorch models in C++
*Assuming the reader has some experience working with Yocto OS.

Make sure you went through this tutorial video about converting a python model to be executable in C++.

The following tools are used to cross compile the cpp file that will load the model file(.pt) and execute it.
CMAKE
devtools(yocto)
BitBake
Step 1: Put your cpp file and CMakeLists.txt file in a single folder named by your application(or model name). 

Step 2: Go to your yocto build folder and run devtool add --no-same-dir <app_repository_path>
It will do the build in seperate directory.
Step 3: Type devtool edit-recipe <app_name> to open your <app_name>.bb(BitBake recipe file). Make sure you see inherit cmake is present

Step 4: Now run bitbake <app_name>.It will cross compile our cpp app(including the torch bindings) to your target. The app will be in /package/usr/bin

Step 5: Now the recipe data is local and conf data is global. We will edit the conf/local.conf to add our app so that I'll ship with the image, add this CORE_IMAGE_EXTRA_INSTALL = "<app_name">.

Also, add the model file(.pt) to be included with the image by adding the path in the .bb recipe FILES_${PN} += "<model_file_path"

Step 6:
Build the image bitbake <image_name>  (ex: bitbake core-image-minimal)

Step 7: To test your completed image, run 
runqemu <architecture>(ex:qemuarm) nographic slirp

Check if the app is properly installed
which <app_name>

Run the app 
<app_name> <model_path>

Then shut the qemu 
poweroff


And that's it! 🎉

Regards,
Anil Turaga.