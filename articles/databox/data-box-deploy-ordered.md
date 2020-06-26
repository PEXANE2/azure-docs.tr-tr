---
title: Sipariş Azure Data Box öğretici | Microsoft Docs
description: Dağıtım önkoşullarını ve Azure Data Box siparişi etmeyi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: cfb95f2fb02544197f9b2796a705844e33eca201
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85392497"
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

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

Azure 'da oturum açabilir ve Azure CLı komutlarını iki şekilde çalıştırabilirsiniz:

* CLı 'yı yükleyebilir ve CLı komutlarını yerel olarak çalıştırabilirsiniz.
* CLı komutlarını Azure Cloud Shell Azure portal içinden çalıştırabilirsiniz.

Öğretici için Windows PowerShell aracılığıyla Azure CLı kullanıyoruz, ancak her iki seçenekten birini de seçebilirsiniz.

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) sürüm 2.0.67 veya üstünü yükler. Alternatif olarak, [MSI kullanarak yükleyebilirsiniz](https://aka.ms/installazurecliwindows).

#### <a name="sign-in-to-azure"></a>Azure'da oturum açma

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

#### <a name="install-the-azure-data-box-cli-extension"></a>Azure Data Box CLı uzantısını yükler

Azure Data Box CLı komutlarını kullanabilmeniz için, uzantıyı yüklemeniz gerekir. Azure CLı uzantıları, henüz çekirdek CLı 'nin bir parçası olarak gönderilmeyen deneysel ve yayın öncesi komutlara erişmenizi sağlar. Uzantılar hakkında daha fazla bilgi için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).

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

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

CLı komutlarını çalıştırmak için tarayıcınız aracılığıyla Azure 'da barındırılan etkileşimli kabuk ortamı [Azure Cloud Shell](https://shell.azure.com/)kullanabilirsiniz. Azure Cloud Shell, Azure hizmetleriyle bash veya Windows PowerShell 'i destekler. Azure CLı, önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış. Azure portal sağ üst kısmındaki menüdeki Cloud Shell düğmesine tıklayın:

![Cloud Shell](../storage/common/media/storage-quickstart-create-account/cloud-shell-menu.png)

Düğme, bu nasıl yapılır makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır.

<!-- To start Azure Cloud Shell:

| Option | Example/Link |
|-----------------------------------------------|---|
| Select **Try It** in the upper-right corner of a code block. Selecting **Try It** doesn't automatically copy the code to Cloud Shell. | ![Example of Try It for Azure Cloud Shell](../../includes/media/cloud-shell-try-it/hdi-azure-cli-try-it.png) |
| Go to [https://shell.azure.com](https://shell.azure.com), or select the **Launch Cloud Shell** button to open Cloud Shell in your browser. | [![Launch Cloud Shell in a new window](../../includes/media/cloud-shell-try-it/hdi-launch-cloud-shell.png)](https://shell.azure.com) |
| Select the **Cloud Shell** button on the menu bar at the upper right in the [Azure portal](https://portal.azure.com). | ![Cloud Shell button in the Azure portal](../../includes/media/cloud-shell-try-it/hdi-cloud-shell-menu.png) |

To run the code in this article in Azure Cloud Shell:

1. Start Cloud Shell.

2. Select the **Copy** button on a code block to copy the code.

3. Paste the code into the Cloud Shell session by selecting **Ctrl**+**Shift**+**V** on Windows and Linux or by selecting **Cmd**+**Shift**+**V** on macOS.

4. Select **Enter** to run the code.

For this tutorial, we use Windows PowerShell command prompt to run Azure CLI commands. -->

<!-- This goes away, we'll show this later when we show how to order a Data Box. -->
<!-- ## Change the output format type

All Azure CLI commands will use json as the output format by default unless you change it. You can change the output format by using the global parameter `--output <output-format>`. -->

<!-- ```azurecli

az databox job <command> --output <output-format>

```

Azure Data Box CLI commands support the following output formats:

* json (default setting)
* jsonc
* table
* tsv
* yaml
* yamlc
* none

You can use the parameter `--output` with all Azure Data Box CLI commands. -->

<!-- To set the output format to yaml: -->

<!-- ```azurecli
PS C:\Windows>az databox job show --resource-group "myresourcegroup" --name "mydataboxorder" --output "yaml"

``` -->
<!-- 
To set the out format to tabular form (easier to read):

```azurecli
PS C:\Windows>az databox job show --resource-group "myresourcegroup" --name "mydataboxorder" --output "table"

``` -->

<!-- Here's the example output of `az databox job show` after changing the output format to table:

```azurecli
PS C:\WINDOWS\system32> az databox job show --resource-group "GDPTest" --name "mydataboxtest3" --output "table"
Command group 'databox job' is experimental and not covered by customer support. Please use with discretion.

DeliveryType    IsCancellable    IsCancellableWithoutFee    IsDeletable    IsShippingAddressEditable    Location    Name            ResourceGroup    StartTime                         Status
--------------  ---------------  -------------------------  -------------  ---------------------------  ----------  --------------  ---------------  --------------------------------  -------------
NonScheduled    True             True                       False          True                         westus      mydataboxorder  myresourcegroup          2020-06-11T22:05:49.436622+00:00  DeviceOrdered

``` -->

---

## <a name="order-data-box"></a>Data Box sipariş etme

# <a name="portal"></a>[Portal](#tab/portal)

Bir cihazı sıralamak için Azure portal aşağıdaki adımları uygulayın.

1. Microsoft Azure kimlik bilgilerini kullanarak şu URL’de oturum açın: [https://portal.azure.com](https://portal.azure.com).
2. **+ Kaynak oluştur**’a tıklayın ve *Azure Data Box* araması yapın. **Azure Data Box**'a tıklayın.

   [![Azure Data Box arama 1](media/data-box-deploy-ordered/search-azure-data-box1.png)](media/data-box-deploy-ordered/search-azure-data-box1.png#lightbox)

3. **Oluştur**'a tıklayın.

4. Data Box'ın bölgenizde kullanılabilir olup olmadığını kontrol edin. Aşağıdaki bilgileri girin veya seçin ve sonra **Uygula**'yı seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Abonelik     | Data Box hizmeti için bir EA, CSP veya Azure sponsorluk aboneliği seçin. <br> Abonelik fatura hesabınıza bağlıdır.       |
    |Aktarım türü     | **Azure’a içeri aktar**’ı seçin.        |
    |Kaynak ülke/bölge    |    Verilerinizin bulunduğu ülkeyi/bölgeyi seçin.         |
    |Hedef Azure bölgesi     |     Verileri aktarmak istediğiniz Azure bölgesini seçin.        |

5. **Data Box**'ı seçin. Tek bir sipariş için kullanılabilir maksimum kapasite 80 TB 'tır. Daha büyük veri boyutları için birden fazla sipariş oluşturabilirsiniz.

      [![Data Box seçenek 1 ' i seçin](media/data-box-deploy-ordered/select-data-box-option1.png)](media/data-box-deploy-ordered/select-data-box-option1.png#lightbox)

6. **Sipariş** bölümünde **Sipariş ayrıntıları**'nı belirtin. Aşağıdaki bilgileri girin veya seçin ve sonra **İleri**'yi seçin.

    |Ayar  |Değer  |
    |---------|---------|
    |Adı     |  Siparişi takip etmek için kullanılacak kolay bir ad girin. <br> Ad harf, rakam ve tirelerden oluşan 3-24 karakter arası uzunlukta olabilir. <br> Ad bir harf veya sayıyla başlamalı ve bitmelidir.      |
    |Kaynak grubu     |    Var olan bir taneyi kullanın veya yenisini oluşturun. <br> Kaynak grubu, birlikte yönetilebilen ve ya dağıtılabilen kaynaklardan oluşan mantıksal kapsayıcıdır.         |
    |Hedef Azure bölgesi     | Depolama hesabınız için bir bölge seçin. <br> Daha fazla bilgi için [bölge kullanılabilirliği](data-box-overview.md#region-availability)’ne gidin.        |
    |Depolama hedefi     | Depolama hesabı veya yönetilen diskler ya da her ikisini seçin. <br> Belirtilen Azure bölgesine göre filtrelenen listeden var olan bir veya daha fazla depolama hesabı seçin. Data Box en çok 10 depolama hesabına bağlanabilir. <br> Dilerseniz yeni bir **Genel amaçlı v1**, **Genel amaçlı v2** veya **Blob depolama hesabı** da oluşturabilirsiniz. <br>Sanal ağları olan depolama hesapları desteklenir. Data Box hizmetinin güvenli depolama hesaplarıyla çalışmasına izin vermek için, depolama hesabı ağ güvenlik duvarı ayarları içinden güvenilen hizmetleri etkinleştirin. Daha fazla bilgi için bkz. [Azure Data Box güvenilir hizmet olarak ekleme](../storage/common/storage-network-security.md#exceptions).|

    Depolama hesabı depolama hedefi olarak kullanılıyorsa aşağıdaki ekran görüntüsünü görürsünüz:

    ![Depolama hesabı için Data Box sıralaması](media/data-box-deploy-ordered/order-storage-account.png)

    Şirket içi sanal sabit disklerde (VHD) yönetilen diskler oluşturmak için Data Box kullanıyorsanız, aşağıdaki bilgileri de sağlamanız gerekir:

    |Ayar  |Değer  |
    |---------|---------|
    |Kaynak grupları     | Şirket içi VHD'lerden yönetilen diskler oluşturmayı düşünüyorsanız yeni kaynak grupları oluşturun. Mevcut bir kaynak grubunu yalnızca kaynak grubu, Data Box hizmeti tarafından yönetilen disk için bir Data Box sırası oluşturulurken daha önce oluşturulduysa kullanabilirsiniz. <br> Noktalı virgülle ayrılmış birden çok kaynak grubu belirtin. En fazla 10 kaynak grubu desteklenir.|

    ![Yönetilen disk için Data Box sıralaması](media/data-box-deploy-ordered/order-managed-disks.png)

    Yönetilen diskler için belirtilen depolama hesabı, hazırlama depolama hesabı olarak kullanılır. Data Box hizmeti, VHD'leri yönetilen disklere dönüştürmeden ve kaynak gruplarına taşımadan önce hazırlama depolama hesabına sayfa blobları olarak yükler. Daha fazla bilgi için bkz. [Azure'a veri yüklemeyi doğrulama](data-box-deploy-picked-up.md#verify-data-upload-to-azure).

7. **Teslimat adresi**’ne adınızı, soyadınızı, şirket adını, posta adresini ve geçerli bir telefon numarasını girin. **Adresi doğrula**'ya tıklayın. Hizmet, teslimat adresinde hizmetin kullanılabilirlik durumunu doğrular. Hizmet belirtilen teslimat adresinde kullanılabilir durumdaysa bu konuda bir bildirim gönderilir.

   Kendi kendine yönetilen sevkiyat ' ı seçtiyseniz, sipariş başarıyla yerleştirildikten sonra bir e-posta bildirimi alırsınız. Kendi kendine yönetilen kargo hakkında daha fazla bilgi için bkz. [otomatik olarak yönetilen gönderi kullanma](data-box-portal-customer-managed-shipping.md).

8. Gönderim ayrıntıları başarıyla doğrulandıktan sonra **İleri** ' ye tıklayın.

9. **Bildirim ayrıntıları** sayfasında e-posta adresi belirtin. Hizmet, belirtilen e-posta adreslerine sipariş durumundaki güncelleştirmelerle ilgili bilgi gönderir.

    Grup yöneticisinin ayrılması durumunda da bildirim almaya devam etmek için bir grup e-postası kullanmanız önerilir.

10. **Özet** sayfasında sipariş, iletişim, bildirim ve gizlilik koşullarıyla ilgili bilgileri gözden geçirin. Gizlilik koşullarını kabul ettiğinizi belirten kutuyu işaretleyin.

11. **Sipariş**'e tıklayın. Siparişin oluşturulması birkaç dakika sürer.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir cihaz sıralamak için Azure CLı kullanarak aşağıdaki adımları uygulayın:

1. Data Box siparişiniz için ayarlarınızı yazın. Bu ayarlar kişisel/iş bilgilerinizi, abonelik adınızı, cihaz bilgilerini ve gönderi bilgilerini içerir. Data Box sırasını oluşturmak için CLı komutunu çalıştırırken bu ayarları parametre olarak kullanmanız gerekir. Aşağıdaki tabloda, için kullanılan parametre ayarları gösterilmektedir `az databox job create` :

   | Ayar (parametre) | Açıklama |  Örnek değer |
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
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu<string>|
   |ayrıntılı| Ayrıntılı günlük kaydı ekleyin. | --ayrıntılı |

2. Tercih ettiğiniz veya terminalin komut isteminde, Azure Data Box siparişinizi oluşturmak için [az databox Job Create](https://docs.microsoft.com/cli/azure/ext/databox/databox/job?view=azure-cli-latest#ext-databox-az-databox-job-create) komutunu kullanın.

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
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu<string>|
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
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu<string>|
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
   OtherReason This was a test order for documentation purposes.    NonScheduled    False            False                      True           False                        westus      gdpImportTest        GDPTest          2020-05-26T23:20:57.464075+00:00  Cancelled
   NoLongerNeeded This order was created for documentation purposes.NonScheduled    False            False                      True           False                        westus      mydataboxExportTest  GDPTest          2020-05-27T00:04:16.640397+00:00  Cancelled
   IncorrectOrder                                                   NonScheduled    False            False                      True           False                        westus      mydataboxtest2       GDPTest          2020-06-10T16:54:23.509181+00:00  Cancelled
                                                                    NonScheduled    True             True                       False          True                         westus      mydataboxtest3       GDPTest          2020-06-11T22:05:49.436622+00:00  DeviceOrdered
                                                                    NonScheduled    True             True                       False          True                         westus      mydataboxtest4       GDPTest          2020-06-18T03:48:00.905893+00:00  DeviceOrdered
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
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu<string>|
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
   |sorgu| JMESPath sorgu dizesi. Daha fazla bilgi için bkz. [Jmespath](http://jmespath.org/). | --sorgu<string>|
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
