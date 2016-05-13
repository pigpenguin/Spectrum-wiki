Installing Spectrum is ridiculously easy considering how difficult and tedious it was in earlier versions. Follow this guide to ensure that you have a working Spectrum installation.

#### Downloading Spectrum
You can download the latest stable Spectrum version from the [Releases](https://github.com/Ciastex/Spectrum/releases) page. If you prefer to run an unstable Spectrum version, download it from the [AppVeyor page](https://ci.appveyor.com/project/Ciastex/spectrum/build/artifacts).

#### Extracting the package
Spectrum Framework package contains two folders - `Spectrum` and `Managed`. These folders are required to be extracted exactly as they are into your Distance Data folder. See the image below:
![Spectrum Target Directory](http://img.imgland.net/QWL1Lma.png)
Extract both folders to this directory.

#### Running Prism
Now enter the `Managed` directory. Depending on whether you're running Linux/OS X or Windows, run the `spectrum_install_linux.sh` or `spectrum_install_windows.bat`, respectively. Wait until the output looks like the following:
![Spectrum Installer Result](http://img04.imgland.net/nVUilI2.png)  
If your output does not look like the above, then you most probably didn't read this guide carefully enough. If you're sure the problem is on our side, feel free to [file a bug](https://github.com/Ciastex/Spectrum/issues).

That's it. Your Distance installation is now fully prepared for client-side modifications. Enjoy your Spectrum-enhanced Distance experience.