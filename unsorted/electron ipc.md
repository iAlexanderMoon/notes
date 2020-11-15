 // EXPOSE: window.api.printLabel to the website
  contextBridge.exposeInMainWorld(
      "api",{
        printLabel: (filename, buffer) => {
         ipcRenderer.send('PRINT_LABEL', filename, buffer);
        },
      }
  );
  
  
  
  
  OBS.Spa/package.json  
	@types/electron should be removable
	
	
	
Why is all the logic for using the upload document service in app.component.ts?
	startListenOnNewDocumentEvent() {

	}

		this.pdfViewerModal.show().
		getDocumentName 
		reads the file contents and then tries to send
		
		https://github.com/electron/electron/issues/9920
		
		
		// Get 80% security with less refactoring yes please
		
		https://github.com/electron/electron/issues/9920#issuecomment-468323625
		
		window.ipcRenderer = require('electron').ipcRenderer;
		
	The right way of importing your fs/ipcRenderer into your renderer process is with IPC (inter-process-communication). This is Electron's way of allowing you to talk between main and renderer process. Broken down, this is how your app needs to look: