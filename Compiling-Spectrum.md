#### Requirements
To build Spectrum from the currently available source tree you need:  
  1. A Windows-powered machine. There is a possibility of building Spectrum on Linux with MonoDevelop, but it's currently not supported.
  2. Visual Studio 2015 Community, available [here](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx).
  3. An ability to download the current source, either the [git client](https://git-scm.com/download/win/) or the official [github desktop](https://desktop.github.com/) application.

#### Downloading the source code
You have two ways for downloading the source code.

The first one is to use the one of the buttons located on the source page. This provides you with the latest changes from the `master` branch. See the image below for reference:
![Downloading from web](http://img02.imgland.net/_dCbmEy.png).  
`Download ZIP` option allows you to, obviously, download the ZIP containing the latest source. Clicking the button to the left lets you choose between opening the project in Visual Studio or Github Desktop if you have installed them.

The second option is to use the command line client. Simply issue the following command:  
`git clone https://github.com/Ciastex/Spectrum.git`

#### Opening the project
Locate the project source directory and open `Spectrum.sln`. This will cause Visual Studio 2015 to open and shortly you will be able to write your code improvements and, hopefully, after thoroughly reading [the contribution guidelines](https://github.com/Ciastex/Spectrum/wiki/Contribution-guidelines), make a [pull request](https://github.com/Ciastex/Spectrum/pulls).

## Troubleshooting
#### After opening the project I get undefined references and other warnings.
That's because - for some reason - you have no NuGet package source defined. To fix this, you need to complete the following steps:  
  1. Right-click on the `Solution 'Spectrum'` entry in the Solution Explorer and select `Manage NuGet packages for the solution`:  
![Solution explorer](http://img.imgland.net/4V1sUVl.png)
  2. If the list on the right is empty, click the *cog icon* to the right of that list:  
![Package management](http://img03.imgland.net/WBAPfnp.png)
  3. A window will appear. Check the `Microsoft and .NET` package. Click OK:
![Package management, cont.](http://img02.imgland.net/cTb-TN.png)