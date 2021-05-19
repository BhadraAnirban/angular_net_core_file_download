# angular net_core file download
Use Angular and .net core to download file which is saved in byte[]


## In the .net core controller(Web API) -
```
[HttpGet("GetDocument")]
        public ActionResult GetDocument(int userDocumentId)
        {
            try
            {
                var downloadDocument = consultantWorkFlowRepository.GetDocument(userDocumentId);
                if(downloadDocument == null)
                {
                    return UnprocessableEntity("No document found;");
                }
                return File(downloadDocument.DocumentData, "application/vnd.openxmlformats-officedocument.wordprocessingml.document"); /// DocumentData is byte[]
            }
            catch (Exception e)
            {
                return UnprocessableEntity(e.Message);
            }

        }

```

## In Angular first npm install file-saver -
```
https://www.npmjs.com/package/file-saver

```
## Then in Angular service mention the return type as blob-
```
DownloadDocument(userDocumentId: number){
    return this.http.get('consultantworkflow/GetDocument?userDocumentId=' + userDocumentId, { responseType: 'blob' });
  }

```

## In the component use saveAs to call the service download the document-
### If in typescript the file-server path can not be found then install npm i --save-dev @types/file-saver
```
npm i --save-dev @types/file-saver

```


```
import { saveAs } from 'file-saver';

DownloadDocument(userDocumentId: number, documentName: string, forConsultant = false){
    this.consultantService.DownloadDocument(userDocumentId).subscribe(res=> {
       saveAs(res, documentName);
    });
  }

```
