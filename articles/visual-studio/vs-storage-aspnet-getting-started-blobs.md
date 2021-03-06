---
title: Azure Blob 저장소 및 Visual Studio 연결된 서비스 시작(ASP.NET) | Microsoft Docs
description: Visual Studio 연결된 서비스를 사용하여 저장소 계정에 연결한 후 Visual Studio의 ASP.NET 프로젝트에서 Azure Blob 저장소를 사용하는 방법에 대해 설명합니다.
services: storage
author: ghogen
manager: douge
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: ghogen
ms.openlocfilehash: 55b083ac5384e749098338d2f3b7b24bfe16fe8f
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31799552"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Azure Blob 저장소 및 Visual Studio 연결된 서비스 시작(ASP.NET)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob 저장소는 구조화되지 않은 데이터를 개체 또는 Blob으로 클라우드에 저장하는 서비스입니다. Blob storage는 문서, 미디어 파일 또는 응용 프로그램 설치 프로그램과 같은 모든 종류의 텍스트 또는 이진 데이터를 저장할 수 있습니다. 또한 Blob storage를 개체 저장소라고 합니다.

이 자습서에서는 Blob 저장소를 사용하는 몇 가지 일반적인 시나리오에 대한 ASP.NET 코드를 작성하는 방법을 보여 줍니다. 시나리오에는 Blob 컨테이너 만들기, Blob 업로드, 나열, 다운로드 및 삭제가 포함됩니다.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>필수 조건

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]


