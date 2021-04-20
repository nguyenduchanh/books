# Upload file lên Google Drive
# Tạo Credential

- Vào đường link sau để tạo project:
[https://console.developers.google.com](https://console.developers.google.com)
![alt text](https://raw.githubusercontent.com/nguyenduchanh/books/master/Images/google_drive_create_project.png?token=ADEKSVMNWKRZW3EI63NUZZLAPZ35W "create project")

- Lưu lại project Id: *carbide-ratio-311303*
- Enable google drive api
![alt text](https://raw.githubusercontent.com/nguyenduchanh/books/57006875c246e5a89f9e7eba5c7ac192383c4738/Images/google_drive_enable_api.png "enable api")
- Chọn enable

![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_enable_api_form.png?raw=true "enable form")
- Tạo credential
![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_create_credential.png?raw=true "create credential 1")
![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_create_credential_2.png?raw=true "create credential 1")
- Tạo OAth client id
![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_create_OAuth_Client_Id.png?raw=true "OAth client id 1")
![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_create_OAuth_Client_Id_2.png?raw=true "OAth client id 2")
![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_create_OAuth_Client_Id_3.png?raw=true "OAth client id 3")
- Lưu lại client id và client secret
Client Id: 275344795279-k8db2g7ab09s3rm6dq3mvmngbtvkchv7.apps.googleusercontent.com
Client Secret: mYwdJLOMsuxtZg1GmLOanGyR
- Tạo file : client_secret.json có nội dụng như sau
```sh
{
   "installed":{
      "client_id":"649642596267-r9q7u1p0nnus1mp03kdcosua3grhj8kk.apps.googleusercontent.com",
      "project_id":"baexpress-310801",
      "auth_uri":"https://accounts.google.com/o/oauth2/auth",
      "token_uri":"https://accounts.google.com/o/oauth2/token",
      "auth_provider_x509_cert_url":"https://www.googleapis.com/oauth2/v1/certs",
      "client_secret":"sTWFfEZ3yZvH68fL2p9FMZiu",
      "redirect_uris":[
         "urn:ietf:wg:oauth:2.0:oob",
         "http://localhost"
      ]
   }
}
```
với client_id, project_id, client_secret đã lấy bên trên. đặt file trong thư mục chạy (bin/debug hoặc bin/release)
- Tạo 1 folder để lưu trữ. Lấy id của folder
![alt text](https://github.com/nguyenduchanh/books/blob/master/Images/google_drive_get_folder_id.png?raw=true "get folder id")
- Hàm lấy Credential
```sh
private static UserCredential GetCredentials()
    {
        using (var stream = new FileStream("client_secret.json", FileMode.Open, FileAccess.Read))
        {
            string credPath = System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal);
            credPath = Path.Combine(credPath, "driveApiCredentials","client_secreta.json");
            return GoogleWebAuthorizationBroker.AuthorizeAsync(
                GoogleClientSecrets.Load(stream).Secrets,
                Scopes,"User",
                CancellationToken.None,
                new FileDataStore(credPath, true)).Result;
        }
    }
```
- Hàm lấy drive service
```sh
private static DriveService GetDriveService(UserCredential credential)
    {
        return new DriveService(
            new BaseClientService.Initializer { 
            HttpClientInitializer = credential,
            ApplicationName = ApplicationName
            });
    }
```
- Hàm upload file lên drive
```sh
        private static string[] Scopes = { DriveService.Scope.Drive };
        private static string ApplicationName = "BaExpressUploadToDrive";
        private static string folderId = "1VVhp4rjekp-axnrFPb8lbE93UjN7GpGi";
        private static string fileName = "testFile.mp4";
        private static string filePath = @"C:\Users\hanhnd.BA\Downloads\Example.mp4";
        private static string contentType = "video/mp4";
        private static string UpdoadFileToDrive(DriveService service, string fileName, string filePath, string contentType) 
        {
            var fileMetadata = new File();
            fileMetadata.Name = fileName;
            fileMetadata.Parents = new List<string> { folderId};
            FilesResource.CreateMediaUpload request;
            using (var stream = new FileStream(filePath, FileMode.Open)) {
                request = service.Files.Create(fileMetadata, stream, contentType);
                request.Upload(); 
                    }
            var file = request.ResponseBody;
                return file.Id;
        }

```
- Hàm lấy danh sách file theo tên
```sh
        public static List<string> GetFileByName(string fileName) {
            var urlList = new List<string>() { };
            var fileList = ListAll("name=\"" + fileName+ "\"");
            foreach (var file in fileList.Files)
            {
                var url = "https://drive.google.com/file/d/" + file.Id.ToString() + "/view";
              urlList.Add(url);
            }
            return urlList;
        }
        public static Google.Apis.Drive.v3.Data.FileList ListAll(string query)
        {
            UserCredential credential = GetCredentials();
            DriveService service = GetDriveService(credential);
            string pageToken = null;
            var request = service.Files.List();
            request.Q = query;
            request.Fields = "nextPageToken, files(id, name,parents,mimeType)";

            request.PageToken = pageToken;
            // Add this if you are facing certificate issue or not able to connect from you machine
            System.Net.ServicePointManager.ServerCertificateValidationCallback = delegate (object sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors sslPolicyErrors) { return true; };
            var result = request.Execute();
            return result;
        }

```

- chương trình đầy đủ
```sh
    public static class UploadToGoogleDrive
    {
        private static string[] Scopes = { DriveService.Scope.Drive };
        private static string ApplicationName = "BaExpressUploadToDrive";
        private static string folderId = "1VVhp4rjekp-axnrFPb8lbE93UjN7GpGi";
        private static string fileName = "testFile.mp4";
        private static string filePath = @"C:\Users\hanhnd.BA\Downloads\Example.mp4";
        private static string contentType = "video/mp4";
        private static string viewFile = "https://drive.google.com/file/d/{0}/view";
        public static void Upload()
        {
            //UserCredential credential = GetCredentials();
            //DriveService service = GetDriveService(credential);
            //UpdoadFileToDrive(service, fileName, filePath, contentType);
            
            var result = ConvertLink("testFile.mp4");
        }
        /// <summary>
        /// 
        /// </summary>
        /// <param name="fileName"></param>
        public static List<string> GetFileByName(string fileName) {
            var urlList = new List<string>() { };
            var fileList = ListAll("name=\"" + fileName+ "\"");
            foreach (var file in fileList.Files)
            {
                var url = "https://drive.google.com/file/d/" + file.Id.ToString() + "/view";
                urlList.Add(url);
            }
            return urlList;
        }
        public static Google.Apis.Drive.v3.Data.FileList ListAll(string query)
        {
            UserCredential credential = GetCredentials();
            DriveService service = GetDriveService(credential);
            string pageToken = null;
            var request = service.Files.List();
            request.Q = query;
            request.Fields = "nextPageToken, files(id, name,parents,mimeType)";

            request.PageToken = pageToken;
            // Add this if you are facing certificate issue or not able to connect from you machine
            System.Net.ServicePointManager.ServerCertificateValidationCallback = delegate (object sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors sslPolicyErrors) { return true; };
            var result = request.Execute();
            return result;
        }
        private static UserCredential GetCredentials()
        {
            using (var stream = new FileStream("client_secret.json", FileMode.Open, FileAccess.Read))
            {
                string credPath = System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal);
                credPath = Path.Combine(credPath, "driveApiCredentials","client_secreta.json");
                return GoogleWebAuthorizationBroker.AuthorizeAsync(
                    GoogleClientSecrets.Load(stream).Secrets,
                    Scopes,"User",
                    CancellationToken.None,
                    new FileDataStore(credPath, true)).Result;
            }
        }
        private static DriveService GetDriveService(UserCredential credential)
        {
            return new DriveService(
                new BaseClientService.Initializer { 
                HttpClientInitializer = credential,
                ApplicationName = ApplicationName
                });
        }
        private static string UpdoadFileToDrive(DriveService service, string fileName, string filePath, string contentType) {
            var fileMetadata = new File();
            fileMetadata.Name = fileName;
            fileMetadata.Parents = new List<string> { folderId};
            FilesResource.CreateMediaUpload request;
            using (var stream = new FileStream(filePath, FileMode.Open)) {
                request = service.Files.Create(fileMetadata, stream, contentType);
                request.Upload(); 
                    }
            var file = request.ResponseBody;
                return file.Id;
        }
    }
``` 