https://javascript.info/data-storage

the main features of `localStorage` :
Shared between all tabs and windows from the same origin.
The data does not expire. It remains after the browser restart and even OS reboot.


 the features of `sessionStorage`  :
- The `sessionStorage` exists only within the current browser tab.
    - Another tab with the same page will have a different storage.
    - But it is shared between iframes in the same tab (assuming they come from the same origin).
- The data survives page refresh, but not closing/opening the tab.