[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

## <a name="create-an-mvc-controller"></a>MVC 컨트롤러 만들기 

1. **솔루션 탐색기**에서 **컨트롤러**를 마우스 오른쪽 단추로 클릭합니다.

2. 상황에 맞는 메뉴에서 **추가** > **컨트롤러**를 선택합니다.

    ![추가 및 컨트롤러가 강조 표시된 솔루션 탐색기 스크린샷](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. **스캐폴드 추가** 대화 상자에서 **MVC 5 컨트롤러 - 비어 있음**을 선택하고 **추가**를 선택합니다.

    ![스캐폴드 추가 대화 상자 스크린샷](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. **컨트롤러 추가** 대화 상자에서 컨트롤러 이름을*BlobsController*로 설정하고 **추가**를 선택합니다.

    ![컨트롤러 추가 대화 상자 스크린샷](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. 다음 `using` 지시문을 `BlobsController.cs` 파일에 추가합니다.

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>저장소 계정에 연결 및 컨테이너 참조 가져오기

Blob 컨테이너는 Blob 및 폴더의 중첩된 계층 구조입니다. 이 문서의 나머지 단계에는 Blob 컨테이너에 대한 참조가 필요하므로 해당 코드를 재사용하기 위해 자체 메서드에 배치해야 합니다.

다음 단계에서는 **Web.config**의 연결 문자열을 사용하여 저장소 계정에 연결하는 메서드를 만들고 컨테이너에 대한 참조도 만듭니다.  **Web.config**의 연결 문자열 설정은 `<storageaccountname>_AzureStorageConnectionString` 형식으로 명명됩니다. 

1. `BlobsController.cs` 파일을 엽니다.

1. **CloudBlobContainer**를 반환하는**GetCloudBlobContainer**라는 메서드를 추가합니다.  `<storageaccountname>_AzureStorageConnectionString`을 **Web.config**에 있는 키의 실제 이름으로 바꿔야 합니다.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
                CloudConfigurationManager.GetSetting("<storageaccountname>_AzureStorageConnectionString"));
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> *test-blob-container*가 아직 없더라도 이 코드는 해당 참조를 만듭니다. 따라서 다음 단계에 표시된 `CreateIfNotExists` 메서드를 사용하여 컨테이너를 만들 수 있습니다.

## <a name="create-a-blob-container"></a>Blob 컨테이너 만들기

다음 단계에서는 Blob 컨테이너를 만드는 방법을 보여 줍니다.

1. `ActionResult`를 반환하는 `CreateBlobContainer`라는 메서드를 추가합니다.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. 원하는 Blob 컨테이너 이름에 대한 참조를 나타내는 `CloudBlobContainer` 개체를 가져옵니다. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. 컨테이너가 아직 없는 경우 `CloudBlobContainer.CreateIfNotExists` 메서드를 호출하여 만듭니다. 컨테이너가 없고 성공적으로 만들어지면 `CloudBlobContainer.CreateIfNotExists` 메서드에서 **true**를 반환합니다. 그렇지 않으면 메서드에서 **false**가 반환됩니다.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. `ViewBag`을 Blob 컨테이너 이름으로 업데이트합니다.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    다음에서는 완성된 `CreateBlobContainer` 메서드를 보여줍니다.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExists();
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. **솔루션 탐색기**에서 **Views** 폴더를 마우스 오른쪽 단추로 클릭합니다.

2. 상황에 맞는 메뉴에서 **추가** > **새 폴더**를 선택합니다. 새 폴더 이름을 *Blobs*로 지정합니다. 
 
1. **솔루션 탐색기**에서 **뷰** 폴더를 확장하고 **Blob**을 마우스 오른쪽 단추로 클릭합니다.

4. 상황에 맞는 메뉴에서 **추가** > **뷰**를 선택합니다.

1. **뷰 추가** 대화 상자에서 뷰 이름으로 **CreateBlobContainer**를 입력하고 **추가**를 선택합니다.

1. `CreateBlobContainer.cshtml`을 열고 다음 코드 조각과 같이 수정합니다.

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. **솔루션 탐색기**에서 **뷰** > **공유됨** 폴더를 확장하고 `_Layout.cshtml`을 엽니다.

1. 마지막 **Html.ActionLink** 뒤에 다음 **Html.ActionLink**를 추가합니다.

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. 응용 프로그램을 실행하고 **Blob 컨테이너 만들기**를 선택하여 다음 스크린샷과 유사한 결과를 확인합니다.
  
    ![Blob 컨테이너 만들기 스크린샷](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    앞에서 언급했듯이 컨테이너가 없고 만들어진 경우에만 `CloudBlobContainer.CreateIfNotExists` 메서드에서 **true**를 반환합니다. 따라서 컨테이너가 있을 때 앱을 실행하면 메서드에서 **false**를 반환합니다.

## <a name="upload-a-blob-into-a-blob-container"></a>Blob 컨테이너에 Blob 업로드

[Blob 컨테이너가 생성](#create-a-blob-container)되면 해당 컨테이너에 파일을 업로드합니다. 이 섹션에서는 Blob 컨테이너에 로컬 파일을 업로드하는 과정을 안내합니다. 이 단계에서는 *test-blob-container*라는 이름의 Blob 컨테이너가 있다고 가정합니다. 

1. `BlobsController.cs` 파일을 엽니다.

1. 문자열을 반환하는 `UploadBlob`라는 메서드를 추가합니다.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. `UploadBlob` 메서드 내에서 원하는 Blob 컨테이너 이름에 대한 참조를 나타내는 `CloudBlobContainer` 개체를 가져옵니다. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Azure Storage는 다양한 Blob 유형을 지원합니다. 이 자습서에서는 블록 Blob을 사용합니다. 블록 Blob에 대한 참조를 검색하려면 `CloudBlobContainer.GetBlockBlobReference` 메서드를 호출합니다.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Blob 이름은 Blob를 검색하는 데 사용되는 URL의 일부이고 파일의 이름을 포함한 문자열일 수 있습니다.

1. Blob 참조가 있으면 Blob 참조 개체의 `UploadFromStream` 메서드를 호출하여 해당 참조에 데이터 스트림을 업로드할 수 있습니다. `UploadFromStream` 메서드는 Blob이 없는 경우 새로 만들고, Blob이 있는 경우 덮어씁니다. *&lt;file-to-upload>* 를 업로드하려는 파일의 정규화된 경로로 변경합니다.

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStream(fileStream);
    }
    ```
    
    다음에서는 `UploadBlob` 메서드를 보여줍니다(업로드할 파일의 정규화된 경로 포함).

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStream(fileStream);
        }
        return "success!";
    }
    ```

1. **솔루션 탐색기**에서 **뷰** > **공유됨** 폴더를 확장하고 `_Layout.cshtml`을 엽니다.

1. 마지막 **Html.ActionLink** 뒤에 다음 **Html.ActionLink**를 추가합니다.

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. 응용 프로그램을 실행하고 **Blob 업로드**를 선택합니다.  ‘성공!’이라는 단어가 표시됩니다.
    
    ![성공 확인 스크린샷](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>Blob 컨테이너에 Blob 나열

이 섹션에서는 Blob 컨테이너에 Blob을 나열하는 방법을 보여 줍니다. 샘플 코드는 [Blob 컨테이너 만들기](#create-a-blob-container) 섹션에서 만든 *test-blob-container*를 참조합니다.

1. `BlobsController.cs` 파일을 엽니다.

1. `ActionResult`를 반환하는 `ListBlobs`라는 메서드를 추가합니다.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. `ListBlobs` 메서드 내에서 Blob 컨테이너에 대한 참조를 나타내는 `CloudBlobContainer` 개체를 가져옵니다. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Blob 컨테이너의 Blob을 나열하려면 `CloudBlobContainer.ListBlobs` 메서드를 사용합니다. `CloudBlobContainer.ListBlobs` 메서드가 `CloudBlockBlob`, `CloudPageBlob` 또는 `CloudBlobDirectory` 개체로 캐스팅될 수 있는 `IListBlobItem` 개체를 반환합니다. 다음 코드 조각은 Blob 컨테이너에 있는 모든 Blob를 열거합니다. 각 Blob은 해당 유형에 따라 적절한 개체로 캐스팅됩니다. 이름(또는 **CloudBlobDirectory**의 경우 URI)이 목록에 추가됩니다.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs())
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Blob 컨테이너는 Blob 외에도 디렉터리를 포함할 수 있습니다. 다음 계층 구조를 가진 *test-blob-container*라는 Blob 컨테이너가 있다고 가정해 보겠습니다.

        foo.png
        dir1/bar.png
        dir2/baz.png

    앞의 코드 예제를 사용하면 **Blob** 문자열 목록에 다음과 비슷한 값이 포함됩니다.

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    표시된 대로 목록에는 중첩된 엔터티가 아닌 최상위 엔터티만 포함됩니다(*bar.png* 및 *baz.png*). Blob 컨테이너 내의 모든 엔터티를 나열하려면 **CloudBlobContainer.ListBlobs** 메서드가 **useFlatBlobListing** 매개 변수에 **true**로 전달되도록 코드를 변경합니다.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    **useFlatBlobListing** 매개 변수를 **true**로 설정하면 Blob 컨테이너의 모든 항목에 대한 플랫 목록이 반환되고 다음 결과가 생성됩니다.

        foo.png
        dir1/bar.png
        dir2/baz.png
    
    다음에서는 완성된 **ListBlobs** 메서드를 보여줍니다.

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing: true))
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. **솔루션 탐색기**에서 **뷰** 폴더를 확장하고 **Blob**을 마우스 오른쪽 단추로 클릭합니다.

2. 상황에 맞는 메뉴에서 **추가** > **뷰**를 선택합니다.

1. **뷰 추가** 대화 상자에서 뷰 이름으로 `ListBlobs`을 입력하고 **추가**를 선택합니다.

1. `ListBlobs.cshtml`를 열고 내용을 다음 코드로 바꿉니다.

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. **솔루션 탐색기**에서 **뷰** > **공유됨** 폴더를 확장하고 `_Layout.cshtml`을 엽니다.

1. 마지막 **Html.ActionLink** 뒤에 다음 **Html.ActionLink**를 추가합니다.

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. 응용 프로그램을 실행하고 **Blob 나열**을 선택하여 다음 스크린샷과 유사한 결과를 확인합니다.
  
    ![Blob 나열 스크린샷](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blob 다운로드

이 섹션에서는 Blob을 다운로드하는 방법을 보여 줍니다. 로컬 저장소에 저장하거나 콘텐츠를 문자열로 읽어올 수 있습니다. 샘플 코드는 [Blob 컨테이너 만들기](#create-a-blob-container) 섹션에서 만든 *test-blob-container*를 참조합니다.

1. `BlobsController.cs` 파일을 엽니다.

1. 문자열을 반환하는 `DownloadBlob`라는 메서드를 추가합니다.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. `DownloadBlob` 메서드 내에서 Blob 컨테이너에 대한 참조를 나타내는 `CloudBlobContainer` 개체를 가져옵니다.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. `CloudBlobContainer.GetBlockBlobReference` 메서드를 호출하여 Blob 참조 개체를 가져옵니다. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Blob을 다운로드하려면 `CloudBlockBlob.DownloadToStream` 메서드를 사용합니다. 다음 코드는 Blob의 콘텐츠를 스트림 개체로 전송합니다. 그런 다음 해당 개체가 로컬 파일에 저장됩니다. Blob을 다운로드할 위치를 나타내는 정규화된 파일 이름으로 *&lt;local-file-name>* 을 변경합니다. 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    다음에서는 `ListBlobs` 메서드를 보여줍니다(만들 로컬 파일의 정규화된 경로 포함).
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStream(fileStream);
        }
        return "success!";
    }
    ```

1. **솔루션 탐색기**에서 **뷰** > **공유됨** 폴더를 확장하고 `_Layout.cshtml`을 엽니다.

1. 마지막 **Html.ActionLink** 뒤에 다음 **Html.ActionLink**를 추가합니다.

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. 응용 프로그램을 실행하고 **Blob 다운로드**를 선택하여 Blob을 다운로드합니다. `CloudBlobContainer.GetBlockBlobReference` 메서드 호출에 지정된 Blob은 `File.OpenWrite` 메서드 호출에서 지정된 위치에 다운로드됩니다.  ‘성공!’이라는 텍스트가 브라우저에 표시됩니다. 

## <a name="delete-blobs"></a>Blob 삭제

다음 단계에서는 Blob을 삭제하는 방법을 보여 줍니다.

1. `BlobsController.cs` 파일을 엽니다.

1. 문자열을 반환하는 `DeleteBlob`라는 메서드를 추가합니다.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. `DeleteBlob` 메서드 내에서 Blob 컨테이너에 대한 참조를 나타내는 `CloudBlobContainer` 개체를 가져옵니다.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. `CloudBlobContainer.GetBlockBlobReference` 메서드를 호출하여 Blob 참조 개체를 가져옵니다. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Blob을 삭제하려면 `Delete` 메서드를 사용합니다.

    ```csharp
    blob.Delete();
    ```
    
    전체 `DeleteBlob` 메서드는 다음과 같아야 합니다.
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.Delete();
        return "success!";
    }
    ```

1. **솔루션 탐색기**에서 **뷰** > **공유됨** 폴더를 확장하고 `_Layout.cshtml`을 엽니다.

1. 마지막 **Html.ActionLink** 뒤에 다음 **Html.ActionLink**를 추가합니다.

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. 응용 프로그램을 실행하고 **Blob 삭제**를 선택하여 `CloudBlobContainer.GetBlockBlobReference` 메서드 호출에서 지정한 Blob을 삭제합니다. ‘성공!’이라는 텍스트가 브라우저에 표시됩니다. 브라우저의 **뒤로** 단추를 선택한 다음 **Blob 나열**을 선택하여 컨테이너에 더 이상 Blob이 없음을 확인합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 ASP.NET을 사용하여 Azure Storage에서 Blob을 저장, 나열 및 검색하는 방법을 알아보았습니다. Azure에 데이터를 저장하기 위한 추가 옵션에 대한 자세한 내용은 추가 기능 가이드를 참조하십시오.

  * [Azure Table Storage 및 Visual Studio 연결된 서비스 시작(ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Azure Queue Storage 및 Visual Studio 연결된 서비스 시작(ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
