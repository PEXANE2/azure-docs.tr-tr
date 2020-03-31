---
title: Blob yolunu varsayılandan değiştirme
description: Blob dosya yolunu yeniden adlandırmak için azure işlevini nasıl ayarlayizleyeceğinizi öğrenin
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 5ba1709ae195631371e4ea72667ba9b2a4bf279e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76270622"
---
# <a name="change-a-blob-path-from-the-default-path"></a>Varsayılan yoldan blob yolunu değiştirme

StorSimple Data Manager hizmeti verileri dönüştürdüğünde, varsayılan olarak dönüştürülmüş lekeleri hedef deposunun oluşturulması sırasında belirtildiği gibi bir depolama kabına yerleştirir. Lekeler bu konuma geldiğinde, bu lekeleri başka bir konuma taşımak isteyebilirsiniz. Bu makalede, varsayılan blob dosya yolunu yeniden adlandırmak ve dolayısıyla lekeleri farklı bir konuma taşımak için bir Azure işlevinin nasıl ayarlanır.

## <a name="prerequisites"></a>Ön koşullar

StorSimple Data Manager hizmetinizde doğru yapılandırılmış bir iş tanımına sahip olduğundan emin olun.

## <a name="create-an-azure-function"></a>Azure işlevi oluşturma

Bir Azure işlevi oluşturmak için aşağıdaki adımları gerçekleştirin:

