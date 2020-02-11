---
title: Azure depolama ile klasik Azure CLı 'yi kullanma | Microsoft Docs
description: Azure depolama ile Azure Storage komut satırı arabirimini (CLı) kullanarak depolama hesapları oluşturma ve yönetme ve Azure Blob 'ları ile dosyalarla çalışma hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/30/2017
ms.author: tamram
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: 6554385a879b054153dcb808c3dff4b60c136458
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77120848"
---
# <a name="using-the-azure-classic-cli-with-azure-storage"></a>Azure depolama ile klasik Azure CLı kullanma

## <a name="overview"></a>Genel Bakış

Azure klasik CLı, Azure platformuyla çalışmaya yönelik bir dizi açık kaynak ve platformlar arası komut sunar. Bu, [Azure Portal](https://portal.azure.com) ve zengin veri erişimi işlevselliğinde bulunan aynı işlevselliğin çoğunu sağlar.

Bu kılavuzda, Azure depolama ile çeşitli geliştirme ve yönetim görevlerini gerçekleştirmek için [Azure klasık CLI](../../cli-install-nodejs.md) 'yi nasıl kullanacağınızı keşfedeceğiz. Bu kılavuzu kullanmadan önce, en son klasik CLı 'yi indirip yüklemenizi ve yükseltmenizi öneririz.

Bu kılavuzda, Azure depolama 'nın temel kavramlarını anladığınızı varsaymaktadır. Kılavuz, Azure depolama ile klasik CLı kullanımını göstermek için bir dizi betik sağlar. Her bir betiği çalıştırmadan önce, yapılandırmanıza göre betik değişkenlerini güncelleştirdiğinizden emin olun.

> [!NOTE]
> Kılavuz, klasik depolama hesapları için Azure klasik CLı komutu ve betik örnekleri sağlar. Kaynak Yöneticisi depolama hesapları için Azure klasik CLı komutları için Azure [kaynak yönetimi Ile Mac, Linux ve Windows Için Azure CLI 'Yı kullanma](../../virtual-machines/azure-cli-arm-commands.md#storage-objects) konusuna bakın.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-classic-cli-in-5-minutes"></a>Azure depolama 'yı ve Azure klasik CLı 'yı 5 dakika içinde kullanmaya başlayın
Bu kılavuz örnekleri için Ubuntu kullanır, ancak diğer işletim sistemi platformları benzer şekilde gerçekleştirmelidir.

**Azure 'Da yeni:** Microsoft Azure aboneliği ve bu abonelikle ilişkili bir Microsoft hesabı alın. Azure satın alma seçenekleri hakkında bilgi için bkz. [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/), [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/)ve [üye teklifleri](https://azure.microsoft.com/pricing/member-offers/) (MSDN, Microsoft iş ortağı ağı ve BizSpark üyeleri ve diğer Microsoft programları için).

Azure abonelikleri hakkında daha fazla bilgi için bkz. [Azure Active Directory (Azure AD) içinde yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) .

**Microsoft Azure aboneliği ve hesabı oluşturduktan sonra:**

1. Klasik Azure [CLI 'Yı yükleme](../../cli-install-nodejs.md)bölümünde özetlenen yönergeleri izleyerek Azure klasik CLI 'yı indirip yükleyin.
2. Klasik CLı yüklendikten sonra, klasik CLı komutlarına erişmek için komut satırı arabiriminizdeki (Bash, Terminal, komut istemi) Azure komutunu kullanabilirsiniz. _Azure_ komutunu yazın ve aşağıdaki çıktıyı görmeniz gerekir.

    ![Azure komut çıktısı](./media/storage-azure-cli/azure_command.png)   
3. Komut satırı arabiriminde, tüm Azure depolama komutlarının listesini listelemek için `azure storage` yazın ve klasik CLı 'nın sağladığı işlevselliklerin ilk bir izlenimini alın. Komut sözdiziminin ayrıntılarını görmek için **-h** parametresi ile komut adı yazabilirsiniz (örneğin, `azure storage share create -h`).
4. Şimdi, Azure depolama 'ya erişmek için temel klasik CLı komutlarını gösteren basit bir betik sunacağız. Betik, ilk olarak depolama hesabınız ve anahtarınız için iki değişken ayarlamanızı ister. Ardından, komut dosyası bu yeni depolama hesabında yeni bir kapsayıcı oluşturur ve var olan bir görüntü dosyasını (blob) bu kapsayıcıya yükler. Betik, bu kapsayıcıdaki tüm Blobları listeledikten sonra, görüntü dosyasını yerel bilgisayarda var olan hedef dizine indirir.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. Yerel bilgisayarınızda, tercih ettiğiniz metin düzenleyicisini açın (örneğin, VIM). Yukarıdaki betiği metin düzenleyicinize yazın.
6. Şimdi, komut dosyası değişkenlerini yapılandırma ayarlarınıza göre güncelleştirmeniz gerekir.

   * **< storage_account_name >** Betikte verilen adı kullanın veya depolama hesabınız için yeni bir ad girin. **Önemli:** Depolama hesabının adı Azure 'da benzersiz olmalıdır. Küçük harf olmalıdır!
   * **< storage_account_key >** Depolama hesabınızın erişim anahtarı.
   * **< container_name >** Betikte verilen adı kullanın veya Kapsayıcınız için yeni bir ad girin.
   * **< image_to_upload >** Yerel bilgisayarınızda bir resmin yolunu girin, örneğin: "~/Images/HelloWorld.exe".
   * **< destination_folder >** Azure depolama 'dan indirilen dosyaları depolamak için bir yerel dizinin yolunu girin, örneğin: "~/downloadImages".
7. VIM 'de gerekli değişkenleri güncelleştirdikten sonra, komut dosyasını kaydetmek için `ESC`, `:`, `wq!` tuş kombinasyonlarına basın.
8. Bu betiği çalıştırmak için, komut dosyası adını bash konsoluna yazmanız yeterlidir. Bu komut dosyası çalıştıktan sonra, indirilen görüntü dosyasını içeren bir yerel hedef klasörünüz olmalıdır. Aşağıdaki ekran görüntüsünde bir örnek çıktı gösterilmektedir:

Betiği çalıştıktan sonra, indirilen görüntü dosyasını içeren bir yerel hedef klasörünüz olmalıdır.

## <a name="manage-storage-accounts-with-the-azure-classic-cli"></a>Azure klasik CLı ile depolama hesaplarını yönetme
### <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma
Depolama komutlarının birçoğu bir Azure aboneliği olmadan çalışırken, klasik CLı 'dan aboneliğinize bağlanmanızı öneririz.

### <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma
Azure Storage 'ı kullanmak için bir depolama hesabına ihtiyacınız olacaktır. Bilgisayarınızı aboneliğinize bağlanacak şekilde yapılandırdıktan sonra, yeni bir Azure depolama hesabı oluşturabilirsiniz.

```azurecli
azure storage account create <account_name>
```

Depolama hesabınızın adı 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakam ve küçük harf kullanılmalıdır.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Ortam değişkenlerinde varsayılan bir Azure depolama hesabı ayarlama
Aboneliğinizde birden çok depolama hesabınız olabilir. Bunlardan birini seçebilir ve aynı oturumdaki tüm depolama komutlarının ortam değişkenlerinde ayarlayabilirsiniz. Bu, depolama hesabını ve anahtarı açıkça belirtmeden klasik CLı depolama komutlarını çalıştırmanızı sağlar.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Varsayılan depolama hesabı ayarlamak için başka bir yol bağlantı dizesi kullanmaktır. Şu komutla bağlantı dizesini al:

```azurecli
azure storage account connectionstring show <account_name>
```

Ardından çıkış bağlantı dizesini kopyalayın ve ortam değişkenine ayarlayın:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Blobları oluşturma ve yönetme
Azure Blob depolama, HTTP veya HTTPS aracılığıyla dünyanın her yerinden erişilebilen metin veya ikili veriler gibi büyük miktarda yapılandırılmamış veriyi depolamaya yönelik bir hizmettir. Bu bölümde, Azure Blob depolama kavramlarını zaten bildiğiniz varsayılmaktadır. Ayrıntılı bilgi için bkz. .NET ve [BLOB hizmeti kavramlarını](https://msdn.microsoft.com/library/azure/dd179376.aspx) [kullanarak Azure Blob depolamayı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) .

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Azure Storage 'daki her blob bir kapsayıcıda olmalıdır. `azure storage container create` komutunu kullanarak özel bir kapsayıcı oluşturabilirsiniz:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Üç düzeyde anonim okuma erişimi vardır: **kapalı**, **BLOB**ve **kapsayıcı**. Bloblara anonim erişimi engellemek için Izin parametresini **kapalı**olarak ayarlayın. Varsayılan olarak, yeni kapsayıcı özeldir ve yalnızca hesap sahibi tarafından erişilebilir. Blob kaynaklarına anonim genel okuma erişimi sağlamak, ancak kapsayıcı meta verileri veya kapsayıcıdaki Blobların listesi için izin parametresini **BLOB**olarak ayarlayın. Blob kaynaklarına, kapsayıcı meta verilerine ve kapsayıcıdaki Blobların listesine tam genel okuma erişimi sağlamak için, Izin parametresini **kapsayıcı**olarak ayarlayın. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Azure Blob Storage blok blobları ve sayfa bloblarını destekler. Daha fazla bilgi için bkz. [blok bloblarını, ekleme bloblarını ve sayfa Bloblarını anlama](https://msdn.microsoft.com/library/azure/ee691964.aspx).

İçindeki Blobları bir kapsayıcıya yüklemek için `azure storage blob upload`kullanabilirsiniz. Varsayılan olarak, bu komut yerel dosyaları bir blok blobuna yükler. Blobun türünü belirtmek için `--blobtype` parametresini kullanabilirsiniz.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Bloblardan blob indirme
Aşağıdaki örnek, bir kapsayıcıdan Blobların nasıl indirileceğini gösterir.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Blobları kopyalama
Blobları, depolama hesaplarıyla bölgeler içinde veya bunların arasında zaman uyumsuz olarak kopyalayabilirsiniz.

Aşağıdaki örnekte blobların bir depolama hesabından diğerine nasıl kopyalandığı gösterilmektedir. Bu örnekte, Blobların herkese açık, anonim olarak erişilebilen bir kapsayıcı oluşturuyoruz.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Bu örnek, zaman uyumsuz bir kopya gerçekleştirir. `azure storage blob copy show` işlemini çalıştırarak, her kopyalama işleminin durumunu izleyebilirsiniz.

Kopyalama işlemi için girilen kaynak URL 'sinin herkese açık ya da bir SAS (paylaşılan erişim imzası) belirteci içermesini gerektiğini unutmayın.

### <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için aşağıdaki komutu kullanın:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Dosya paylaşımları oluşturma ve yönetme
Azure dosyaları, standart SMB protokolünü kullanan uygulamalar için paylaşılan depolama alanı sunar. Sanal makineler ve bulut hizmetlerinin yanı sıra şirket içi uygulamalar Microsoft Azure, bağlı paylaşımlar aracılığıyla dosya verilerini paylaşabilir. Dosya paylaşımlarını ve dosya verilerini, klasik CLı aracılığıyla yönetebilirsiniz. Azure dosyaları hakkında daha fazla bilgi için bkz. [Azure dosyaları 'Na giriş](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Azure dosya paylaşma, Azure 'da bir SMB dosya paylaşımıdır. Tüm dizinlerin ve dosyaların bir dosya paylaşımında oluşturulması gerekir. Bir hesap sınırsız sayıda paylaşım içerebilir ve bir paylaşım, depolama hesabının kapasite sınırlarına kadar sınırsız sayıda dosyayı depolayabilirler. Aşağıdaki örnek **myshare**adlı bir dosya paylaşımının oluşturur.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Dizin oluşturma
Bir dizin, Azure dosya paylaşımının isteğe bağlı hiyerarşik yapısını sağlar. Aşağıdaki örnek dosya paylaşımında **mydir** adlı bir dizin oluşturur.

```azurecli
azure storage directory create myshare myDir
```

Dizin yolunun birden çok düzey ( *ör.* **a/b**) içerebileceğini unutmayın. Ancak, tüm üst dizinlerin var olduğundan emin olmanız gerekir. Örneğin, **a/b**yolu için **, ilk olarak** dizin oluşturmanız ve ardından **b**dizinini oluşturmanız gerekir.

### <a name="upload-a-local-file-to-directory"></a>Dizine yerel bir dosya yükleme
Aşağıdaki örnek, **~/Temp/sampledosya.txt** öğesinden **mydir** dizinine bir dosya yükler. Dosya yolunu yerel makinenizde geçerli bir dosyaya işaret eden şekilde düzenleyin:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Paylaşımdaki bir dosyanın boyutu 1 TB 'a kadar olabileceğini unutmayın.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Paylaşılan kök veya dizindeki dosyaları listeleyin
Aşağıdaki komutu kullanarak bir Share kökündeki veya dizindeki dosyaları ve alt dizinleri listeleyebilirsiniz:

```azurecli
azure storage file list myshare myDir
```

Dizin adının listeleme işlemi için isteğe bağlı olduğunu unutmayın. Atlanırsa, komut paylaşımın kök dizininin içeriğini listeler.

### <a name="copy-files"></a>Dosyaları kopyalama
Klasik CLı sürüm 0.9.8 başlayarak, bir dosyayı başka bir dosyaya, bir bloba bir dosyaya veya bir blob dosyasına kopyalayabilirsiniz. Aşağıda CLı komutlarını kullanarak bu kopyalama işlemlerinin nasıl gerçekleştirileceği gösterilmektedir. Yeni dizine bir dosya kopyalamak için:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

Bir blobu bir dosya dizinine kopyalamak için:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Sonraki Adımlar

Depolama kaynaklarıyla çalışmak için Azure klasik CLı komut başvurusunu burada bulabilirsiniz:

* [Kaynak Yöneticisi modunda Azure klasik CLı komutları](../../virtual-machines/azure-cli-arm-commands.md#storage-objects)
* [Azure hizmet yönetimi modunda klasik Azure CLı komutları](../../cli-install-nodejs.md)

[Azure CLI](../storage-azure-cli.md)'nin en son sürümünü Kaynak Yöneticisi dağıtım modeliyle kullanmak üzere denemek de isteyebilirsiniz.
