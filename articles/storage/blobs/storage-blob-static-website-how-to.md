---
title: Azure Depolama'da statik bir web sitesi barındırma
description: Statik içeriği (HTML, CSS, JavaScript ve resim dosyaları) doğrudan bir Azure Depolama GPv2 hesabındaki bir kapsayıcıdan nasıl sunmayı öğrenin.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.author: normesta
ms.date: 03/04/2020
ms.openlocfilehash: 056e23f0f0cf1a3a1c70042cef3c92dd41f14f82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247019"
---
# <a name="host-a-static-website-in-azure-storage"></a>Azure Depolama'da statik bir web sitesi barındırma

Statik içeriği (HTML, CSS, JavaScript ve resim dosyaları) doğrudan bir Azure Depolama GPv2 hesabındaki bir kapsayıcıdan hizmet verebilirsiniz. Daha fazla bilgi için [Azure Depolama'da barındırılanan Statik web sitesine](storage-blob-static-website.md)bakın.

Bu makalede, Azure portalı, Azure CLI veya PowerShell kullanarak statik web sitesi barındırma yı nasıl etkinleştirdiğinizi gösterilmektedir.

## <a name="enable-static-website-hosting"></a>Statik web sitesi barındırma yı etkinleştirme

Statik web sitesi barındırma depolama hesabında etkinleştirmek zorunda bir özelliktir.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Başlamak için [Azure portalında](https://portal.azure.com/) oturum açın.

2. Depolama hesabınızı bulun ve hesaba genel bakışı görüntüleyin.

3. Statik web **sitelerinin** yapılandırma sayfasını görüntülemek için Statik web sitesini seçin.

4. Depolama hesabı için statik web sitesi barındırmayı etkinleştirmek için **Etkin'i** seçin.

5. **Dizin belge adı** alanında varsayılan dizin sayfası belirtin (Örneğin: *index.html).* 

   Varsayılan dizin sayfası, bir kullanıcı statik web sitenizin köküne gittiğinde görüntülenir.  

6. Hata **belge yolu** alanında varsayılan hata sayfası belirtin (Örneğin: *404.html).* 

   Varsayılan hata sayfası, kullanıcı statik web sitenizde bulunmayan bir sayfaya gezinmeye çalıştığında görüntülenir.

7. **Kaydet**'e tıklayın. Azure portalı artık statik web sitesi bitiş noktanızı görüntüler. 

    ![Depolama hesabı için statik web sitesi barındırmayı etkinleştirme](media/storage-blob-static-website-host/enable-static-website-hosting.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

<a id="cli" />

[Azure Komut Satırı Arabirimi 'ni (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak statik web sitesi barındırmayı etkinleştirebilirsiniz.

1. İlk olarak, [Azure Bulut Kabuğu'nu](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest)açın veya Azure CLI'yi yerel olarak [yüklediyseniz,](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Windows PowerShell gibi bir komut konsolu uygulamasını açın.

2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik web sitenizi barındıracak depolama hesabı aboneliğine ayarlayın.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   `<subscription-id>` Yer tutucu değerini aboneliğinizin kimliğiyle değiştirin.

3. Statik web sitesi barındırma etkinleştirin.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

   * Bir `<error-document-name>` tarayıcı sitenizde var olmayan bir sayfa istediğinde, yer tutucuyu kullanıcılara görünecek hata belgesinin adı ile değiştirin.

   * Yer `<index-document-name>` tutucuyu dizin belgesinin adı ile değiştirin. Bu belge genellikle "index.html" dir.

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

<a id="powershell" />

Azure PowerShell modüllerini kullanarak statik web sitesi barındırmayı etkinleştirebilirsiniz.

1. Windows PowerShell komut pencereni açın.

2. Azure PowerShell modülü Az sürüm 0.7 veya daha sonra olduğunu doğrulayın.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

3. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

   ```powershell
   Connect-AzAccount
   ```

4. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik web sitenizi barındıracak depolama hesabı aboneliğine ayarlayın.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   `<subscription-id>` Yer tutucu değerini aboneliğinizin kimliğiyle değiştirin.

5. Kullanmak istediğiniz depolama hesabını tanımlayan depolama hesabı bağlamını alın.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * `<resource-group-name>` Yer tutucu değerini kaynak grubunuzun adı ile değiştirin.

   * `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

6. Statik web sitesi barındırma etkinleştirin.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Bir `<error-document-name>` tarayıcı sitenizde var olmayan bir sayfa istediğinde, yer tutucuyu kullanıcılara görünecek hata belgesinin adı ile değiştirin.

   * Yer `<index-document-name>` tutucuyu dizin belgesinin adı ile değiştirin. Bu belge genellikle "index.html" dir.

---

## <a name="upload-files"></a>Dosyaları karşıya yükleme 

### <a name="portal"></a>[Portal](#tab/azure-portal)

Bu yönergeler, Azure portalında görünen Depolama Gezgini sürümünü kullanarak dosyaları nasıl yükleyeceğimiz gösterilmektedir. Ancak, Azure portalının dışında çalışan [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) sürümünü de kullanabilirsiniz. [AzCopy,](../common/storage-use-azcopy-v10.md)PowerShell, CLI veya hesabınızın **$web** kapsayıcıya dosya yükleyebilen herhangi bir özel uygulamayı kullanabilirsiniz. Visual Studio kodunu kullanarak dosya yükleyen adım adım öğretici için [Bkz. Tutorial: Host blob Depolama statik bir web sitesi.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host)

1. **Depolama Gezgini'ni seçin (önizleme)**.

2. **BLOB CONTAINERS** düğümünü genişletin ve **ardından $web** kapsayıcısını seçin.

3. Dosya yüklemek için **Yükle** düğmesini seçin.

   ![Dosyaları karşıya yükleme](media/storage-blob-static-website/storage-blob-static-website-upload.png)

4. Tarayıcının dosya içeriğini görüntülemesini istiyorsanız, o dosyanın içerik türünün `text/html`' olarak ayarlandığınızdan emin olun. 

   ![İçerik türlerini denetleme](media/storage-blob-static-website/storage-blob-static-website-content-type.png)

   >[!NOTE]
   > Depolama Gezgini bu özelliği `text/html` otomatik olarak `.html`. Ancak, bazı durumlarda, bu kendiniz ayarlamak gerekir. Bu özelliği `text/html`ayarlamazsanız, tarayıcı kullanıcıları içeriği işlemek yerine dosyayı karşıdan yüklemeye zorlar. Bu özelliği ayarlamak için dosyayı sağ tıklatın ve ardından **Özellikler'i**tıklatın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Nesneleri kaynak dizinden *$web* kapsayıcısına yükleyin.

> [!NOTE]
> Azure Bulut Su Şurası kullanıyorsanız, `\` `$web` kapsayıcıya atıfta bulunurken bir kaçış `\$web`karakteri eklediğinizden emin olun (Örneğin: ). Azure CLI'nin yerel yüklemesini kullanıyorsanız, kaçış karakterini kullanmanız gerekmeyecek.

Bu örnek, Azure Bulut Bulutu oturumundan komutlar çalıştırdığınızı varsayar.

```azurecli-interactive
az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name> --content-type 'text/html; charset=utf-8'
```

* `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

* Yer `<source-path>` tutucuyu yüklemek istediğiniz dosyaların konumuna giden bir yol ile değiştirin.

> [!NOTE]
> Azure CLI'nin konum yüklemesini kullanıyorsanız, yerel bilgisayarınızdaki herhangi bir konuma giden yolu `C:\myFolder`kullanabilirsiniz (Örneğin: .
>
> Azure Bulut Su Şur'u kullanıyorsanız, Bulut Kabuğu tarafından görülebilen bir dosya paylaşımına başvurmanız gerekir. Bu konum, Bulut paylaşımının dosya payı veya Bulut Bulut Bulutu'ndan monte ettiğiniz varolan bir dosya paylaşımı olabilir. Bunu nasıl yapacağınızı öğrenmek için [Azure Bulut BulutU'ndaki Devam dosyalarına](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage)bakın.

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Nesneleri kaynak dizinden *$web* kapsayıcısına yükleyin.

```powershell
# upload a file
set-AzStorageblobcontent -File "<path-to-file>" `
-Properties @{ ContentType = "text/html; charset=utf-8";} `
-Container `$web `
-Blob "<blob-name>" `
-Context $ctx
```

* `<path-to-file>` Yer tutucu değerini yüklemek istediğiniz dosyaya tam nitelikli yol ile değiştirin (Örneğin: ). `C:\temp\index.html`

* `<blob-name>` Yer tutucu değerini, ortaya çıkan blob'u vermek istediğiniz adla `index.html`değiştirin (Örneğin: ).

---

## <a name="find-the-website-url-by-using-the-azure-portal"></a>Azure portalını kullanarak web sitesinin URL'sini bulun

Web sitesinin genel URL'sini kullanarak sitenizin sayfalarını bir tarayıcıdan görüntüleyebilirsiniz.

### <a name="portal"></a>[Portal](#tab/azure-portal)

<a id="portal-find-url" />

Depolama hesabınızın hesaba genel bakış sayfasının yanında görünen bölmede **Statik Web Sitesi'ni**seçin. Sitenizin URL'si **Birincil bitiş noktası** alanında görünür.

![Azure Depolama statik web siteleri ölçümleri metrik](./media/storage-blob-static-website/storage-blob-static-website-url.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

<a id="cli-find-url" />

Aşağıdaki komutu kullanarak statik web sitenizin genel URL'sini bulun:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

* `<resource-group-name>` Yer tutucu değerini kaynak grubunuzun adı ile değiştirin.

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

<a id="powershell-find-url" />

Aşağıdaki komutu kullanarak statik web sitenizin genel URL'sini bulun:

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -Name "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* `<resource-group-name>` Yer tutucu değerini kaynak grubunuzun adı ile değiştirin.

* `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

---

<a id="metrics" />

## <a name="enable-metrics-on-static-website-pages"></a>Statik web sitesi sayfalarında ölçümleri etkinleştirme

Ölçümleri etkinleştirdikten sonra, **$web** kapsayıcıdaki dosyalara ilişkin trafik istatistikleri ölçümler panosunda bildirilir.

1. Depolama hesabı menüsünün **Monitör** bölümü altında **Ölçümler'i** tıklatın.

   > [!div class="mx-imgBorder"]
   > ![Ölçümler bağlantısı](./media/storage-blob-static-website/metrics-link.png)

   > [!NOTE]
   > Ölçüm verileri, farklı ölçüm API'lerine bağlanarak oluşturulur. Portal, yalnızca veri döndüren üyelere odaklanmak için belirli bir zaman dilimi içinde kullanılan API üyelerini görüntüler. Gerekli API üyesini seçebildiğinizden emin olmak için ilk adım zaman dilimini genişletmektir.

2. Zaman dilimi düğmesini tıklatın, bir zaman dilimi seçin ve sonra **Uygula'yı**tıklatın.

   ![Azure Depolama statik web siteleri ölçüm zaman aralığı](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. *Namespace* açılan damlasından **Blob'u** seçin.

   ![Azure Depolama statik web siteleri ölçümleri ad alanı](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. Ardından **Çıkış** ölçümünü seçin.

   ![Azure Depolama statik web siteleri ölçümleri metrik](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. *Toplama* seçiciden **Toplam'ı** seçin.

   ![Azure Depolama statik web siteleri ölçümleri toplama](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. Filtre **ekle** düğmesini tıklatın ve *Özellik* seçicisinden **API adını** seçin.

   ![Azure Depolama statik web siteleri API adını ölçümler](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. Ölçümler raporunu doldurmak için *Değerler* seçicisinde **GetWebContent'in** yanındaki kutuyu işaretleyin.

   ![Azure Depolama statik web siteleri ölçümleri GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

   >[!NOTE]
   > **GetWebContent** onay kutusu yalnızca bu API üyesi belirli bir zaman dilimi içinde kullanıldıysa görünür. Portal, yalnızca veri döndüren üyelere odaklanmak için belirli bir zaman dilimi içinde kullanılan API üyelerini görüntüler. Bu listede belirli bir API üyesi bulamazsanız, zaman dilimini genişletin.

## <a name="next-steps"></a>Sonraki adımlar

* Statik web sitenizle özel bir etki alanını nasıl yapılandırıştırıştırmayı öğrenin. Bkz. [Azure Blob Depolama bitiş noktası için özel bir etki alanını haritalama.](storage-custom-domain-name.md)

