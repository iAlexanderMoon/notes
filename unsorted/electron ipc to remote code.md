PRINT_LABEL


	load.component.printManifestBarcode 
	load.component.printIncidentReportBarcode 
	load.component.printLossReportBarcode 

	* Get the reponse which should be a PDF... and calls printPDFLabel
	
		basecomponent.pringPDFLabel(response) - Shouldn't this have been a service instead of being on the basecomponent?
		* create a reader, add read from buffer, get the filename 
		
		ipcRenderer = requiring electron... I don't think that is correct. 
		we should just use the window api here!
		
		
Should remove electron from the SPA build






pcMain.on("toMain", (event, args) => {
  fs.readFile("path/to/file", (error, data) => {
    // Do something with file contents

    // Send result back to renderer process
    win.webContents.send("fromMain", responseObj);
  });
});



      if ((<any>window).require) {
      try {
        this.ipc = (<any>window).require("electron").ipcRenderer;


        this.ipc.on("onNewDocumentScanned", (event, arg) => {
          console.log(`${arg} is scanned`);
          
          this.pdfViewerModal.show();
          this.pdfSrc = arg;
          this.ngZone.runTask(() => {
            this.newFileName = 'wait...';// this.pdfSrc.replace(/^.*[\\\/]/, '').replace('.pdf','');
            this.getDocumentName();
          });
        });
        console.warn("Electron ipc loaded.");
      } catch (error) {
        throw error;
      }
    } else {
      console.warn("Could not load electron ipc");
    }