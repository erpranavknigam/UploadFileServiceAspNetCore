### ----------------------------------------------------Upload File Service---------------------------------------------------
### -------------------------------------------------Author: Pranavkant Nigam----------------------------------------------

The UploadFileService is a .NET class designed to facilitate file uploads within .NET applications.
It's specifically crafted to handle file uploads securely and efficiently.
With methods for synchronous and asynchronous file uploads, it offers flexibility to suit various application requirements.
The service validates crucial parameters such as file size, extension, and directory existence to ensure data integrity and security.
Whether uploading files as byte arrays or using FileStream, it provides a streamlined solution for integrating file upload functionality into .NET applications.
Furthermore, robust error handling capabilities enable graceful management of upload failures, enhancing overall application reliability.

## 1) UploadFile:
	path: The base path where the file will be uploaded.
	directory: The directory within the base path where the file will be uploaded.
	fileName: The name of the file.
	fileSize: The size of the file.
	fileExtension: The extension of the file.
	allowedExtensions: A list of allowed file extensions.
	allowedSize: The maximum allowed size for the file.
	fileContent: The content of the file as a byte array.

## 2) UploadFileAsync:
	path: The base path where the file will be uploaded.
	directory: The directory within the base path where the file will be uploaded.
	fileName: The name of the file.
	fileSize: The size of the file.
	fileExtension: The extension of the file.
	allowedExtensions: A list of allowed file extensions.
	allowedSize: The maximum allowed size for the file.
	fileContent: The content of the file as a byte array.

## 3) UploadFile (alternate version with IFormFile parameter):
	path: The base path where the file will be uploaded.
	directory: The directory within the base path where the file will be uploaded.
	fileName: The File stream representing the file.
	allowedExtensions: A list of allowed file extensions.
	allowedSize: The maximum allowed size for the file.

## 4) UploadFileAsync (alternate version with IFormFile parameter):
	path: The base path where the file will be uploaded.
	directory: The directory within the base path where the file will be uploaded.
	fileName: The File stream representing the file.
	allowedExtensions: A list of allowed file extensions.
	allowedSize: The maximum allowed size for the file.



### --------------------------------------------------Error and Success Codes--------------------------------------------------
Success (0): The file was uploaded successfully.
SizeLessThanZero (1): The size of the file is less than zero.
SizeMoreThanSuggested (2): The size of the file is more than the suggested maximum size.
InvalidExtension (3): The file has an invalid extension.
NullValues (4): One or more required values are null.
FileNameAlreadyExists (5): The specified file name already exists in the target directory.
FileWriteError (6): An error occurred while writing the file to disk.
NoError (7): No error occurred during the file upload operation.
Others (8): Represents other unspecified errors.


Example: 

1) UploadFile:
	public IActionResult Index(IFormFile file)
        {
            List<string> extension = new List<string>();
            extension.Add(".pdf");

            using (var memoryStream = new MemoryStream())
            {
                using (var fileStream = file.OpenReadStream())
                {
                    fileStream.CopyTo(memoryStream);
                }

                byte[] fileBytes = memoryStream.ToArray();
                UploadFileService uploadFileService = new UploadFileService();
                ErrorTypes result = uploadFileService.UploadFile("wwwroot", "hero", file.FileName, file.Length, Path.GetExtension(file.FileName), extension, 500000000,fileBytes);
                if(result == ErrorTypes.Success)
                {
                    return Ok("successful");
                }
                else
                {
                    return BadRequest("Error: " + result);
                }
            }    
        }

2) UploadFileAsync:
	public async Task<IActionResult> Index(IFormFile file)
        {
            List<string> extension = new List<string>();
            extension.Add(".pdf");

            using (var memoryStream = new MemoryStream())
            {
                using (var fileStream = file.OpenReadStream())
                {
                    fileStream.CopyTo(memoryStream);
                }

                byte[] fileBytes = memoryStream.ToArray();
                UploadFileService uploadFileService = new UploadFileService();
                ErrorTypes result = await uploadFileService.UploadFileAsync("wwwroot", "hero", file.FileName, file.Length, Path.GetExtension(file.FileName), extension, 500000000,fileBytes);
                if(result == ErrorTypes.Success)
                {
                    return Ok("successful");
                }
                else
                {
                    return BadRequest("Error: " + result);

                }
            }
        }

        
3) UploadFile (alternate version with IFormFile parameter):
    public IActionResult IndexNew(IFormFile file)
        {
            List<string> extension = new List<string>();
            extension.Add(".pdf");

            UploadFileService uploadFileService = new UploadFileService();
            ErrorTypes result = uploadFileService.UploadFile("wwwroot", "hero", file, extension, 500000000);
            if (result == ErrorTypes.Success)
            {
                return Ok("successful");
            }
            else
            {
                return BadRequest("Error: " + result);
            }
        }

4) UploadFileAsync (alternate version with IFormFile parameter):
    public async Task<IActionResult> IndexNewAsync(IFormFile file)
        {
            List<string> extension = new List<string>();
            extension.Add(".pdf");

            UploadFileService uploadFileService = new UploadFileService();
            ErrorTypes result = await uploadFileService.UploadFileAsync("wwwroot", "hero", file, extension, 500000000);
            if (result == ErrorTypes.Success)
            {
                return Ok("successful");
            }
            else
            {
                return BadRequest("Error: " + result);

            }
        }
