---
title: 'Quickstart: Tarayıcıda JavaScript v10 için Azure Blob depolama'
description: Bir HTML sayfasında JavaScript v10 SDK'yı kullanarak blob'ları yüklemeyi, listelemeyi ve silmeyi öğrenin.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.author: mhopkins
ms.date: 01/24/2020
ms.topic: quickstart
ms.subservice: blobs
ms.openlocfilehash: da5db7d956b1ba8aa1ac245b77fe0d4cb31909c1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061458"
---
<!-- Customer intent: As a web application developer I want to interface with Azure Blob storage entirely on the client so that I can build a SPA application that is able to upload and delete files on blob storage. -->

# <a name="quickstart-manage-blobs-with-javascript-v10-sdk-in-browser"></a>Quickstart: Tarayıcıda JavaScript v10 SDK ile blobs yönetin

Bu hızlı başlatmada, tamamen tarayıcıda çalışan JavaScript kodunu kullanarak lekeleri yönetmeyi öğrenirsiniz. Blobs, resimler, belgeler, akışlı ortam ve arşiv verileri de dahil olmak üzere büyük miktarda metin veya ikili veri tutabilen nesnelerdir. Blob depolama hesabınıza korumalı erişim sağlamak için gerekli güvenlik önlemlerini kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- Azure Depolama hesabı. [Bir depolama hesabı oluşturun.](../common/storage-account-create.md)
- Yerel bir web sunucusu. Bu makalede, temel bir sunucu açmak için [Node.js](https://nodejs.org) kullanır.
- [Görsel Stüdyo Kodu](https://code.visualstudio.com).
- Tarayıcı hata ayıklama için Bir VS Kodu uzantısı, [Chrome için Hata Ayıklama](vscode:extension/msjsdiag.debugger-for-chrome) veya Microsoft [Edge için Hata Ayıklama](vscode:extension/msjsdiag.debugger-for-edge)gibi.

## <a name="setting-up-storage-account-cors-rules"></a>Depolama hesabı CORS kurallarını ayarlama

Web uygulamanız istemciden bir blob depolama alanına erişebilmesi için önce, hesabınızı [orijinler arası kaynak paylaşımını](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services)veya CORS'i etkinleştirecek şekilde yapılandırmanız gerekir.

Azure portalına geri dönün depolama hesabınızı seçin. Yeni bir CORS kuralını tanımlamak için **Ayarlar** bölümüne gidin ve **CORS** bağlantısını tıklayın. Ardından, **Ekle** düğmesine tıklayarak **CORS kuralı ekle** penceresini açın. Bu hızlı başlangıçta bir açık CORS kuralı oluşturacaksınız:

![Azure Blob Depolama Hesabı CORS ayarları](media/storage-quickstart-blobs-javascript-client-libraries-v10/azure-blob-storage-cors-settings.png)

Aşağıdaki tabloda her bir CORS ayarı açıklanmakta ve kuralı tanımlamak için kullanılan değerler anlatılmaktadır.

|Ayar  |Değer  | Açıklama |
|---------|---------|---------|
| İzin verilen çıkış noktaları | * | Kabul edilebilir çıkış noktaları olarak etki alanları kümesinin virgülle ayrılmış bir listesini kabul eder. Değerin `*` olarak ayarlanması, depolama hesabına tüm etki alanlarının erişmesine izin verir. |
| İzin verilen yöntemler     | silme, alma, başlık, birleştirme, yayınlama, seçenekler ve yerleştirme | Depolama hesabına göre yürütülmesine izin verilen HTTP fiillerini listeler. Bu hızlı başlangıçta tüm kullanılabilir seçenekleri işaretleyin. |
| İzin verilen üst bilgiler | * | Depolama hesabı tarafından izin verilen istek üst bilgilerinin (ön ekli üst bilgiler dahil) listesini tanımlar. Değerin `*` olarak ayarlanması tüm üst bilgilere erişim izni verir. |
| Kullanıma sunulan üst bilgiler | * | Hesaba göre izin verilen yanıt üst bilgilerini listeler. Değerin `*` olarak ayarlanması hesabın herhangi bir üst bilgiyi göndermesine izin verir.  |
| Maksimum yaş (saniye) | 86400 | Tarayıcının denetim öncesi SEÇENEKLER isteğini önbelleğe aldığı en uzun süre. *86400* değeri, önbelleğin bir tam gün boyunca kalmasına izin verir. |

> [!IMPORTANT]
> Üretimde kullandığınız ayarların, güvenli erişimi sağlamak için depolama hesabınıza gereken minimum erişim miktarını ortaya çıkardığından emin olun. Burada açıklanan CORS ayarları esnek bir güvenlik ilkesini tanımladığı için hızlı başlangıç için uygundur. Ancak bu ayarlar gerçek bir bağlam için önerilmez.

Bundan sonra, Azure Cloud Shell hizmetini kullanarak bir güvenlik belirteci oluşturacaksınız.

[!INCLUDE [Open the Azure cloud shell](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-shared-access-signature"></a>Paylaşılan erişim imzası oluşturma

Paylaşılan erişim imzası (SAS), Blob depolama alanına gönderilen isteklerini yetkilendirmek için tarayıcıda çalışan kod tarafından kullanılır. İstemci, SAS kullanarak hesap erişim anahtarı veya bağlantı dizesi kullanmadan depolama kaynaklarına erişimi yetkilendirebilir. SAS hakkında daha fazla bilgi edinmek için bkz. [Paylaşılan erişim imzaları (SAS) kullanma](../common/storage-sas-overview.md).

Azure bulut uyşu üzerinden veya Azure portalı veya Azure Depolama Gezgini ile Azure CLI'yi kullanarak bir SAS oluşturabilirsiniz. Aşağıdaki tabloda CLI ile bir SAS oluşturmak için değerler sağlamanız gereken parametreler açıklanmaktadır.

| Parametre      |Açıklama  | Yer tutucu |
|----------------|-------------|-------------|
| *expiry*       | Erişim belirtecinin YYYY-AA-GG biçimindeki sona erme tarihi. Bu hızlı başlangıçta kullanmak için yarının tarihini girin. | *FUTURE_DATE* |
| *hesap adı* | Depolama hesabı adı. Daha önceki bir adımda ayrılan adı kullanın. | *YOUR_STORAGE_ACCOUNT_NAME* |
| *account-key*  | Depolama hesabı anahtarı. Daha önceki bir adımda ayrılan anahtarı kullanın. | *YOUR_STORAGE_ACCOUNT_KEY* |

JavaScript kodunuzda kullanabileceğiniz bir SAS oluşturmak için her yer tutucu için gerçek değerlerle birlikte aşağıdaki CLI komutunu kullanın.

```azurecli-interactive
az storage account generate-sas \
  --permissions racwdl \
  --resource-types sco \
  --services b \
  --expiry FUTURE_DATE \
  --account-name YOUR_STORAGE_ACCOUNT_NAME \
  --account-key YOUR_STORAGE_ACCOUNT_KEY
```

Her parametreden sonraki değer serisini biraz şifreli bulabilirsiniz. Bu parametre değerleri, kendi ilgili izninin ilk harfinden alınır. Aşağıdaki tabloda değerlerin nereden geldiği açıklanmıştır:

| Parametre        | Değer   | Açıklama  |
|------------------|---------|---------|
| *Izin*    | racwdl  | Bu SAS *read*, *append*, *create*, *write*, *delete* ve *list* özelliklerine izin verir. |
| *resource-types* | sco     | SAS’den etkilenen kaynaklar *service*, *container* ve *object* kaynaklarıdır. |
| *Hizmetleri*       | b       | SAS’den etkilenen hizmet *blob* hizmetidir. |

Artık SAS oluşturulduğuna göre, iade değerini kopyalayın ve gelecek adımda kullanılmak üzere bir yere kaydedin. SAS'ınızı Azure CLI dışında bir yöntem kullanarak oluşturduysanız, varsa `?` ilk harfini kaldırmanız gerekir. Bu karakter, SAS'ın kullanıldığı bu konuda daha sonra URL şablonunda zaten sağlanan bir URL ayırıcısıdır.

> [!IMPORTANT]
> Üretimde her zaman SSL kullanarak SAS belirteçlerini geçirin. Ayrıca, SAS belirteçleri sunucu üzerinde oluşturulmalı ve Azure Blob Depolama’ya geri geçirmek için HTML sayfasına gönderilmelidir. Düşünebileceğiniz bir yaklaşım, SAS belirteçleri oluşturmak için bir sunucusuz bir işlev kullanmaktır. Azure Portal, bir JavaScript işlevi ile SAS oluşturma özelliğine sahip işlev şablonları içerir.

## <a name="implement-the-html-page"></a>HTML sayfasını uygulama

Bu bölümde, temel bir web sayfası oluşturacak ve sayfayı başlatmak ve hata ayıklamak için VS Kodunu yapılandıracaksınız. Ancak, başlatmadan önce, yerel bir web sunucusu başlatmak ve tarayıcınız istediğinde sayfaya hizmet etmek için Node.js kullanmanız gerekir. Ardından, çeşitli blob depolama API'lerini aramak ve sonuçları sayfada görüntülemek için JavaScript kodu eklersiniz. Bu aramaların sonuçlarını [Azure portalında](https://portal.azure.com), [Azure Depolama Gezgini'nde](https://azure.microsoft.com/features/storage-explorer)ve VS Kodu için [Azure Depolama uzantısında](vscode:extension/ms-azuretools.vscode-azurestorage) da görebilirsiniz.

### <a name="set-up-the-web-application"></a>Web uygulamasını ayarlama

İlk olarak, *azure-blobs-javascript* adında yeni bir klasör oluşturun ve VS Code'da açın. Ardından VS Code'da yeni bir dosya oluşturun, aşağıdaki HTML'yi ekleyin ve *azure-blobs-javascript* klasörüne *index.html* olarak kaydedin.

```html
<!DOCTYPE html>
<html>

<body>
    <button id="create-container-button">Create container</button>
    <button id="delete-container-button">Delete container</button>
    <button id="select-button">Select and upload files</button>
    <input type="file" id="file-input" multiple style="display: none;" />
    <button id="list-button">List files</button>
    <button id="delete-button">Delete selected files</button>
    <p><b>Status:</b></p>
    <p id="status" style="height:160px; width: 593px; overflow: scroll;" />
    <p><b>Files:</b></p>
    <select id="file-list" multiple style="height:222px; width: 593px; overflow: scroll;" />
</body>

<!-- You'll add code here later in this quickstart. -->

</html>
```

### <a name="configure-the-debugger"></a>Hata ayıklamayı yapılandırma

VS Code'da hata ayıklama uzantısını ayarlamak için Hata **Ayıklama > Yapılandırma Ekle'yi seçin,** ardından Önkoşullar bölümüne daha önce yüklediğiniz uzantıya bağlı olarak **Chrome** veya **Edge'i**seçin. Bu eylem bir *launch.json* dosyası oluşturur ve düzenleyicide açar.

Ardından, *launch.json* dosyasını, `url` değerin `/index.html` gösterildiği gibi içer."

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:8080/index.html",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

Bu yapılandırma VS Code'a hangi tarayıcının hangi tarayıcıyı ve hangi URL'nin yüklenmesini söyler.

### <a name="launch-the-web-server"></a>Web sunucusunu başlatın

Yerel Node.js web sunucusunu başlatmak için, VS Code içinde bir konsol penceresi açmak için **> Terminalini Görüntüle'yi** seçin ve ardından aşağıdaki komutu girin.

```console
npx http-server
```

Bu *komut, http-sunucu* paketini yükler ve sunucuyu başlatarak geçerli klasörü önceki adımda belirtilenler de dahil olmak üzere varsayılan URL'ler aracılığıyla kullanılabilir hale getirir.

### <a name="start-debugging"></a>Hata ayıklama yı başlatma

*INDEX.html'i* tarayıcıda VS Kodu hata ayıklayıcısı ekli başlatmak için Hata **Ayıklama'yı seçin > Hata Ayıklama'yı başlatın** veya VS Kodu'nda F5 tuşuna basın.

Görüntülenen UI henüz bir şey yapmaz, ancak gösterilen her işlevi uygulamak için aşağıdaki bölümde JavaScript kodu eklersiniz. Daha sonra kesme noktaları ayarlayabilir ve kodunuzda duraklatıldığunda hata ayıklayıcıyla etkileşimkurabilirsiniz.

*index.html'de*değişiklik yaptığınızda, tarayıcıdaki değişiklikleri görmek için sayfayı yeniden yüklediğinizden emin olun. VS Code'da Hata **Ayıklama >** Yeniden Başlatma'yı seçebilir veya CTRL + SHIFT + F5 tuşuna basabilirsiniz.

### <a name="add-the-blob-storage-client-library"></a>Blob depolama istemcisi kitaplığını ekleme

Blob depolama API'sine yapılan aramaları etkinleştirmek için, önce [JavaScript için Azure Depolama SDK'sını indirin - Blob istemci kitaplığı,](https://aka.ms/downloadazurestoragejsblob)zip içeriğini ayıklayın ve *azure-blobs-javascript* klasörüne *azure-storage-blob.js* dosyasını yerleştirin.

Ardından, yer tutucu yorumunu değiştirerek kapanış `</body>` etiketinden sonra aşağıdaki HTML'yi *index.html'e* yapıştırın.

```html
<script src="azure-storage-blob.js" charset="utf-8"></script>

<script>
// You'll add code here in the following sections.
</script>
```

Bu kod komut dosyası dosyasına bir başvuru ekler ve kendi JavaScript kodunuz için bir yer sağlar. Bu hızlı başlatma amacıyla, *AZURE-storage-blob.js* komut dosyası dosyasını kullanarak VS Code'da açabilmeniz, içeriğini okuyabilmeniz ve kesme noktaları ayarlayabilmeniz için. Üretimde, zip dosyasında da sağlanan daha kompakt *azure-storage.blob.min.js* dosyasını kullanmalısınız.

[Başvuru belgelerinde](https://docs.microsoft.com/javascript/api/%40azure/storage-blob/index)her blob depolama işlevi hakkında daha fazla bilgi bulabilirsiniz. SDK'daki bazı işlevlerin yalnızca Düğüm.js'de veya yalnızca tarayıcıda kullanılabildiğini unutmayın.

*Azure-storage-blob.js'deki* kod, blob `azblob`depolama API'lerine erişmek için JavaScript kodunuzda kullanacağınız genel bir değişkeni dışa vuruyor.

### <a name="add-the-initial-javascript-code"></a>İlk JavaScript kodunu ekleme

Ardından, yer tutucu yorumunu `<script>` değiştirerek aşağıdaki kodu önceki kod bloğunda gösterilen öğeye yapıştırın.

```javascript
const createContainerButton = document.getElementById("create-container-button");
const deleteContainerButton = document.getElementById("delete-container-button");
const selectButton = document.getElementById("select-button");
const fileInput = document.getElementById("file-input");
const listButton = document.getElementById("list-button");
const deleteButton = document.getElementById("delete-button");
const status = document.getElementById("status");
const fileList = document.getElementById("file-list");

const reportStatus = message => {
    status.innerHTML += `${message}<br/>`;
    status.scrollTop = status.scrollHeight;
}
```

Bu kod, aşağıdaki kodun kullanacağı her HTML öğesi için `reportStatus` alanlar oluşturur ve çıktıyı görüntülemek için bir işlev uygular.

Aşağıdaki bölümlerde, önceki bloktan sonra javascript kodunun her yeni bloğunu ekleyin.

### <a name="add-your-storage-account-info"></a>Depolama hesabı bilgilerinizi ekleme

Ardından, yer tutucularını hesap adınız ve önceki adımda oluşturduğunuz SAS ile değiştirerek depolama hesabınıza erişmek için kod ekleyin.

```javascript
const accountName = "<Add your storage account name>";
const sasString = "<Add the SAS you generated earlier>";
const containerName = "testcontainer";
const containerURL = new azblob.ContainerURL(
    `https://${accountName}.blob.core.windows.net/${containerName}?${sasString}`,
    azblob.StorageURL.newPipeline(new azblob.AnonymousCredential));
```

Bu kod, bir depolama kapsayıcısı oluşturmak ve işlemek için yararlı bir [ContainerURL](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL) örneği oluşturmak için hesap bilgilerinizi ve SAS'ı kullanır.

### <a name="create-and-delete-a-storage-container"></a>Depolama kapsayıcısı oluşturma ve silme

Ardından, ilgili düğmeye bastığınızda depolama kapsayıcısını oluşturmak ve silmek için kod ekleyin.

```javascript
const createContainer = async () => {
    try {
        reportStatus(`Creating container "${containerName}"...`);
        await containerURL.create(azblob.Aborter.none);
        reportStatus(`Done.`);
    } catch (error) {
        reportStatus(error.body.message);
    }
};

const deleteContainer = async () => {
    try {
        reportStatus(`Deleting container "${containerName}"...`);
        await containerURL.delete(azblob.Aborter.none);
        reportStatus(`Done.`);
    } catch (error) {
        reportStatus(error.body.message);
    }
};

createContainerButton.addEventListener("click", createContainer);
deleteContainerButton.addEventListener("click", deleteContainer);
```

Bu kod, [Bir İptal](https://docs.microsoft.com/javascript/api/@azure/storage-blob/aborter) örneği kullanmadan ContainerURL [oluşturma](https://docs.microsoft.com/javascript/api/@azure/storage-blob/containerclient#create-containercreateoptions-) ve [silme](https://docs.microsoft.com/javascript/api/@azure/storage-blob/containerclient#delete-containerdeletemethodoptions-) işlevlerini çağırır. Bu hızlı başlangıç için işleri basit tutmak için, bu kod depolama hesabınızın oluşturulduğunu ve etkinleştirildiğini varsayar. Üretim kodunda, zaman araişlevselliği eklemek için bir İptal örneği kullanın.

### <a name="list-blobs"></a>Blobları listeleme

Ardından, **Liste dosyaları** düğmesine bastığında depolama kapsayıcısının içeriğini listelemek için kod ekleyin.

```javascript
const listFiles = async () => {
    fileList.size = 0;
    fileList.innerHTML = "";
    try {
        reportStatus("Retrieving file list...");
        let marker = undefined;
        do {
            const listBlobsResponse = await containerURL.listBlobFlatSegment(
                azblob.Aborter.none, marker);
            marker = listBlobsResponse.nextMarker;
            const items = listBlobsResponse.segment.blobItems;
            for (const blob of items) {
                fileList.size += 1;
                fileList.innerHTML += `<option>${blob.name}</option>`;
            }
        } while (marker);
        if (fileList.size > 0) {
            reportStatus("Done.");
        } else {
            reportStatus("The container does not contain any files.");
        }
    } catch (error) {
        reportStatus(error.body.message);
    }
};

listButton.addEventListener("click", listFiles);
```

Bu kod, tüm segmentlerin alınmasını sağlamak için [ContainerURL.listBlobFlatSegment](https://docs.microsoft.com/javascript/api/@azure/storage-blob/containerclient#listblobsflat-containerlistblobsoptions-) işlevini bir döngü içinde çağırır. Her kesim için, içerdiği blob öğeleri listesi üzerinde döngüler ve **Dosyalar** listesini güncelleştirir.

### <a name="upload-blobs"></a>Blobs yükleyin

Ardından, **Dosyaları Seç düğmesine** bastığında dosya yı yüklemek için kod ekleyin.

```javascript
const uploadFiles = async () => {
    try {
        reportStatus("Uploading files...");
        const promises = [];
        for (const file of fileInput.files) {
            const blockBlobURL = azblob.BlockBlobURL.fromContainerURL(containerURL, file.name);
            promises.push(azblob.uploadBrowserDataToBlockBlob(
                azblob.Aborter.none, file, blockBlobURL));
        }
        await Promise.all(promises);
        reportStatus("Done.");
        listFiles();
    } catch (error) {
        reportStatus(error.body.message);
    }
}

selectButton.addEventListener("click", () => fileInput.click());
fileInput.addEventListener("change", uploadFiles);
```

Bu kod, **Dosyaları Seç ve yükle** `file-input` düğmesini gizli öğeye bağlar. Bu şekilde, düğme `click` olayı dosya giriş `click` olayını tetikler ve dosya seçiciyi görüntüler. Dosyaları seçtikten ve iletişim kutusunu kapattıktan `input` sonra, `uploadFiles` olay oluşur ve işlev çağrılır. Bu işlev seçtiğiniz her dosya için yalnızca tarayıcıya özel [uploadBrowserDataToBlockBlob](https://docs.microsoft.com/javascript/api/@azure/storage-blob/blockblobclient#uploadbrowserdata-blob---arraybuffer---arraybufferview--blockblobparalleluploadoptions-) işlevini çağırır. Her arama, dosyaların aynı anda beklenilebilmeleri için listeye eklenen bir Söz döndürür ve dosyaların paralel olarak yüklenmesine neden olur.

### <a name="delete-blobs"></a>Blob’ları silme

Ardından, **seçili dosyaları sil** düğmesine bastığında dosyaları depolama kapsayıcısından silmek için kod ekleyin.

```javascript
const deleteFiles = async () => {
    try {
        if (fileList.selectedOptions.length > 0) {
            reportStatus("Deleting files...");
            for (const option of fileList.selectedOptions) {
                const blobURL = azblob.BlobURL.fromContainerURL(containerURL, option.text);
                await blobURL.delete(azblob.Aborter.none);
            }
            reportStatus("Done.");
            listFiles();
        } else {
            reportStatus("No files selected.");
        }
    } catch (error) {
        reportStatus(error.body.message);
    }
};

deleteButton.addEventListener("click", deleteFiles);
```

Bu kod, listede seçilen her dosyayı kaldırmak için [BlobURL.delete](https://docs.microsoft.com/javascript/api/@azure/storage-blob/BlobURL#delete-aborter--iblobdeleteoptions-) işlevini çağırır. Daha `listFiles` **sonra, Dosyalar** listesinin içeriğini yenilemek için daha önce gösterilen işlevi çağırır.

### <a name="run-and-test-the-web-application"></a>Web uygulamasını çalıştırın ve test edin

Bu noktada, sayfayı başlatabilir ve blob depolamanın nasıl çalıştığını öğrenmek için deneme yapabilirsiniz. Herhangi bir hata oluşursa (örneğin, kapsayıcıyı oluşturmadan önce dosyaları listelemeye çalıştığınızda), **Durum** bölmesi alınan hata iletisini görüntüler. Depolama API'leri tarafından döndürülen değerleri incelemek için JavaScript kodunda kesme noktaları da ayarlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç sırasında oluşturulan kaynakları temizlemek için [Azure portalına](https://portal.azure.com) gidin ve Önkoşullar bölümünde oluşturduğunuz kaynak grubunu silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, tarayıcı tabanlı JavaScript'ten blob depolama alanına erişen basit bir web sitesi oluşturdunuz. Bir web sitesini blob depolamada nasıl barındırabileceğinizi öğrenmek için aşağıdaki öğreticiye devam edin:

> [!div class="nextstepaction"]
> [Blob Depolama'da statik bir web sitesi barındırma](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host)
