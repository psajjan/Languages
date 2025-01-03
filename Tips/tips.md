### Download Zoom Recording in Chrome

Open the zoom recording link in chrome and just enable the context menu by running the following command in console.  
Open console use the `Ctrl + Shift + j` shortcut. Once the context menu is enabled, you can right click on the video and save it.

```c++
> document.addEventListener('contextmenu', event => event.stopPropagation(), true)
```
