# Handle File Upload #

WebWind supports file upload by integration of
[Commons FileUpload](http://commons.apache.org/fileupload/). You don't need any
configuration but just place the `commons-fileupload-1.2.x.jar` into
`/WEB-INF/lib/`. File upload feature is disabled if commons-fileupload cannot
be found under `/WEB-INF/lib/`.

It is very simple to handle file upload:

```
public class FileUpload {
    @Mapping("/fileupload")
    public String post() throws Exception {
        HttpServletRequest request = ActionContext.getActionContext().getHttpServletRequest();
        if (request instanceof MultipartHttpServletRequest) {
            MultipartHttpServletRequest multipartRequest = (MultipartHttpServletRequest) request;
            // we are assume that file is uploaded with field name 'myfile':
            String filename = multipartRequest.getFileName("myfile");
            // now get file name like 'C:\WINDOWS\Notepad.ini'...
            InputStream input = multipartRequest.getFileInputStream("myfile");
            // TODO: read file content here...
            input.close();
            return "redirect:/upload-success.htm";
        }
        throw new IOException("Not a multipart request!");
    }
}
```

Normal form fields can still be retrieved by `HttpServletRequest.getParameter(String)`.

**NOTE** that WebWind only supports [Commons FileUpload](http://commons.apache.org/fileupload/) **1.2.0** or above.