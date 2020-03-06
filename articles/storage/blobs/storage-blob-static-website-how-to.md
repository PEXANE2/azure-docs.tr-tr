---
title: Azure depolama 'da statik bir Web sitesi barındırma
description: Azure Storage GPv2 hesabındaki bir kapsayıcıdan doğrudan statik içerik (HTML, CSS, JavaScript ve resim dosyaları) sunma hakkında bilgi edinin.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.author: normesta
ms.date: 03/04/2020
ms.openlocfilehash: e312cc0dc6c58bb33a737e1fc28dd6eb3578b764
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78330302"
---
# <a name="host-a-static-website-in-azure-storage"></a>Azure depolama 'da statik bir Web sitesi barındırma

Doğrudan bir Azure Storage GPv2 hesabındaki bir kapsayıcıdan statik içerik (HTML, CSS, JavaScript ve resim dosyaları) sunabilir. Daha fazla bilgi için bkz. [Azure Storage 'Da statik Web sitesi barındırma](storage-blob-static-website.md).

Bu makalede Azure portal, Azure CLı veya PowerShell kullanarak statik Web sitesi barındırmayı nasıl etkinleştireceğinizi gösterilmektedir.

## <a name="enable-static-website-hosting"></a>Statik Web sitesi barındırmayı etkinleştir

Statik Web sitesi barındırma, depolama hesabında etkinleştirmeniz gereken bir özelliktir.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Başlamak için [Azure Portal](https://portal.azure.com/) oturum açın.

2. Depolama hesabınızı bulun ve hesabına genel bakış görüntüler.

3. Statik Web siteleri için yapılandırma sayfasını göstermek üzere **statik Web sitesi** ' ni seçin.

4. Depolama hesabı için statik Web sitesi barındırmayı etkinleştirmek üzere **etkin** ' i seçin.

5. **Dizin belgesi adı** alanında, bir varsayılan dizin sayfası (örneğin: *Index. html*) belirtin. 

   Bir kullanıcı, statik Web sitesi köküne gittiğinde varsayılan dizin sayfası görüntülenir.  

6. **Hata belgesi yolu** alanında varsayılan bir hata sayfası (örneğin: *404. html*) belirtin. 

   Statik Web sitenize var olmayan bir sayfaya gitmek bir kullanıcı çalıştığında varsayılan hata sayfası görüntülenir.

7. **Kaydet** düğmesine tıklayın. Azure portalında statik Web sitesi uç noktanızı artık görüntüler. 

    ![Bir depolama hesabı için statik Web sitesi barındırma etkinleştir](media/storage-blob-static-website-host/enable-static-website-hosting.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

<a id="cli" />

[Azure komut satırı arabirimi 'ni (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak statik Web sitesi barındırmayı etkinleştirebilirsiniz.

1. İlk olarak, [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)açın veya Azure CLI 'yı yerel olarak [yüklediyseniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) , Windows PowerShell gibi bir komut konsol uygulaması açın.

2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik Web sitenizi barındıracak depolama hesabının aboneliğine ayarlayın.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   `<subscription-id>` yer tutucu değerini aboneliğinizin KIMLIĞIYLE değiştirin.

3. Statik Web sitesi barındırmayı etkinleştirin.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * `<storage-account-name>` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

   * Bir tarayıcı, sitenizde olmayan bir sayfa istediğinde, `<error-document-name>` yer tutucusunu kullanıcılara görüntülenecek hata belgesi adıyla değiştirin.

   * `<index-document-name>` yer tutucusunu Dizin belgesi adıyla değiştirin. Bu belge genellikle "index. html" dir.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

<a id="powershell" />

Azure PowerShell modülünü kullanarak statik Web sitesi barındırmayı etkinleştirebilirsiniz.

1. Bir Windows PowerShell komut penceresi açın.

2. Azure PowerShell modülünüzün az 0,7 veya sonraki bir sürümü olduğunu doğrulayın.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

3. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

   ```powershell
   Connect-AzAccount
   ```

4. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik Web sitenizi barındıracak depolama hesabının aboneliğine ayarlayın.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   `<subscription-id>` yer tutucu değerini aboneliğinizin KIMLIĞIYLE değiştirin.

5. Kullanmak istediğiniz depolama hesabını tanımlayan depolama hesabı bağlamını alın.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * `<resource-group-name>` yer tutucu değerini kaynak grubunuzun adıyla değiştirin.

   * `<storage-account-name>` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

6. Statik Web sitesi barındırmayı etkinleştirin.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Bir tarayıcı, sitenizde olmayan bir sayfa istediğinde, `<error-document-name>` yer tutucusunu kullanıcılara görüntülenecek hata belgesi adıyla değiştirin.

   * `<index-document-name>` yer tutucusunu Dizin belgesi adıyla değiştirin. Bu belge genellikle "index. html" dir.

---

## <a name="upload-files"></a>Dosyaları karşıya yükleme 

### <a name="portal"></a>[Portal](#tab/azure-portal)

Bu yönergeler, Azure portal görüntülenen Depolama Gezgini sürümünü kullanarak dosyaları karşıya yükleme işleminin nasıl yapılacağını gösterir. Ancak, Azure portal dışında çalışan [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) sürümünü de kullanabilirsiniz. Hesabınızın **$Web** kapsayıcısına dosya yükleyebileceğiniz [AzCopy](../common/storage-use-azcopy-v10.md), PowerShell, CLI veya herhangi bir özel uygulama kullanabilirsiniz. Visual Studio Code kullanarak dosyaları karşıya yükleyen adım adım bir öğretici için bkz. [öğretici: blob depolamada statik bir Web sitesi barındırma](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host).

1. **Depolama Gezgini (Önizleme)** öğesini seçin.

2. **BLOB kapsayıcıları** düğümünü genişletin ve **$Web** kapsayıcısını seçin.

3. Dosyaları karşıya yüklemek için **karşıya yükle** düğmesini seçin.

   ![Dosyaları karşıya yükleme](media/storage-blob-static-website/storage-blob-static-website-upload.png)

4. Tarayıcının dosya içeriğini görüntülemesini istiyorsanız, bu dosyanın içerik türünün `text/html`olarak ayarlandığından emin olun. 

   ![İçerik türlerini denetle](media/storage-blob-static-website/storage-blob-static-website-content-type.png)

   >[!NOTE]
   > Depolama Gezgini, `.html`gibi yaygın olarak tanınan uzantılar için bu özelliği otomatik olarak `text/html` olarak ayarlar. Ancak, bazı durumlarda bunu kendiniz ayarlamanız gerekecektir. Bu özelliği `text/html`olarak ayarlamazsanız tarayıcı, kullanıcılardan içeriği işlemek yerine dosyayı indirmesini ister. Bu özelliği ayarlamak için, dosyaya sağ tıklayın ve ardından **Özellikler**' e tıklayın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Nesneleri bir kaynak dizininden *$Web* kapsayıcısına yükleyin.

> [!NOTE]
> Azure Cloud Shell kullanıyorsanız, `$web` kapsayıcısına başvururken bir `\` kaçış karakteri eklediğinizden emin olun (örneğin: `\$web`). Azure CLı 'nın yerel bir yüklemesini kullanıyorsanız kaçış karakterini kullanmanız gerekmez.

Bu örnekte Azure Cloud Shell oturumundan komut çalıştırdığınız varsayılır.

```azurecli-interactive
az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name> --content-type 'text/html; charset=utf-8'
```

* `<storage-account-name>` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

* `<source-path>` yer tutucusunu, karşıya yüklemek istediğiniz dosyaların konumunun bir yolu ile değiştirin.

> [!NOTE]
> Azure CLı 'nin konum yüklemesini kullanıyorsanız, yerel bilgisayarınızdaki herhangi bir konumun yolunu kullanabilirsiniz (örneğin: `C:\myFolder`.
>
> Azure Cloud Shell kullanıyorsanız, Cloud Shell görünür bir dosya paylaşımıyla başvurulmalıdır. Bu konum, bulut paylaşımının kendisi veya Cloud Shell oluşturduğunuz var olan bir dosya paylaşımının dosya paylaşımıdır. Bunu nasıl yapacağınızı öğrenmek için bkz. [Azure Cloud Shell dosyaları kalıcı hale](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage)getirme.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Nesneleri bir kaynak dizininden *$Web* kapsayıcısına yükleyin.

```powershell
# upload a file
set-AzStorageblobcontent -File "<path-to-file>" `
-Properties @{ ContentType = "text/html; charset=utf-8";} `
-Container `$web `
-Blob "<blob-name>" `
-Context $ctx
```

* `<path-to-file>` yer tutucu değerini, yüklemek istediğiniz dosyanın tam yolu ile değiştirin (örneğin: `C:\temp\index.html`).

* `<blob-name>` yer tutucu değerini, sonuçta elde edilen Blobun vermek istediğiniz adla değiştirin (örneğin: `index.html`).

---

## <a name="find-the-website-url-by-using-the-azure-portal"></a>Azure portal kullanarak Web sitesi URL 'sini bulma

Sitenizin sayfalarını bir tarayıcıdan, Web sitesinin genel URL 'sini kullanarak görüntüleyebilirsiniz.

### <a name="portal"></a>[Portal](#tab/azure-portal)

<a id="portal-find-url" />

Depolama hesabınızın firmaya Genel Bakış sayfasının yanında görünen bölmede, **statik Web sitesi**' ni seçin. Sitenizin URL 'SI **birincil uç nokta** alanında görüntülenir.

![Azure depolama statik Web siteleri ölçümleri ölçümü](./media/storage-blob-static-website/storage-blob-static-website-url.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

<a id="cli-find-url" />

Aşağıdaki komutu kullanarak statik Web sitenizin genel URL 'sini bulun:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* `<storage-account-name>` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

* `<resource-group-name>` yer tutucu değerini kaynak grubunuzun adıyla değiştirin.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

<a id="powershell-find-url" />

Aşağıdaki komutu kullanarak, kullanarak statik Web sitenizin genel URL 'sini bulun:

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -Name "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* `<resource-group-name>` yer tutucu değerini kaynak grubunuzun adıyla değiştirin.

* `<storage-account-name>` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

---

<a id="metrics" />

## <a name="enable-metrics-on-static-website-pages"></a>Statik Web sitesi sayfalarında ölçümleri etkinleştirme

Ölçümleri etkinleştirdikten sonra, **$Web** kapsayıcısındaki dosyalardaki trafik istatistikleri ölçüm panosunda raporlanır.

1. **Ayarlar** > **izleme** > **ölçümleri**' ne tıklayın.

   Ölçüm verileri farklı ölçüm API 'Lerine bağlanarak oluşturulur. Portal yalnızca verileri döndüren üyelere odaklanmak için yalnızca belirli bir zaman çerçevesinde kullanılan API üyelerini görüntüler. Gerekli API üyesini seçebildiğinizden emin olmak için ilk adım zaman çerçevesini genişletmelidir.

2. Zaman çerçevesi düğmesine tıklayın ve **son 24 saat** ' i seçin ve ardından **Uygula**' ya tıklayın.

   ![Azure depolama statik Web siteleri ölçümleri zaman aralığı](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. *Ad alanı* açılan listesinden **BLOB** ' u seçin.

   ![Azure depolama statik Web siteleri ölçümleri ad alanı](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. Ardından **Çıkış** ölçümünü seçin.

   ![Azure depolama statik Web siteleri ölçümleri ölçümü](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. *Toplama* seçicideki **Toplam** ' u seçin.

   ![Azure depolama statik Web siteleri ölçümleri toplama](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. **Filtre Ekle** düğmesine tıklayın ve *özellik* seçicideki **API adı** ' nı seçin.

   ![Azure depolama statik Web siteleri ölçümleri API adı](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. Ölçüm raporunu doldurmak için *değerler* seçicisindeki **getwebcontent** seçeneğinin yanındaki kutuyu işaretleyin.

   ![Azure depolama statik Web siteleri ölçümleri GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>Sonraki adımlar

* Statik Web siteniz ile özel bir etki alanı yapılandırmayı öğrenin. Bkz. [özel bir etki alanını Azure Blob depolama uç noktası Ile eşleme](storage-custom-domain-name.md).

