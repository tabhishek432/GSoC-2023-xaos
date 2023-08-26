# Google Summer of Code 2023
## GSoC 2023 Final Report

* Author :  [Abhishek Tiwari](https://github.com/tabhishek432) (yours truly :D)
* The Ultimate Support System (aka Mentors) : [Dr. Zoltán Kovács](https://github.com/kovzol) & [Mr. Anurag Aggarwal](https://github.com/kanurag94) (Can't thank them enough! :')
* The Proposed Problem : [GSoC 2023 Idea for XaoS](https://www.gnu.org/software/soc-projects/ideas-2023.html#xaos)
* My Proposal Summary ->
    * The project aims to enable XaoS, a fractal program, to function effectively on web browsers through the use of Qt. Currently, XaoS requires the user to download and install the software, which limits its accessibility. However, the current implementation of XaoS using Qt web assembly is not functioning well on web platforms due to issues with the synchronous, infinite loop used in XaoS's event loop. Browsers typically run JavaScript, which is single-threaded and relies on asynchronous events to avoid blocking the main thread, making the current implementation unsuitable. The proposed solution is to use a QTimer to periodically update the fractal using UIH functions. By refactoring the code to use a QTimer, XaoS can be made compatible with browser architecture and enable a single Qt source code for implementation across different platforms.
* My Contribution Journey ->
    * [Project Development Diary](https://docs.google.com/document/d/1gsRQepBochpEL1ASOPFwII6olB8vulU73TDNcHqstUE/edit?usp=sharing) 
    * [Progress Progress Tracker](https://docs.google.com/spreadsheets/d/1925x3CV-awJKOs7Q3sSbBp-BN1PwsH4O2mp9Xqw5H4c/edit?usp=sharing)
* Also, don't forget to read the [Acknowledgements](https://github.com/tabhishek432/GSoC-2023-xaos/blob/main/README.md#acknowledgement)!

### Accomplished tasks
#### Adding the web user interface

#### Resolving the load random example issue

#### Implementing XaoS in Qt6

### Future tasks


### Acknowledgement
