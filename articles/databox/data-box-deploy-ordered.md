---
title: Sipariş Azure Data Box öğretici | Microsoft Docs
description: Bu öğreticide, şirket içi verileri Azure 'a aktarmanıza ve Azure Data Box nasıl sipariş alabileceğinizi sağlayan bir karma çözüm olan Azure Data Box hakkında bilgi edinin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: alkohli
ms.openlocfilehash: c2d971c2c9375f58fd5f41a46716fac4bff29f88
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604317"
---
# <a name="tutorial-order-azure-data-box"></a>Öğretici: Azure Data Box sipariş etme

Azure Data Box, şirket içi verilerinizi Azure'a hızlı, kolay ve güvenilir bir şekilde aktarmanızı sağlayan bir hibrit çözümdür. Verilerinizi Microsoft tarafından sağlanan 80 TB'lık (kullanılabilir kapasite) depolama cihazına aktarabilir ve cihazı geri gönderebilirsiniz. Bu veriler daha sonra Azure'a yüklenir.

Bu öğreticide Azure Data Box sipariş etme adımları açıklanır. Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Data Box dağıtımı önkoşulları
> * Data Box sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

## <a name="prerequisites"></a>Ön koşullar

# <a name="portal"></a>[Portal](#tab/portal)

Cihazı dağıtmadan önce Data Box hizmet ve cihaz için aşağıdaki yapılandırma önkoşullarını doldurun:

