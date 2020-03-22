<h2>JQuery AJAX File Upload</h2>
<p>
This is a simple plugin that uses ajax to upload files. However this uses FormData object so doesn't support IE < 10.
</p>

<p>
Setting it up is really easy.
</p>

<p>
First need to add it in a script tag.
</p>

<pre>
script src="/scripts/upload.js"
</pre>

<p>Then use it very simply</p>
<pre>
var input = $('file-ip');
input.fileupload(
	{
       		url: '/upload',
                success: function(files)
                {
               	    console.log(files);
                },
                error: function(msg)
                {
                    console.log(msg);
                }
        }
);
</pre>

<p>I primarily created it for a laravel project.<br/>So the controller method will be like,</p>
<pre>
class UploadController extends Controller
{
    public function upload()
    {
        $allFiles = Input::file();
        $uploadedFiles = array();
        $destination_path = public_path().'/tmp/';
        foreach($allFiles as $file)
        {
            $fileNameWithExt = $file->getClientOriginalName();
            $fileName = pathinfo($fileNameWithExt, PATHINFO_FILENAME);
            $fileExt = pathinfo($fileNameWithExt, PATHINFO_EXTENSION);
            $newFileName = $fileName . '-' . time() . '.' . $fileExt;
            $file->move($destination_path, $newFileName);
            $uploadedFiles[] = $newFileName;
        }
        return json_encode(["status"=>"ok", "files"=>$uploadedFiles]);
    }
}
</pre>