1. [Azure portalına](https://portal.azure.com/)gidin.

2. + **Kaynak oluştur'u**tıklatın. **Arama** **kutusunda, Fonksiyon Uygulaması** yazın ve **Enter**tuşuna basın. Görüntülenen uygulamalar listesinde **İşlev uygulamasını** seçin ve tıklayın.

    ![Arama kutusuna "İşlev Uygulaması" yazın](./media/storsimple-data-manager-change-default-blob-path/search-function-app.png)

3. **Oluştur'u**tıklatın.

    ![İşlev Uygulaması penceresi "Oluştur" düğmesi](./media/storsimple-data-manager-change-default-blob-path/create-function-app.png)

4. Function **App** yapılandırma bıçağında aşağıdaki adımları gerçekleştirin:

    1. Benzersiz bir **Uygulama adı**sağlayın.
    2. Açılan listeden **Abonelik'i**seçin. Bu abonelik, StorSimple Data Manager hizmetinizle ilişkili abonelikle aynı olmalıdır.
    3. Yeni kaynak grubu **oluştur'u** seçin.
    4. Barındırma **Planı** açılır listesi için **Tüketim Planı'nı**seçin.
    5. İşlevinizin çalıştığı yeri belirtin. StorSimple Data Manager hizmetinin ve iş tanımıyla ilişkili depolama hesabının bulunduğu aynı bölgeyi istiyorsunuz.
    6. Mevcut bir depolama hesabını seçin veya yeni bir depolama hesabı oluşturun. İşlev için dahili olarak bir depolama hesabı kullanılır.

        ![Yeni İşlev Uygulaması yapılandırma verilerini girin](./media/storsimple-data-manager-change-default-blob-path/function-app-parameters.png)

    7. **Oluştur'u**tıklatın. İşlev uygulaması oluşturulur.
     
        ![Fonksiyon Uygulaması oluşturuldu](./media/storsimple-data-manager-change-default-blob-path/function-app-created.png)

5. **İşlevler'i**seçin ve **+ Yeni işlevi**tıklatın.

    ![+ Yeni işlevi tıklatın](./media/storsimple-data-manager-change-default-blob-path/create-new-function.png)

6. Dil için **C#** seçeneğini belirleyin. Şablon kutucukdizisinde **QueueTrigger-CSharp** kutucuğunda **C#** seçeneğini belirleyin.

7. Kuyruk **tetikleyicisinde:**

    1. Işleviniz için bir **Ad** girin.
    2. Sıra **adı** kutusuna, veri dönüştürme iş tanımı adınızı yazın.
    3. **Depolama hesabı bağlantısı**altında, **yeni**yi tıklatın. Depolama hesapları listesinden, iş tanımınızla ilişkili hesabı seçin. Bağlantı adını not edin (vurgulanır). Ad daha sonra Azure işlevinde gereklidir.

        ![Yeni bir C# işlevi oluşturma](./media/storsimple-data-manager-change-default-blob-path/new-function-parameters.png)

    4. **Oluştur'u**tıklatın. **İşlev** oluşturulur.

     
10. İşlev penceresinde _.csx_ dosyasını çalıştırın.

    ![Yeni bir C# işlevi oluşturma](./media/storsimple-data-manager-change-default-blob-path/new-function-run-csx.png)
    
    Aşağıdaki adımları uygulayın.

    1. Aşağıdaki kodu yapıştırın:

        ```
        using System;
        using System.Configuration;
        using Microsoft.WindowsAzure.Storage.Blob;
        using Microsoft.WindowsAzure.Storage.Queue;
        using Microsoft.WindowsAzure.Storage;
        using System.Collections.Generic;
        using System.Linq;

        public static void Run(QueueItem myQueueItem, TraceWriter log)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

            string storageAccUriEndswith = "windows.net/";
            string uri = myQueueItem.TargetLocation.Replace("%20", " ");
            log.Info($"Blob Uri: {uri}");

            // Remove storage account uri string
            uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

            string containerName = uri.Substring(0, uri.IndexOf("/")); 

            // Remove container name string
            uri = uri.Substring(containerName.Length + 1);

            // Current blob path
            string blobName = uri; 

            string volumeName = uri.Substring(containerName.Length + 1);
            volumeName = uri.Substring(0, uri.IndexOf("/"));

            // Remove volume name string
            uri = uri.Substring(volumeName.Length + 1);

            string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
            string newBlobName = uri.Substring(newContainerName.Length + 1);

            log.Info($"Container name: {containerName}");
            log.Info($"Volume name: {volumeName}");
            log.Info($"New container name: {newContainerName}");

            log.Info($"Blob name: {blobName}");
            log.Info($"New blob name: {newBlobName}");

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Container reference
            CloudBlobContainer container = blobClient.GetContainerReference(containerName);
            CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
            newContainer.CreateIfNotExists();

            if(!container.Exists())
            {
                log.Info($"Container - {containerName} not exists");
                return;
            }

            if(!newContainer.Exists())
            {
                log.Info($"Container - {newContainerName} not exists");
                return;
            }

            CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
            if (!blob.Exists())
            {
                // Skip to copy the blob to new container, if source blob doesn't exist
                log.Info($"The specified blob does not exist.");
                log.Info($"Blob Uri: {blob.Uri}");
                return;
            }

            CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
            if (!blobCopy.Exists())
            {
                blobCopy.StartCopy(blob);
                // Delete old blob, after copy to new container
                blob.DeleteIfExists();
                log.Info($"Blob file path renamed completed successfully");
            }
            else
            {
                log.Info($"Blob file path renamed already done");
                // Delete old blob, if already exists.
                blob.DeleteIfExists();
            }
        }

        public class QueueItem
        {
            public string SourceLocation {get;set;}
            public long SizeInBytes {get;set;}
            public string Status {get;set;}
            public string JobID {get;set;}
            public string TargetLocation {get; set;}
        }

        ```

    2. 11. satırdaki **STORAGE_CONNECTIONNAME** depolama hesabı bağlantınızla değiştirin (7c adımına bakın).

        ![Depolama bağlantı adını kopyalama](./media/storsimple-data-manager-change-default-blob-path/new-function-storage-connection-name.png)

    3. İşlevi **kaydedin.**

        ![Fonksiyonu kaydet](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. İşlevi tamamlamak için aşağıdaki adımları yaparak bir dosya daha ekleyin:

    1. **Dosyaları Görüntüle'yi**tıklatın.

       !["Dosyaları görüntüle" bağlantısı](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    2. **+ Ekle**'ye tıklayın.
        
        !["Dosyaları görüntüle" bağlantısı](./media/storsimple-data-manager-change-default-blob-path/new-function-add-file.png)
    
    3. **project.json**yazın ve enter **tuşuna**basın. **project.json** dosyasında aşağıdaki kodu yapıştırın:

        ```
        {
        "frameworks": {
            "net46":{
            "dependencies": {
                "windowsazure.storage": "8.1.1"
            }
            }
        }
        }

        ```

    
    4. **Kaydet**'e tıklayın.

        !["Dosyaları görüntüle" bağlantısı](./media/storsimple-data-manager-change-default-blob-path/new-function-project-json.png)

Bir Azure işlevi oluşturdunuz. Bu işlev, veri dönüştürme işi tarafından her yeni bir blob oluşturulduğunda tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

[Verilerinizi dönüştürmek için StorSimple Data Manager Kullanıcı UI'ı kullanın](storsimple-data-manager-ui.md)
