# Google Summer of Code 2023
## GSoC 2023 Final Report

* Author :  [Abhishek Tiwari](https://github.com/tabhishek432) (yours truly :D)
* The Ultimate Support System (aka Mentors) : [Dr. Zoltán Kovács](https://github.com/kovzol) & [Mr. Anurag Aggarwal](https://github.com/kanurag94) (Can't thank them enough! :')
* The Project Name : **Adding web user interface to XaoS**
* The Proposed Problem : [GSoC 2023 Idea for XaoS](https://www.gnu.org/software/soc-projects/ideas-2023.html#xaos)
* My Proposal Summary ->
    * The project aims to enable XaoS, a fractal program, to function effectively on web browsers through the use of Qt. Currently, XaoS requires the user to download and install the software, which limits its accessibility. However, the current implementation of XaoS using Qt web assembly is not functioning well on web platforms due to issues with the synchronous, infinite loop used in XaoS's event loop. Browsers typically run JavaScript, which is single-threaded and relies on asynchronous events to avoid blocking the main thread, making the current implementation unsuitable. The proposed solution is to use a QTimer to periodically update the fractal using UIH functions. By refactoring the code to use a QTimer, XaoS can be made compatible with browser architecture and enable a single Qt source code for implementation across different platforms.
* My Contribution Journey ->
    * [Project Development Diary](https://docs.google.com/document/d/1gsRQepBochpEL1ASOPFwII6olB8vulU73TDNcHqstUE/edit?usp=sharing) 
    * [Progress Progress Tracker](https://docs.google.com/spreadsheets/d/1925x3CV-awJKOs7Q3sSbBp-BN1PwsH4O2mp9Xqw5H4c/edit?usp=sharing)
* Also, don't forget to read the [Acknowledgements](https://github.com/tabhishek432/GSoC-2023-xaos/blob/main/README.md#acknowledgement)!

### Accomplished tasks
First of all, to commence, it became evident that achieving the compilation of XaoS in web assembly necessitated certain adjustments. These alterations encompassed the inclusion of `c++11` within the configuration and the incorporation of `QMAKE_CXXFLAGS += -fpermissive` during the process.
#### Adding the web user interface
The challenge was as mentioned above was that the current implementation of XaoS using Qt web assembly is not functioning on web platforms due to issues with the synchronous, infinite loop used in XaoS's event loop. Initially it was a challenging task to make web assembly version of XaoS work as we thought to use QTimer in all the functions. But thinking of approach to do it one by one, and finally QTimer, or we can say can say connecting its timeout() signal to the only function responsible for primary infinite loop in main.cpp, that is eventLoop() function inside mainwindow.cpp, it was finally working in web browser.

The Initial commit resolving the issue can be found here: [#151](https://github.com/xaos-project/XaoS/commit/8a08d80386325dd0ec700f0fcd1d6d717a03b76d) <br />
The first version of working XaoS web UI can be found here: [https://matek.hu/zoltan/xaos/](https://matek.hu/zoltan/xaos/)

***Minor issue to get tutorials, catalogs and examples working***<br />
In web version, there was an issue of tutorials, catalogs and examples not being found as it wasn't having the right datapath to load the files. Following were the adjustments needed in the Xaos.pro file to get these files loaded and working:
```
wasm{
    QMAKE_LFLAGS += --preload-file $$PWD/examples@$$DATAPATH/examples
    QMAKE_LFLAGS += --preload-file $$PWD/catalogs@$$DATAPATH/catalogs
    QMAKE_LFLAGS += --preload-file $$PWD/tutorial@$$DATAPATH/tutorial
}
```
Here are the commits made to incorporate these changes: [commit 1](https://github.com/xaos-project/XaoS/commit/2aaa9fec927da3878f18d242760fa4f86e5626e8) and [commit 2](https://github.com/xaos-project/XaoS/commit/84b5dba18df410ea576f6e445bce1f96d8bd5058)

#### Resolving the Load Random Example issue
In the meantime to get Xaos web version work fully. There was an issue in both the native version as well as the web version to get load random example working. The problem arises from the fact that the examples are kept in different subfolders inside the examples folder to better help them classify. Here, it was needed to define a function that would go inside the diretories and then select a random example. But this was inducing problems as it was only taking examples from first subfolder. <br />
After some thoughts, there was a better solution found. We can define a function that would select a random subfolder from the list of names of subfolders and inside that path we can run our function as usual to load the random example. This is how it was resolved!

The commit to resolve this issue can found here: [#225](https://github.com/xaos-project/XaoS/commit/67a9c261479e5dc70db9704d468c99d65725aeef)

#### Attempts to make async functions work
Now back to making Xaos work fully functional in the web browser, we faced many challenges. Many menu items were not working for example, if you wish to change the Iterations in Calculations menu item, it was not updating in the dialog box after you change and hit Ok. This was happening for many of the menu items. For debugging, we looked at the google chrome developers' console, and it was showing error, that is, "Uncaught Please compile your program with async support in order to use asynchronous operations like emscripten_sleep". <br />
We realised that maybe it's time to shift to Qt6 version to resolve the issue. But it was also giving an Application exit error stating "Index out of bounds".

We then posted this query on Qt forums hoping to resolve this issue : [Query posted on Qt forums](https://forum.qt.io/topic/146513/error-while-running-xaos-as-a-web-application-in-qt6) <br />
But it didn't help us much. Then in order to tackle this issue, we thought of using "ASYNCIFY" flag inside Xaos.pro hoping that it'll work, but it didn't as well. <br />

We then wrote a mail to Morten Sorvig (from Qt) explaining our issue and ASYNCIFY not working for Qt5. He told us that Qt5 will see no further development in web assembly and all the development will now happen in Qt6. Therefore, now we were only left with one option that is to make Xaos work with Qt6 verison.

#### Implementing XaoS in Qt6
By running XaoS in Qt6.5.1 with emscripten version 3.1.25, we were getting index out of bound error for a long time. We tried many versions of emscripten like 3.1.39 and 3.1.42 but it didn't resolve the issue. It finally worked when Dr. Zoltan told me that it was working fine for the newer versions of Qt. Then, after installing Qt version 6.5.2 and running it with emscripten version it with 3.1.25, it finally worked!

Here's the web version of Xaos working in Qt6.5.2: [Xaos in Qt6](https://matek.hu/zoltan/xaos.tmp3/xaos.html)
Still we needed to solve the issue of getting the menu items work properly in Qt6. We tried to use "sASYNCIFY" flag to make it work but it was just slowing the application down very much and the application kept hanging. We tried many other similar things to get it working but it didn't work.

In the meantime, Dr. Zoltan advised to me to work on the `showDialog` function and try conneting it to timeout signals. I tried the usual way but it wasn't helping. Then, after he took time to write code for individual cases of the type of input parameters. And it finally started working! My task was to now continue this and reciprocate for all the cases of different types of input parameters.

Here's the code I committed for making it work in Qt6 and all the above features: [Final Result](https://github.com/tabhishek432/XaoS/tree/qt6)

### Future tasks
Future tasks involve some of the pending implementation like making `DIALOF_IFILE`, `DIALOG_OFILE` etc. cases work. Full testing after rectifying all the errors is also to be done to make sure XaoS works fine across platforms.<br />
If web version of XaoS becomes success, we can think of expanding the code to android version too!

### Acknowledgement
I wish to extend my heartfelt gratitude to [Dr. Zoltán Kovács](https://github.com/kovzol), whose unwavering support and guidance have been invaluable. He not only provided motivation during challenging phases but also fostered an environment where questions were encouraged. His dedication was evident as he delved into code, authored new lines, and generously invested his time to aid my progress. Furthermore, my appreciation extends to [Mr. Anurag Aggarwal](https://github.com/kanurag94), whose consistent help proved instrumental in comprehending complex processes. His patient explanations on project debugging have equipped me with valuable skills. The privilege of contributing to the [GNU project organization](https://www.gnu.org/home.en.html) during the summer has been a source of immense gratitude. This experience has been an enriching journey, and the lessons learned will forever hold a special place in my growth as a professional.

