# How to install spectrum

First [download spectrum](https://github.com/Ciastex/Spectrum/releases), at the time of writing the most
recent version is 0.0.244.

The download should give you a  zip, in it there are two directories, `Spectrum` and `Managed`. Move both 
of those folders into the `Distance_Data` directory in the Distance game files. If you don't know where 
they are, you can right click Distance in your steam library, tab over to Local Files, and then click 
Browse Local Files.

Once you have done that, you need to run the `install_windows.bat` or `install_linux.sh` shell script,
which is in the new `Managed` directory after you thrown the spectrum files in there. This will run the 
patcher which will hook spectrum into Distance. The output in the resulting terminal 
window should look something like this:

![](https://i.imgur.com/zVZQxux.png)

Providing nothing looks horribly broken, you're done! Congratulations.

