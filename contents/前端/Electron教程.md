环境：pnpm、pnpx

https://yuzhigang5460.gitbook.io/electron/


[使用electronforge创建](https://www.electronforge.io/)    
模板创建：`pnpx create-electron-app@latest my-app --template=webpack`  
启动：`cd my-app && pnpm start`  
打包：`pnpm make`  



contextBridge 
英文命名准确，易理解。主进程context和渲染进程context是相互隔离的，通过contextBridge将主进程上下文的接口暴露给渲染进程。

Inter-process communication (IPC) 

模式一：`ipcMain` and `ipcRenderer`
模式二：`ipcRenderer.invoke` 和 `ipcMain.handle`
模式三：`webContents` 和  `ipcRenderer.on`  