[!INCLUDE [Prerequisites](../../includes/data-box-deploy-ordered-prerequisites.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[!INCLUDE [Prerequisites](../../includes/data-box-deploy-ordered-prerequisites.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Azure 'da oturum açabilir ve Azure CLı komutlarını iki şekilde çalıştırabilirsiniz:

* CLı 'yı yükleyebilir ve CLı komutlarını yerel olarak çalıştırabilirsiniz.
* CLı komutlarını Azure Cloud Shell Azure portal içinden çalıştırabilirsiniz.

Öğretici için Windows PowerShell aracılığıyla Azure CLı kullanıyoruz, ancak her iki seçenekten birini de seçebilirsiniz.

### <a name="for-azure-cli"></a>Azure CLı için

Başlamadan önce aşağıdakilerden emin olun:

#### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) sürüm 2.0.67 veya üstünü yükler. Alternatif olarak, [MSI kullanarak yükleyebilirsiniz](https://aka.ms/installazurecliwindows).

**Azure'da oturum açma**

Bir Windows PowerShell komut penceresi açın ve [az Login](/cli/azure/reference-index#az-login) komutuyla Azure 'da oturum açın:

```azurecli
PS C:\Windows> az login
```

İşte başarılı bir oturum açma çıkışı:

```output
You have logged in. Now let us find all the subscriptions to which you have access.
[
   {
      "cloudName": "AzureCloud",
      "homeTenantId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "isDefault": true,
      "managedByTenants": [],
      "name": "My Subscription",
      "state": "Enabled",
      "tenantId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "user": {
          "name": "gusp@contoso.com",
          "type": "user"
      }
   }
]
```

**Azure Data Box CLı uzantısını yükler**

Azure Data Box CLı komutlarını kullanabilmeniz için, uzantıyı yüklemeniz gerekir. Azure CLI uzantıları, henüz temel CLI’nın parçası olarak gönderilmeyen deneysel ve ön sürüm komutlarına erişmenize olanak sağlar. Uzantılar hakkında daha fazla bilgi için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).

Azure Data Box uzantısını yüklemek için şu komutu çalıştırın: `az extension add --name databox` :

```azurecli

    PS C:\Windows> az extension add --name databox
```

Uzantı başarıyla yüklenirse aşağıdaki çıktıyı görürsünüz:

```output
    The installed extension 'databox' is experimental and not covered by customer support. Please use with discretion.
    PS C:\Windows>

    # az databox help

    PS C:\Windows> az databox -h

    Group
        az databox

    Subgroups:
        job [Experimental] : Commands to manage databox job.

    For more specific examples, use: az find "az databox"

        Please let us know how we are doing: https://aka.ms/clihats
```

#### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

CLı komutlarını çalıştırmak için tarayıcınız aracılığıyla Azure 'da barındırılan etkileşimli kabuk ortamı [Azure Cloud Shell](https://shell.azure.com/)kullanabilirsiniz. Azure Cloud Shell, Azure hizmetleriyle bash veya Windows PowerShell 'i destekler. Azure CLı, önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış. Azure portal sağ üst kısmında bulunan menüdeki Cloud Shell düğmesini seçin:

![Cloud Shell](../storage/common/media/storage-quickstart-create-account/cloud-shell-menu.png)

Düğme, bu nasıl yapılır makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır.

# <a name="powershell"></a>[PowerShell](#tab/azure-ps)

[!INCLUDE [Prerequisites](../../includes/data-box-deploy-ordered-prerequisites.md)]

### <a name="for-azure-powershell"></a>Azure PowerShell için

Başlamadan önce şunları yaptığınızdan emin olun:

* Windows PowerShell 6.2.4 veya üstünü yükler.
* Azure PowerShell (AZ) modülünü yükler.
* Azure Data Box (az. DataBox) modülünü yükler.
* Azure'da oturum açın.

#### <a name="install-azure-powershell-and-modules-locally"></a>Azure PowerShell ve modülleri yerel olarak yükler

**Windows PowerShell 'i yükler veya yükseltin**

Windows PowerShell sürüm 6.2.4 veya sonraki bir sürümün yüklü olması gerekir. Hangi PowerShell sürümünü yüklebileceğinizi öğrenmek için şunu çalıştırın: `$PSVersionTable` .

Şu çıkışı görürsünüz:

```azurepowershell
    PS C:\users\gusp> $PSVersionTable
    
    Name                           Value
    ----                           -----
    PSVersion                      6.2.3
    PSEdition                      Core
    GitCommitId                    6.2.3
    OS                             Microsoft Windows 10.0.18363
    Platform                       Win32NT
    PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
    PSRemotingProtocolVersion      2.3
    SerializationVersion           1.1.0.1
    WSManStackVersion              3.0
```

Sürümünüz 6.2.4 'den düşükse, Windows PowerShell sürümünüzü yükseltmeniz gerekir. Windows PowerShell 'in en son sürümünü yüklemek için bkz. [ınstall Azure PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell?view=powershell-7).

**Azure PowerShell ve Data Box modülleri 'ni yükler**

Azure Data Box sıralamak için Azure PowerShell kullanmak üzere Azure PowerShell modüllerini yüklemeniz gerekir. Azure PowerShell modüllerini yüklemek için:

1. [Azure PowerShell az Module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)' ü yükler.
2. Ardından komutunu kullanarak az. DataBox öğesini kullanın `Install-Module -Name Az.DataBox` .

```azurepowershell
PS C:\PowerShell\Modules> Install-Module -Name Az.DataBox
PS C:\PowerShell\Modules> Get-InstalledModule -Name "Az.DataBox"

Version              Name                                Repository           Description
-------              ----                                ----------           -----------
0.1.1                Az.DataBox                          PSGallery            Microsoft Azure PowerShell - DataBox ser…
```

#### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Bir Windows PowerShell komut penceresi açın ve [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) komutuyla Azure 'da oturum açın:

```azurepowershell
PS C:\Windows> Connect-AzAccount
```

İşte başarılı bir oturum açma çıkışı:

```output
WARNING: To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FSBFZMBKC to authenticate.

Account              SubscriptionName                          TenantId                             Environment
-------              ----------------                          --------                             -----------
gusp@contoso.com     MySubscription                            aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa AzureCloud

PS C:\Windows\System32>
```

Windows PowerShell kullanarak Azure 'da oturum açma hakkında ayrıntılı bilgi için bkz. [Azure PowerShell Ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

---

## <a name="order-data-box"></a>Data Box sipariş etme

# <a name="portal"></a>[Portal](#tab/portal)

Bir cihazı sıralamak için Azure portal aşağıdaki adımları uygulayın.

1. Microsoft Azure kimlik bilgilerini kullanarak şu URL’de oturum açın: [https://portal.azure.com](https://portal.azure.com).
2. **+ Kaynak oluştur**’u seçin ve *Azure Data Box* araması yapın. **Azure Data Box**’ı seçin.

   ![Azure Data Box seçin](media/data-box-deploy-ordered/select-data-box-import-02.png)

3. **Oluştur**’u seçin.

   ![Azure Data Box seçin](media/data-box-deploy-ordered/select-data-box-import-03.png)

4. Data Box'ın bölgenizde kullanılabilir olup olmadığını kontrol edin. Aşağıdaki bilgileri girin veya seçin ve sonra **Uygula**'yı seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Aktarım türü     | **Azure’a içeri aktar**’ı seçin.        |
    |Abonelik     | Data Box hizmeti için bir EA, CSP veya Azure sponsorluk aboneliği seçin. <br> Abonelik fatura hesabınıza bağlıdır.       |
    |Kaynak Grubu | Var olan bir kaynak grubunu seçin. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. |
    |Kaynak ülke/bölge    |    Verilerinizin bulunduğu ülkeyi/bölgeyi seçin.         |
    |Hedef Azure bölgesi     |     Verileri aktarmak istediğiniz Azure bölgesini seçin. <br> Daha fazla bilgi için [bölge kullanılabilirliği](data-box-overview.md#region-availability)’ne gidin.            |

    [![Azure Data Box içeri aktarma sırası](media/data-box-deploy-ordered/select-data-box-import-04b.png)](media/data-box-deploy-ordered/select-data-box-import-04b.png#lightbox)

5. **Data Box**'ı seçin. Tek bir sipariş için kullanılabilir maksimum kapasite 80 TB 'tır. Daha büyük veri boyutları için birden fazla sipariş oluşturabilirsiniz.

    ![Data Box seçeneğini belirtin 1](media/data-box-deploy-ordered/select-data-box-import-05.png)

6. **Sırasıyla** **temel bilgiler** sekmesine gidin. Aşağıdaki bilgileri girin veya seçin ve Ileri ' **yi seçin: veri hedefi>**.

    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik      | Abonelik, önceki seçiminize göre otomatik olarak doldurulur.|
    |Kaynak grubu    | Daha önce seçtiğiniz kaynak grubu. |
    |İçeri aktarma sırası adı | Siparişi takip etmek için kullanılacak kolay bir ad girin. <br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir.    |

    ![Data Box seçeneğini belirtin 1](media/data-box-deploy-ordered/select-data-box-import-06.png)

    Varsayılan olarak, cihaz kilidi açma parolası, Microsoft tarafından yönetilen bir anahtar kullanılarak şifrelenir. Siparişi tamamladıktan sonra, müşteri tarafından yönetilen anahtar ekleyebilirsiniz. Müşteri tarafından yönetilen anahtar, cihazın kilidini açma parolasını korumak için bir Azure Anahtar Kasası anahtarından kendi anahtarınızı kullanmanıza olanak sağlar. Daha fazla bilgi için bkz. [Azure Data Box için Azure Key Vault müşteri tarafından yönetilen anahtarları kullanma](data-box-customer-managed-encryption-key-portal.md).

7. **Veri hedefi** sekmesinde **veri hedefi**' ni seçin.

    Depolama **hesabı (ler)** i depolama hedefi olarak kullanılıyorsa, aşağıdaki ekran görüntüsünü görürsünüz:

    ![Azure Data Box veri hedefi](media/data-box-deploy-ordered/select-data-box-import-07.png)

    Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir veya daha fazla depolama hesabı seçin. Data Box en çok 10 depolama hesabına bağlanabilir. Dilerseniz yeni bir **Genel amaçlı v1**, **Genel amaçlı v2** veya **Blob depolama hesabı** da oluşturabilirsiniz.

    Sanal ağları olan depolama hesapları desteklenir. Data Box hizmetinin güvenli depolama hesaplarıyla çalışmasına izin vermek için, depolama hesabı ağ güvenlik duvarı ayarları içinden güvenilen hizmetleri etkinleştirin. Daha fazla bilgi için bkz. [Azure Data Box güvenilir hizmet olarak ekleme](../storage/common/storage-network-security.md#exceptions).

    Şirket içi sanal sabit disklerde (VHD) **yönetilen diskler** oluşturmak için Data Box kullanıyorsanız, aşağıdaki bilgileri de sağlamanız gerekir:

    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak grupları     | Şirket içi VHD'lerden yönetilen diskler oluşturmayı düşünüyorsanız yeni kaynak grupları oluşturun. Mevcut bir kaynak grubunu yalnızca kaynak grubu, Data Box hizmeti tarafından yönetilen disk için bir Data Box sırası oluşturulurken daha önce oluşturulduysa kullanabilirsiniz. <br> Noktalı virgülle ayrılmış birden çok kaynak grubu belirtin. En fazla 10 kaynak grubu desteklenir.|

    ![Yönetilen disk için Data Box sıralaması](media/data-box-deploy-ordered/select-data-box-import-07b.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti, VHD'leri yönetilen disklere dönüştürmeden ve kaynak gruplarına taşımadan önce hazırlama depolama hesabına sayfa blobları olarak yükler. Daha fazla bilgi için bkz. [Azure'a veri yüklemeyi doğrulama](data-box-deploy-picked-up.md#verify-data-upload-to-azure).
   > [!NOTE]
   > Bir Sayfa Blobu başarıyla yönetilen bir diske dönüştürülemiyorsa, depolama hesabında kalır ve depolama alanı için ücretlendirilirsiniz.

    **İleri ' yi seçin:** devam etmek için güvenlik.

1. **Güvenlik**bölümünde, yazılım tabanlı çift şifrelemeyi etkinleştirmek istiyorsanız, **sırasıyla çift şifrelemeyi etkinleştir**' i seçin. 

   Yazılım tabanlı şifreleme, Data Box verilerin AES-256 bit şifrelemesine ek olarak gerçekleştirilir.

   > [!NOTE]
   > Bu seçeneğin etkinleştirilmesi, sipariş işleme ve veri kopyalamanın daha uzun sürmesiyle yapılır. Siparişinizi oluşturduktan sonra bu seçeneği değiştiremezsiniz.

   ![Veri kutusu içeri aktarma için güvenlik ekranı, Çift şifreleme](media/data-box-deploy-ordered/select-data-box-import-07c.png)

   Ileri ' yi seçin: devam etmek için **kişi ayrıntıları** .

8. **Kişi ayrıntıları**' nda **+ Sevkiyat Adresi Ekle**' yi seçin.

    ![Yönetilen disk için Data Box sıralaması](media/data-box-deploy-ordered/select-data-box-import-08a.png)

9. **Teslimat adresi**’ne adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarasını girin. **Adresi doğrula**'yı seçin. Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir.

   ![Yönetilen disk için Data Box sıralaması](media/data-box-deploy-ordered/select-data-box-import-10.png)

   Kendi kendine yönetilen sevkiyat ' ı seçtiyseniz, sipariş başarıyla yerleştirildikten sonra bir e-posta bildirimi alırsınız. Kendi kendine yönetilen kargo hakkında daha fazla bilgi için bkz. [otomatik olarak yönetilen gönderi kullanma](data-box-portal-customer-managed-shipping.md).

10. Sevkiyat ayrıntıları başarıyla doğrulandıktan sonra **Sevkiyat Adresi Ekle** ' yi seçin. **Kişi ayrıntıları** sekmesine geri dönecaksınız.

11. **İletişim ayrıntılarına** geri döndüğünüzde bir veya daha fazla e-posta adresi ekleyin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir.

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

    ![Yönetilen disk için Data Box sıralaması](media/data-box-deploy-ordered/select-data-box-import-08c.png)

12. **İnceleme** ve sipariş, iletişim, bildirim ve gizlilik koşulları ile ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

13. **Sipariş**'i seçin. Siparişin oluşturulması birkaç dakika sürer.

    ![Yönetilen disk için Data Box sıralaması](media/data-box-deploy-ordered/select-data-box-import-11.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir cihaz sıralamak için Azure CLı kullanarak aşağıdaki adımları uygulayın:

1. Data Box siparişiniz için ayarlarınızı yazın. Bu ayarlar kişisel/iş bilgilerinizi, abonelik adınızı, cihaz bilgilerini ve gönderi bilgilerini içerir. Data Box sırasını oluşturmak için CLı komutunu çalıştırırken bu ayarları parametre olarak kullanmanız gerekir. Aşağıdaki tabloda, için kullanılan parametre ayarları gösterilmektedir `az databox job create` :

   | Ayar (parametre) | Description |  Örnek değer |
   |---|---|---|
   |resource-group| Var olan bir taneyi kullanın veya yenisini oluşturun. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
   |name| Oluşturmakta olduğunuz siparişin adı. | "mydataboxorder"|
   |kişi adı| Sevkiyat adresiyle ilişkilendirilen ad. | "Gus Polonya"|
   |telefon| Siparişi alacak kişinin veya işletmenin telefon numarası.| "14255551234"
   |location| Cihazınızı teslim edecek en yakın Azure bölgesi.| "ABD Batı"|
   |isteyin| Sipariş ettiğiniz özel Data Box cihaz. Geçerli değerler şunlardır: "DataBox", "DataBoxDisk" ve "Databoxağır"| "DataBox" |
   |e-posta-Listele| Siparişle ilişkili e-posta adresleri.| "gusp@contoso.com" |
   |Cadde-Address1| Siparişin sevk edileceği sokak adresi. | "15700 NE 39th" |
   |Cadde-Address2| Apartman numarası veya bina numarası gibi ikincil adres bilgileri. | "Bld 123" |
   |city| Cihazın sevk edileceği şehir. | Redmond |
   |Eyalet-or-il| Cihazın sevk edileceği durum.| HECESI |
   |ülke| Cihazın sevk edileceği ülke. | "Birleşik Devletler" |
   |posta kodu| Sevkiyat adresiyle ilişkili posta kodu veya posta kodu.| "98052"|
   |Şirket adı| Çalıştığınız şirketinizin adı.| "Contoso, LTD" |
   |depolama hesabı| Verileri içeri aktarmak istediğiniz Azure depolama hesabı.| mystorageaccount|
   |hata ayıklama| Ayrıntılı günlüğe hata ayıklama bilgilerini dahil et  | --Hata Ayıkla |
   |Yardım| Bu komutla ilgili yardım bilgilerini görüntüleyin. | --Yardım-h |
   |yalnızca-hataları göster| Yalnızca hataları göster, uyarıları gizleme. | --yalnızca-show-Errors |
   |çıkış-o| Çıkış biçimini ayarlar.  İzin verilen değerler: JSON, jsonc, None, Table, TSV, YAML, yamlc. Varsayılan değer JSON ' dır. | --output "JSON" |
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu <string>|
   |ayrıntılı| Ayrıntılı günlük kaydı ekleyin. | --ayrıntılı |

2. Tercih ettiğiniz veya terminalin komut isteminde, Azure Data Box siparişinizi oluşturmak için [az Data Box Job Create](https://docs.microsoft.com/cli/azure/ext/databox/databox/job?view=azure-cli-latest#ext-databox-az-databox-job-create) komutunu kullanın.

   ```azurecli
   az databox job create --resource-group <resource-group> --name <order-name> --location <azure-location> --sku <databox-device-type> --contact-name <contact-name> --phone <phone-number> --email-list <email-list> --street-address1 <street-address-1> --street-address2 <street-address-2> --city "contact-city" --state-or-province <state-province> --country <country> --postal-code <postal-code> --company-name <company-name> --storage-account "storage-account"
   ```

   Komut kullanımı örneği aşağıda verilmiştir:

   ```azurecli
   az databox job create --resource-group "myresourcegroup" \
                         --name "mydataboxtest3" \
                         --location "westus" \
                         --sku "DataBox" \
                         --contact-name "Gus Poland" \
                         --phone "14255551234" \
                         --email-list "gusp@contoso.com" \
                         --street-address1 "15700 NE 39th St" \
                         --street-address2 "Bld 25" \
                         --city "Redmond" \
                         --state-or-province "WA" \
                         --country "US" \
                         --postal-code "98052" \
                         --company-name "Contoso" \
                         --storage-account mystorageaccount
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
   Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.
   {
     "cancellationReason": null,
     "deliveryInfo": {
        "scheduledDateTime": "0001-01-01T00:00:00+00:00"
   },
   "deliveryType": "NonScheduled",
   "details": null,
   "error": null,
   "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.DataBox/jobs/mydataboxtest3",
   "identity": {
     "type": "None"
   },
   "isCancellable": true,
   "isCancellableWithoutFee": true,
   "isDeletable": false,
   "isShippingAddressEditable": true,
   "location": "westus",
   "name": "mydataboxtest3",
   "resourceGroup": "myresourcegroup",
   "sku": {
     "displayName": null,
     "family": null,
     "name": "DataBox"
   },
   "startTime": "2020-06-10T23:28:27.354241+00:00",
   "status": "DeviceOrdered",
   "tags": {},
   "type": "Microsoft.DataBox/jobs"

   }
   PS C:\Windows>

   ```

3. Değişiklik yapmadığınız sürece, tüm Azure CLı komutları varsayılan olarak JSON 'ı çıkış biçimi olarak kullanır. Çıkış biçimini Genel parametresini kullanarak değiştirebilirsiniz `--output <output-format>` . Biçimin "Tablo" olarak değiştirilmesi, çıkış okunabilirliğini iyileştirir.

   Biçimlendirmeyi değiştirmek için az bir ince ayar ile çalıştık aynı komut aşağıda verilmiştir:

    ```azurecli
    az databox job create --resource-group "myresourcegroup" --name "mydataboxtest4" --location "westus" --sku "DataBox" --contact-name "Gus Poland" --phone "14255551234" --email-list "gusp@contoso.com" --street-address1 "15700 NE 39th St" --street-address2 "Bld 25" --city "Redmond" --state-or-province "WA" --country "US" --postal-code "98052" --company-name "Contoso" --storage-account mystorageaccount --output "table"
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output

    Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.
    DeliveryType    IsCancellable    IsCancellableWithoutFee    IsDeletable    IsShippingAddressEditable    Location    Name            ResourceGroup    StartTime                         Status
    --------------  ---------------  -------------------------  -------------  ---------------------------  ----------  --------------  ---------------  --------------------------------  -------------
    NonScheduled    True             True                       False          True                         westus      mydataboxtest4  myresourcegroup  2020-06-18T03:48:00.905893+00:00  DeviceOrdered

    ```

# <a name="powershell"></a>[PowerShell](#tab/azure-ps)

Bir cihazı sıralamak için Azure PowerShell kullanarak aşağıdaki adımları uygulayın:

1. İçeri Aktarma sırasını oluşturmadan önce depolama hesabınızı almanız ve depolama hesabı nesnesini bir değişkende kaydetmeniz gerekir.

   ```azurepowershell
    $storAcct = Get-AzStorageAccount -Name "mystorageaccount" -ResourceGroup "myresourcegroup"
   ```

2. Data Box siparişiniz için ayarlarınızı yazın. Bu ayarlar kişisel/iş bilgilerinizi, abonelik adınızı, cihaz bilgilerini ve gönderi bilgilerini içerir. Data Box sırasını oluşturmak için PowerShell komutunu çalıştırırken bu ayarları parametre olarak kullanmanız gerekir. Aşağıdaki tabloda [New-AzDataBoxJob](https://docs.microsoft.com/powershell/module/az.databox/New-AzDataBoxJob)için kullanılan parametre ayarları gösterilmektedir.

    | Ayar (parametre) | Description |  Örnek değer |
    |---|---|---|
    |ResourceGroupName [gerekli]| Mevcut bir kaynak grubunu kullanın. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
    |Ad [gerekli]| Oluşturmakta olduğunuz siparişin adı. | "mydataboxorder"|
    |ContactName [gerekli]| Sevkiyat adresiyle ilişkilendirilen ad. | "Gus Polonya"|
    |PhoneNumber [gerekli]| Siparişi alacak kişinin veya işletmenin telefon numarası.| "14255551234"
    |Konum [gerekli]| Cihazınızı teslim edecek en yakın Azure bölgesi.| WestUS|
    |DataBoxType [gerekli]| Sipariş ettiğiniz özel Data Box cihaz. Geçerli değerler şunlardır: "DataBox", "DataBoxDisk" ve "Databoxağır"| "DataBox" |
    |Emailıd [gerekli]| Siparişle ilişkili e-posta adresleri.| "gusp@contoso.com" |
    |StreetAddress1 [gerekli]| Siparişin sevk edileceği sokak adresi. | "15700 NE 39th" |
    |StreetAddress2| Apartman numarası veya bina numarası gibi ikincil adres bilgileri. | "Bld 123" |
    |StreetAddress3| Üçüncül adres bilgileri. | |
    |Şehir [gerekli]| Cihazın sevk edileceği şehir. | Redmond |
    |StateOrProvinceCode [gerekli]| Cihazın sevk edileceği durum.| HECESI |
    |CountryCode [gerekli]| Cihazın sevk edileceği ülke. | "Birleşik Devletler" |
    |PostaKodu [gerekli]| Sevkiyat adresiyle ilişkili posta kodu veya posta kodu.| "98052"|
    |CompanyName| Çalıştığınız şirketinizin adı.| "Contoso, LTD" |
    |Storageaccountresourceıd [gerekli]| Verileri içeri aktarmak istediğiniz Azure depolama hesabı KIMLIĞI.| <AzStorageAccount>. ID |

3. Komut istemindeki veya terminalinizde, Azure Data Box siparişinizi oluşturmak için [New-AzDataBoxJob](https://docs.microsoft.com/powershell/module/az.databox/New-AzDataBoxJob) ' u kullanın.

   ```azurepowershell
    PS> $storAcct = Get-AzureStorageAccount -StorageAccountName "mystorageaccount"
    PS> New-AzDataBoxJob -Location "WestUS" \
                         -StreetAddress1 "15700 NE 39th St" \
                         -PostalCode "98052" \
                         -City "Redmond" \
                         -StateOrProvinceCode "WA" \
                         -CountryCode "US" \
                         -EmailId "gusp@contoso.com" \
                         -PhoneNumber 4255551234 \
                         -ContactName "Gus Poland" \
                         -StorageAccount $storAcct.id \
                         -DataBoxType DataBox \
                         -ResourceGroupName "myresourcegroup" \
                         -Name "myDataBoxOrderPSTest"
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
    jobResource.Name     jobResource.Sku.Name jobResource.Status jobResource.StartTime jobResource.Location ResourceGroup
    ----------------     -------------------- ------------------ --------------------- -------------------- -------------
    myDataBoxOrderPSTest DataBox              DeviceOrdered      07-06-2020 05:25:30   westus               myresourcegroup
   ```

---

## <a name="track-the-order"></a>Siparişi izleme

# <a name="portal"></a>[Portal](#tab/portal)

Siparişi verdikten sonra durumunu Azure portalından takip edebilirsiniz. Data Box siparişiniz ' ne gidin ve durumu görüntülemek için **genel bakış** ' a gidin. Portalda siparişin durumu **Sipariş edildi** olarak görünür.

Cihaz yoksa bir bildirim alırsınız. Cihaz varsa, Microsoft gönderilecek cihazı belirler ve gönderimi hazırlar. Cihaz hazırlanırken şu eylemler gerçekleştirilir:

* Cihazla ilişkili her depolama hesabı için SMB paylaşımları oluşturulur.
* Her paylaşım için, kullanıcı adı veya parola gibi erişim kimlik bilgileri oluşturulur.
* Cihaz kilidinin açılmasına yardımcı olan cihaz parolası da oluşturulur.
* Data Box, herhangi bir noktada cihaza yetkisiz erişimi önlemek için kilitlenmiştir.

Cihazın hazırlanması tamamlandığında portalda sipariş durumu **İşlendi** olarak değişir.

![Data Box siparişi işlendi](media/data-box-overview/data-box-order-status-processed.png)

Microsoft ardından cihazınızı hazırlar ve bölgeye uygun gönderim şirketine teslim eder. Cihaz gönderildikten sonra bir takip numarası iletilir. Portalda sipariş durumu **Yola çıktı** olarak değişir.

![Data Box siparişi yola çıktı](media/data-box-overview/data-box-order-status-dispatched.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="track-a-single-order"></a>Tek bir siparişi izleyin

Tek, mevcut bir Azure Data Box sıra hakkında izleme bilgileri almak için [az databox iş göster](https://docs.microsoft.com/cli/azure/ext/databox/databox/job?view=azure-cli-latest#ext-databox-az-databox-job-show)' i çalıştırın. Bu komut, şu sıralama hakkında bilgiler görüntüler: ad, kaynak grubu, izleme bilgileri, abonelik KIMLIĞI, iletişim bilgileri, sevkiyat türü ve cihaz SKU 'su.

   ```azurecli
   az databox job show --resource-group <resource-group> --name <order-name>
   ```

   Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `az databox job show` :

   | Parametre | Açıklama |  Örnek değer |
   |---|---|---|
   |kaynak grubu [gerekli]| Siparişle ilişkili kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
   |ad [gerekli]| Görüntülenecek sıranın adı. | "mydataboxorder"|
   |hata ayıklama| Ayrıntılı günlüğe hata ayıklama bilgilerini dahil et | --Hata Ayıkla |
   |Yardım| Bu komutla ilgili yardım bilgilerini görüntüleyin. | --Yardım-h |
   |yalnızca-hataları göster| Yalnızca hataları göster, uyarıları gizleme. | --yalnızca-show-Errors |
   |çıkış-o| Çıkış biçimini ayarlar.  İzin verilen değerler: JSON, jsonc, None, Table, TSV, YAML, yamlc. Varsayılan değer JSON ' dır. | --output "JSON" |
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu <string>|
   |ayrıntılı| Ayrıntılı günlük kaydı ekleyin. | --ayrıntılı |

   Çıktı biçimi "Tablo" olarak ayarlanan komuta bir örnek aşağıda verilmiştir:

   ```azurecli
    PS C:\WINDOWS\system32> az databox job show --resource-group "myresourcegroup" \
                                                --name "mydataboxtest4" \
                                                --output "table"
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
    Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.
    DeliveryType    IsCancellable    IsCancellableWithoutFee    IsDeletable    IsShippingAddressEditable    Location    Name            ResourceGroup    StartTime                         Status
    --------------  ---------------  -------------------------  -------------  ---------------------------  ----------  --------------  ---------------  --------------------------------  -------------
    NonScheduled    True             True                       False          True                         westus      mydataboxtest4  myresourcegroup  2020-06-18T03:48:00.905893+00:00  DeviceOrdered
   ```

> [!NOTE]
> Liste sırası, abonelik düzeyinde desteklenebilir ve kaynak grubuna isteğe bağlı bir parametre (gerekli bir parametre değil) oluşturur.

### <a name="list-all-orders"></a>Tüm siparişleri Listele

Birden çok cihaz sipariş ediyorsanız, tüm Azure Data Box siparişlerinizi görüntülemek için [az databox iş listesi](https://docs.microsoft.com/cli/azure/ext/databox/databox/job?view=azure-cli-latest#ext-databox-az-databox-job-list) ' ni çalıştırabilirsiniz. Komut, belirli bir kaynak grubuna ait olan tüm siparişleri listeler. Çıktıda da gösterildiği gibi: sipariş adı, Sevkiyat durumu, Azure bölgesi, teslim türü, sipariş durumu. İptal edilen siparişler listeye de dahildir.
Komut ayrıca her bir siparişin zaman damgalarını görüntüler.

```azurecli
az databox job list --resource-group <resource-group>
```

Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `az databox job list` :

   | Parametre | Açıklama |  Örnek değer |
   |---|---|---|
   |kaynak grubu [gerekli]| Siparişleri içeren kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
   |hata ayıklama| Ayrıntılı günlüğe hata ayıklama bilgilerini dahil et | --Hata Ayıkla |
   |Yardım| Bu komutla ilgili yardım bilgilerini görüntüleyin. | --Yardım-h |
   |yalnızca-hataları göster| Yalnızca hataları göster, uyarıları gizleme. | --yalnızca-show-Errors |
   |çıkış-o| Çıkış biçimini ayarlar.  İzin verilen değerler: JSON, jsonc, None, Table, TSV, YAML, yamlc. Varsayılan değer JSON ' dır. | --output "JSON" |
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu <string>|
   |ayrıntılı| Ayrıntılı günlük kaydı ekleyin. | --ayrıntılı |

   Çıktı biçimi "Tablo" olarak ayarlanan komuta bir örnek aşağıda verilmiştir:

   ```azurecli
    PS C:\WINDOWS\system32> az databox job list --resource-group "GDPTest" --output "table"
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
   Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.
   CancellationReason                                               DeliveryType    IsCancellable    IsCancellableWithoutFee    IsDeletable    IsShippingAddressEditable    Location    Name                 ResourceGroup    StartTime                         Status
   ---------------------- ----------------------------------------  --------------  ---------------  -------------------------  -------------  ---------------------------  ----------  -------------------  ---------------  --------------------------------  -------------
   OtherReason This was a test order for documentation purposes.    NonScheduled    False            False                      True           False                        westus      gdpImportTest        MyResGrp         2020-05-26T23:20:57.464075+00:00  Cancelled
   NoLongerNeeded This order was created for documentation purposes.NonScheduled    False            False                      True           False                        westus      mydataboxExportTest  MyResGrp         2020-05-27T00:04:16.640397+00:00  Cancelled
   IncorrectOrder                                                   NonScheduled    False            False                      True           False                        westus      mydataboxtest2       MyResGrp         2020-06-10T16:54:23.509181+00:00  Cancelled
                                                                    NonScheduled    True             True                       False          True                         westus      mydataboxtest3       MyResGrp         2020-06-11T22:05:49.436622+00:00  DeviceOrdered
                                                                    NonScheduled    True             True                       False          True                         westus      mydataboxtest4       MyResGrp         2020-06-18T03:48:00.905893+00:00  DeviceOrdered
   PS C:\WINDOWS\system32>
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-ps)

### <a name="track-a-single-order"></a>Tek bir siparişi izleyin

Tek, var olan bir Azure Data Box sıra hakkında izleme bilgileri almak için [Get-AzDataBoxJob](https://docs.microsoft.com/powershell/module/az.databox/Get-AzDataBoxJob)komutunu çalıştırın. Bu komut, şu sıralama hakkında bilgiler görüntüler: ad, kaynak grubu, izleme bilgileri, abonelik KIMLIĞI, iletişim bilgileri, sevkiyat türü ve cihaz SKU 'su.

> [!NOTE]
> `Get-AzDataBoxJob` hem tek hem de birden çok siparişi görüntülemek için kullanılır. Fark, tek siparişler için sipariş adını belirtmektir.

   ```azurepowershell
    Get-AzDataBoxJob -ResourceGroupName <String> -Name <String>
   ```

   Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `Get-AzDataBoxJob` :

   | Parametre | Açıklama |  Örnek değer |
   |---|---|---|
   |ResourceGroup [gerekli]| Siparişle ilişkili kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
   |Ad [gerekli]| Bilgi alınacak siparişin adı. | "mydataboxorder"|
   |ResourceId| Siparişle ilişkili kaynağın KIMLIĞI. |  |

   Çıktıyı içeren komuta bir örnek aşağıda verilmiştir:

   ```azurepowershell
    PS C:\WINDOWS\system32> Get-AzDataBoxJob -ResourceGroupName "myResourceGroup" -Name "myDataBoxOrderPSTest"
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
   jobResource.Name     jobResource.Sku.Name jobResource.Status jobResource.StartTime jobResource.Location ResourceGroup
   ----------------     -------------------- ------------------ --------------------- -------------------- -------------
   myDataBoxOrderPSTest DataBox              DeviceOrdered      7/7/2020 12:37:16 AM  WestUS               myResourceGroup
   ```

### <a name="list-all-orders"></a>Tüm siparişleri Listele

Birden çok cihaz sipariş ediyorsanız, tüm Azure Data Box siparişlerinizi görüntülemek için [Get-AzDataBoxJob](https://docs.microsoft.com/powershell/module/az.databox/Get-AzDataBoxJob) komutunu çalıştırabilirsiniz. Komut, belirli bir kaynak grubuna ait olan tüm siparişleri listeler. Çıktıda da gösterildiği gibi: sipariş adı, Sevkiyat durumu, Azure bölgesi, teslim türü, sipariş durumu. İptal edilen siparişler listeye de dahildir.
Komut ayrıca her bir siparişin zaman damgalarını görüntüler.

```azurepowershell
Get-AzDataBoxJob -ResourceGroupName <String>
```

Aşağıda komutun bir örneğini inceleyebilirsiniz:

```azurepowershell
PS C:\WINDOWS\system32> Get-AzDataBoxJob -ResourceGroupName "myResourceGroup"
```

Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

```output
jobResource.Name     jobResource.Sku.Name jobResource.Status jobResource.StartTime jobResource.Location ResourceGroup
----------------     -------------------- ------------------ --------------------- -------------------- -------------
guspImportTest       DataBox              Cancelled          5/26/2020 11:20:57 PM WestUS               myResourceGroup
mydataboxExportTest  DataBox              Cancelled          5/27/2020 12:04:16 AM WestUS               myResourceGroup
mydataboximport1     DataBox              Cancelled          6/26/2020 11:00:34 PM WestUS               myResourceGroup
myDataBoxOrderPSTest DataBox              Cancelled          7/07/2020 12:37:16 AM WestUS               myResourceGroup
mydataboxtest2       DataBox              Cancelled          6/10/2020 4:54:23  PM WestUS               myResourceGroup
mydataboxtest4       DataBox              DeviceOrdered      6/18/2020 3:48:00  AM WestUS               myResourceGroup
PS C:\WINDOWS\system32>
```

---

## <a name="cancel-the-order"></a>Siparişi iptal etme

# <a name="portal"></a>[Portal](#tab/portal)

Bu sırayı iptal etmek için Azure portal, **Genel Bakış ' a** gidin ve komut çubuğundan **iptal** ' i seçin.

Sipariş verdikten sonra, sipariş durumu işlendi olmadan önce herhangi bir noktada siparişi iptal edebilirsiniz.

İptal edilen bir siparişi silmek için **genel bakış** ' a gidin ve komut çubuğundan **Sil** ' i seçin.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="cancel-an-order"></a>Siparişi iptal etme

Azure Data Box sırayı iptal etmek için [az databox Job Cancel](https://docs.microsoft.com/cli/azure/ext/databox/databox/job?view=azure-cli-latest#ext-databox-az-databox-job-cancel)' ı çalıştırın. Siparişi iptal etme nedeninizi belirtmeniz gerekir.

   ```azurecli
   az databox job cancel --resource-group <resource-group> --name <order-name> --reason <cancel-description>
   ```

   Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `az databox job cancel` :

   | Parametre | Açıklama |  Örnek değer |
   |---|---|---|
   |kaynak grubu [gerekli]| Silinecek siparişle ilişkili kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
   |ad [gerekli]| Silinecek siparişin adı. | "mydataboxorder"|
   |Neden [gerekli]| Siparişi iptal etme nedeni. | "Hatalı bilgiler girdim ve siparişi iptal etmek için gerekli." |
   |evet| Onay sorma. | --Evet (-y)| --Evet-y |
   |hata ayıklama| Ayrıntılı günlüğe hata ayıklama bilgilerini dahil et | --Hata Ayıkla |
   |Yardım| Bu komutla ilgili yardım bilgilerini görüntüleyin. | --Yardım-h |
   |yalnızca-hataları göster| Yalnızca hataları göster, uyarıları gizleme. | --yalnızca-show-Errors |
   |çıkış-o| Çıkış biçimini ayarlar.  İzin verilen değerler: JSON, jsonc, None, Table, TSV, YAML, yamlc. Varsayılan değer JSON ' dır. | --output "JSON" |
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu <string>|
   |ayrıntılı| Ayrıntılı günlük kaydı ekleyin. | --ayrıntılı |

   Çıktıyı içeren komuta bir örnek aşağıda verilmiştir:

   ```azurecli
   PS C:\Windows> az databox job cancel --resource-group "myresourcegroup" --name "mydataboxtest3" --reason "Our budget was slashed due to **redacted** and we can no longer afford this device."
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
   Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.
   Are you sure you want to perform this operation? (y/n): y
   PS C:\Windows>
   ```

### <a name="delete-an-order"></a>Bir siparişi silme

Azure Data Box sırayı iptal ediyorsanız, siparişi silmek için [az databox Job Delete](https://docs.microsoft.com/cli/azure/ext/databox/databox/job?view=azure-cli-latest#ext-databox-az-databox-job-delete) ' i çalıştırabilirsiniz.

   ```azurecli
   az databox job delete --name [-n] <order-name> --resource-group <resource-group> [--yes] [--verbose]
   ```

   Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `az databox job delete` :

   | Parametre | Açıklama |  Örnek değer |
   |---|---|---|
   |kaynak grubu [gerekli]| Silinecek siparişle ilişkili kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
   |ad [gerekli]| Silinecek siparişin adı. | "mydataboxorder"|
   |aboneliği| Azure aboneliğinizin adı veya KIMLIĞI (GUID). | "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" |
   |evet| Onay sorma. | --Evet (-y)| --Evet-y |
   |hata ayıklama| Ayrıntılı günlüğe hata ayıklama bilgilerini dahil et | --Hata Ayıkla |
   |Yardım| Bu komutla ilgili yardım bilgilerini görüntüleyin. | --Yardım-h |
   |yalnızca-hataları göster| Yalnızca hataları göster, uyarıları gizleme. | --yalnızca-show-Errors |
   |çıkış-o| Çıkış biçimini ayarlar.  İzin verilen değerler: JSON, jsonc, None, Table, TSV, YAML, yamlc. Varsayılan değer JSON ' dır. | --output "JSON" |
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu <string>|
   |ayrıntılı| Ayrıntılı günlük kaydı ekleyin. | --ayrıntılı |

Çıktıyı içeren komuta bir örnek aşağıda verilmiştir:

   ```azurecli
   PS C:\Windows> az databox job delete --resource-group "myresourcegroup" --name "mydataboxtest3" --yes --verbose
   ```

   Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

   ```output
   Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.
   command ran in 1.142 seconds.
   PS C:\Windows>
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-ps)

### <a name="cancel-an-order"></a>Siparişi iptal etme

Azure Data Box sırayı iptal etmek için [stop-AzDataBoxJob](https://docs.microsoft.com/powershell/module/az.databox/stop-azdataboxjob)komutunu çalıştırın. Siparişi iptal etme nedeninizi belirtmeniz gerekir.

```azurepowershell
Stop-AzDataBoxJob -ResourceGroup <String> -Name <String> -Reason <String>
```

Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `Stop-AzDataBoxJob` :

| Parametre | Açıklama |  Örnek değer |
|---|---|---|
|ResourceGroup [gerekli]| İptal edilecek siparişle ilişkili kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
|Ad [gerekli]| Silinecek siparişin adı. | "mydataboxorder"|
|Neden [gerekli]| Siparişi iptal etme nedeni. | "Hatalı bilgiler girdim ve siparişi iptal etmek için gerekli." |
|Force | Cmdlet 'ini Kullanıcı onayı olmadan çalışmaya zorlar. | -Force |

Çıktıyı içeren komuta bir örnek aşağıda verilmiştir:

```azurepowershell
PS C:\PowerShell\Modules> Stop-AzDataBoxJob -ResourceGroupName myResourceGroup \
                                            -Name "myDataBoxOrderPSTest" \
                                            -Reason "I entered erroneous information and had to cancel."
```

Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

```output
Confirm
"Cancelling Databox Job "myDataBoxOrderPSTest
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
PS C:\WINDOWS\system32>
```

### <a name="delete-an-order"></a>Bir siparişi silme

Azure Data Box sırayı iptal etmeniz durumunda, sırayı silmek için [Remove-AzDataBoxJob](https://docs.microsoft.com/powershell/module/az.databox/remove-azdataboxjob) komutunu çalıştırabilirsiniz.

```azurepowershell
Remove-AzDataBoxJob -Name <String> -ResourceGroup <String>
```

Aşağıdaki tabloda, için parametre bilgileri gösterilmektedir `Remove-AzDataBoxJob` :

| Parametre | Açıklama |  Örnek değer |
|---|---|---|
|ResourceGroup [gerekli]| Silinecek siparişle ilişkili kaynak grubunun adı. Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır. | myresourcegroup|
|Ad [gerekli]| Silinecek siparişin adı. | "mydataboxorder"|
|Force | Cmdlet 'ini Kullanıcı onayı olmadan çalışmaya zorlar. | -Force |

Çıktıyı içeren komuta bir örnek aşağıda verilmiştir:

```azurepowershell
PS C:\Windows> Remove-AzDataBoxJob -ResourceGroup "myresourcegroup" \
                                   -Name "mydataboxtest3"
```

Komutu çalıştırmanın çıktısı aşağıda verilmiştir:

```output
Confirm
"Removing Databox Job "mydataboxtest3
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
PS C:\Windows>
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şöyle Azure Data Box makaleleri öğrendiniz:

> [!div class="checklist"]
>
> * Data Box dağıtımı önkoşulları
> * Data Box sipariş etme
> * Siparişi izleme
> * Siparişi iptal etme

Data Box’ınızı ayarlama hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box’ınızı ayarlama](./data-box-deploy-set-up.md)
