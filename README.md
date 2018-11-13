# Unpack-re-pack-android-DAT-files
Ubuntu 64-bit - Guide

	Step 1 - Decompressing = DAT (sparse data) -> EXT4 (raw image)

We're now using sdat2img binary, the usage is very simple (make sure you have python 2.7+ installed):

./sdat2img.py <transfer_list> <system_new_file> [system_img]
- <transfer_list> = input, system.transfer.list from rom zip
- <system_new_file> = input, system.new.dat from rom zip
- [system_img] = output ext4 raw image file

and a quick example:
./sdat2img.py system.transfer.list system.new.dat system.img
by running this command you will get as output the file my_new_system.img which is the raw ext4 image.


	Step 2 - Decompress EXT4 (raw image) -> OUTPUT folder -> Compress EXT4 (raw image)

Now we need to mount or ext4 raw image into an output folder so we can see apks/jars etc.
To do this we need to type this command:

sudo mount -t ext4 -o loop system.img output/

As you can see there is a new folder called output which we can edit/modify/delete your files (not able to? see here)

Now we need to compress it back to a raw ext4 image, to do this we need the make_ext4fs binary. Make sure you have the file_contexts file (taken from the Rom zip) inside the make_ext4fs path. Then type this (got issues? see here)

./make_ext4fs -T 0 -S file_contexts -l 1073741824 -a system system_new.img output/

You will get the new raw ext4 image called 'system_new.img' ready for the next step.

	Step 3 - Converting = EXT4 (raw image) -> IMG (sparse image)

Now we need to convert the ext4 raw image into a sparse image. For this you need img2simg binary you can find here (thx to @A.S._id). 
The usage is simple:

img2simg <raw_image_file> <sparse_image_file>

Pretty self-explanatory, the output will be a new sparse image (.img).


	Step 4 - Converting = IMG (sparse image) -> DAT (sparse data)

Now we need the img2sdat binary, the usage is very simple (make sure you have python 2.7+ installed):

./img2sdat.py <system_img>

- <system_img> = name of input sparse image file (from step 3)

As you can see the output is composed by system.transfer.list, (system.patch.dat) & system.new.dat, ready to be replaced inside your Rom zip.
