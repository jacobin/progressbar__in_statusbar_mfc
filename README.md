# Showing progress bar in a status bar pane

An easy way to add a progress control to a status bar


- [Download source - 4.6 Kb](https://raw.githubusercontent.com/ChrisMaunder/progressbar/master/docs/assets/ProgressBar_src.zip)

![Sample Image](https://raw.githubusercontent.com/ChrisMaunder/progressbar/master/docs/assets/ProgressBar.gif)

Adding a `CProgressCtrl `to the status bar has already been addressed by Brad Mann. His method involved modifying the status bar and messing around with the resource editor. I developed a separate CProgressBar class in order to allow the programmer to just drop in a progress bar whereever they wanted using a single "`CProgressBar Bar(...)`" declaration, which would initialise and display itself and clean up after itself after it was done. The progress bar can also be created once (say as a member variable) and reused multiple times. This new version of the progress bar also resizes itself if the status bar size changes. 

The progress bar lets you specify a message (displayed to the left of the bar) and a Progress Control bar in the any pane of your applications status bar (if it has one - thanks to Patty You for a bug fix on that one). The message for the progress bar can be changed at any time, as can the size and range of the bar. 

## Construction

```cpp
CProgressBar(); 
CProgressBar(LPCTSTR strMessage, int nSize=100, int MaxValue=100, BOOL bSmooth=FALSE, int nPane=0);
BOOL Create(LPCTSTR strMessage, int nSize=100, int MaxValue=100, BOOL bSmooth=FALSE, int nPane=0);
```

Construction is either via the constructor or a two-step process using the constructor and the "`Create`" function. "`strMessage`" is the message to be displayed, "`nSize`" is the percentage width of the status bar that will be occupied by the bar (including the text), and "`MaxValue`" is the maximum range of the bar.

"bSmooth" will only be effective if you have the header files and *commctrl32.dll* from IE 3.0 or above (no problems for MS VC 5.0). It specifies whether the progress bar will be smooth or chunky. 

## Operations

```cpp
    BOOL Success()                      // Construction successful?

    COLORREF SetBarColour(COLORREF clrBar);  // Set Bar colour, returns previous
    COLORREF SetBkColour(COLORREF clrBar);   // Set background colour, returns previous

    int  SetPos(int nPos);              // Same as CProgressCtrl
    int  OffsetPos(int nPos);           // Same as CProgressCtrl
    int  SetStep(int nStep);            // Same as CProgressCtrl
    int  StepIt();                      // Same as CProgressCtrl
    void Clear();                       // Clear the status bar
    void SetRange(int nLower, int nUpper, int nStep = 1);
                                        // Set min, max and step size
    void SetText(LPCTSTR strMessage);   // Set the message
    void SetSize(int nSize);            // Set the bar size
```

To use the bar, just do something like: 

```cpp
    CProgressBar Bar("Testing", 40, 1000);
    
    for (int i = 0;  i <  1000; i++) 
    {
        // perform operation
        Bar.StepIt();
    }
```

or it can be done two stage as: 

```cpp
    CProgressBar bar;
    
    bar.Create("Processing", 40, 1000);
    for (int i = 0; i < 1000; i++) 
    { 
        //    perform operation 
        bar.StepIt();
    } 
    bar.SetText("Writing"); 
    for  (int i = 0;  i <  1000; i++) 
    {
        // perform operation
        bar.StepIt();
        PeekAndPump();    // Message pump
    }
    bar.Clear();
```

In the above case, `PeekAndPump`() is a function which simply peeks and pumps messages, allowing user interaction with the window during a lengthy process. If the window size changes during the processing, the progress bar size will alsow change. 

This article was updated by [Michael Martin](mailto:martm@pegasystems.com) (allowing the progress bar can now be placed in any pane of the status bar). A further update by [YoSilver](http://www.codeproject.com/script/Membership/View.aspx?mid=32659) has fixed an issue that occurs in multithreaded apps. Yet Another Update by Alex Paterson allows you to specify which statusbar you wish the bar to appear in.
