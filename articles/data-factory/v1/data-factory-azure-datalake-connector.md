---
title: Azure Veri Gölü Depolama Gen1'e ve bu verilerden kopyalama
description: Azure Veri Fabrikası'nı kullanarak Veri Gölü Deposu'na veri kopyalama yı ve bunları nasıl kopyalayacağım
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: b6a60536bae6fbedf01eda7aa340e90ced58e004
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281606"
---
# <a name="copy-data-to-and-from-data-lake-storage-gen1-by-using-data-factory"></a>Veri Fabrikası'nı kullanarak Verileri Veri Gölü Depolama Gen1'e ve
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-datalake-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-data-lake-store.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure Veri Gölü Depolama Gen1 bağlayıcısına](../connector-azure-data-lake-store.md)bakın.

Bu makalede, verileri Azure Veri Gölü Deposu Gen1'e (daha önce Azure Veri Gölü Deposu olarak da bilinir) taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. [Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesi, Kopyalama Etkinliği ile veri hareketine genel bir bakış üzerine inşa edin.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Azure Veri **Gölü Deposu'ndaki** verileri aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri mağazalarından Azure **Veri Gölü Deposu'na**veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Kopyalama Etkinliği ile bir ardışık hatlar oluşturmadan önce bir Veri Gölü Deposu hesabı oluşturun. Daha fazla bilgi için bkz: [Azure Veri Gölü Deposu'na başlayın.](../../data-lake-store/data-lake-store-get-started-portal.md)

## <a name="supported-authentication-types"></a>Desteklenen kimlik doğrulama türleri
Veri Gölü Deposu bağlayıcısı şu kimlik doğrulama türlerini destekler:
* Hizmet sorumlusu kimlik doğrulaması
* Kullanıcı kimlik bilgisi (OAuth) kimlik doğrulaması

Özellikle zamanlanmış bir veri kopyası için hizmet temel kimlik doğrulaması kullanmanızı öneririz. Kullanıcı kimlik bilgisi kimlik doğrulaması ile belirteç sona erme davranışı oluşabilir. Yapılandırma ayrıntıları için [Bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.

## <a name="get-started"></a>Kullanmaya başlayın
Farklı araçlar/API'ler kullanarak verileri Azure Veri Gölü Deposu'na/ondan aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Verileri kopyalamak için bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Kopya Sihirbazı'nı kullanarak bir ardışık yol oluşturma öğreticisi için [Bkz. Öğretici: Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluştur.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat lar içerebilir.
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir Azure blob deposundan bir Azure Veri Gölü Mağazası'na veri kopyalıyorsanız, Azure depolama hesabınızı ve Azure Veri Gölü mağazanızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturursunuz. Azure Veri Gölü Deposu'na özgü bağlantılı hizmet özellikleri için [bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda belirtilen örnekte, giriş verilerini içeren blob kapsayıcısını ve klasörünü belirtmek için bir veri kümesi oluşturursunuz. Ayrıca, veri gölü deposunda blob depolamadan kopyalanan verileri tutan klasör ve dosya yolunu belirtmek için başka bir veri kümesi oluşturursunuz. Azure Veri Gölü Deposu'na özgü veri kümesi özellikleri için [bkz.](#dataset-properties)
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** Daha önce bahsedilen örnekte, BlobSource'u kaynak olarak, AzureDataLakeStoreSink'i ise kopyalama etkinliği için lavabo olarak kullanırsınız. Benzer şekilde, Azure Veri Gölü Deposu'ndan Azure Blob Depolama'ya kopyalıyorsanız, kopyalama etkinliğinde AzureDataLakeStoreSource ve BlobSink'i kullanırsınız. Azure Veri Gölü Deposu'na özgü kopyalama etkinliği özellikleri için [kopyalama etkinlik özellikleri](#copy-activity-properties) bölümüne bakın. Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı yla ilgili ayrıntılar için, veri deponuzun önceki bölümündeki bağlantıyı tıklatın.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Azure Veri Gölü Deposu'na/Azure Veri Gölü Deposu'na/verilerinden veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples-for-copying-data-to-and-from-data-lake-store) bölümüne bakın.

Aşağıdaki bölümler, Veri Gölü Deposu'na özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Bağlantılı bir hizmet, bir veri deposuna veri fabrikasına bağlanır. Veri Gölü Deposu verilerinizi veri fabrikanıza bağlamak için **AzureDataLakeStore** türünde bağlantılı bir hizmet oluşturursunuz. Aşağıdaki tabloda, Data Lake Store bağlantılı hizmetlere özgü JSON öğeleri açıklanmaktadır. Hizmet sorumlusu ile kullanıcı kimlik bilgisi kimlik doğrulaması arasında seçim yapabilirsiniz.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **Türü** | Tür özelliği **AzureDataLakeStore**olarak ayarlanmalıdır. | Evet |
| **dataLakeStoreUri** | Azure Veri Gölü Deposu hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini `https://[accountname].azuredatalakestore.net/webhdfs/v1` `adl://[accountname].azuredatalakestore.net/`alır: veya . | Evet |
| **subscriptionId** | Data Lake Store hesabının ait olduğu Azure abonelik kimliği. | Lavabo için gerekli |
| **resourceGroupName** | Veri Gölü Deposu hesabının ait olduğu Azure kaynak grubu adı. | Lavabo için gerekli |

### <a name="service-principal-authentication-recommended"></a>Hizmet temel kimlik doğrulaması (önerilir)
Hizmet temel kimlik doğrulamasını kullanmak için, azure Etkin Dizin'e (Azure AD) bir uygulama varlığı kaydedin ve veri gölü deposuna erişim izni verin. Ayrıntılı adımlar için [hizmete kimlik doğrulamasına](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md)bakın. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:
* Uygulama Kimliği
* Uygulama anahtarı
* Kiracı Kimliği

> [!IMPORTANT]
> Azure Veri Gölü Deposu'nda hizmet ilkesine uygun izni aldığınızdan emin olun:
>- **Veri Gölü Deposu'nu kaynak olarak kullanmak için,** bir klasörün içeriğini listelemek ve kopyalamak için en azından **Read + Execute** veri erişim izni verin veya tek bir dosyayı kopyalamak için okuma izni verin. **Read** Hesap düzeyi erişim denetiminde gereklilik yok.
>- **Veri Gölü Deposu'nu lavabo olarak kullanmak için,** klasörde alt öğeler oluşturmak için en azından Veri Erişim Izni **Yaz + Yürüt'** e izin verin. Veri Fabrikası'nın Data Lake Store'un bölgesini algılamasına izin vermek için kopyalamayı güçlendirmek için Azure IR'yi kullanırsanız (hem kaynak hem de lavabo buluttadır), hesap erişim denetiminde (IAM) en az **Reader** rolü verir. Bu IAM rolünden kaçınmak istiyorsanız, kopyalama etkinliğinde Veri Gölü Mağazanızın konumuyla [birlikte yürütmeKonumu'nu belirtin.](data-factory-data-movement-activities.md#global)
>- **Ardışık hatlar yazar kopya sihirbazı kullanıyorsanız,** hesap erişim denetimi (IAM) en az **Reader** rolü verir. Ayrıca, Veri Gölü Deposu root ("/") ve çocuklarına en azından **Okuma + Yürüt** izni verin. Aksi takdirde "Sağlanan kimlik bilgileri geçersizdir" iletisini görebilirsiniz.

Aşağıdaki özellikleri belirterek hizmet temel kimlik doğrulamasını kullanın:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **hizmetPrincipalId** | Uygulamanın istemci kimliğini belirtin. | Evet |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin. | Evet |
| **Kiracı** | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet |

**Örnek: Hizmet temel kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgisi kimlik doğrulaması
Alternatif olarak, aşağıdaki özellikleri belirterek, kopyalamak için kullanıcı kimlik bilgilerini veya Data Lake Store'u kullanabilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **Yetkilendirme** | Veri Fabrikası Düzenleyicisi'ndeki **Yetkilendirme** düğmesini tıklatın ve bu özelliğe otomatik olarak oluşturulmuş yetkilendirme URL'sini atayan kimlik bilgilerinizi girin. | Evet |
| **Sessionıd** | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersizdir ve yalnızca bir kez kullanılabilir. Veri Fabrikası Düzenleyicisi'ni kullandığınızda bu ayar otomatik olarak oluşturulur. | Evet |

> [!IMPORTANT]
> Azure Veri Gölü Deposu'nda kullanıcıya uygun izni aldığınızdan emin olun:
>- **Veri Gölü Deposu'nu kaynak olarak kullanmak için,** bir klasörün içeriğini listelemek ve kopyalamak için en azından **Read + Execute** veri erişim izni verin veya tek bir dosyayı kopyalamak için okuma izni verin. **Read** Hesap düzeyi erişim denetiminde gereklilik yok.
>- **Veri Gölü Deposu'nu lavabo olarak kullanmak için,** klasörde alt öğeler oluşturmak için en azından Veri Erişim Izni **Yaz + Yürüt'** e izin verin. Veri Fabrikası'nın Data Lake Store'un bölgesini algılamasına izin vermek için kopyalamayı güçlendirmek için Azure IR'yi kullanırsanız (hem kaynak hem de lavabo buluttadır), hesap erişim denetiminde (IAM) en az **Reader** rolü verir. Bu IAM rolünden kaçınmak istiyorsanız, kopyalama etkinliğinde Veri Gölü Mağazanızın konumuyla [birlikte yürütmeKonumu'nu belirtin.](data-factory-data-movement-activities.md#global)
>- **Ardışık hatlar yazar kopya sihirbazı kullanıyorsanız,** hesap erişim denetimi (IAM) en az **Reader** rolü verir. Ayrıca, Veri Gölü Deposu root ("/") ve çocuklarına en azından **Okuma + Yürüt** izni verin. Aksi takdirde "Sağlanan kimlik bilgileri geçersizdir" iletisini görebilirsiniz.

**Örnek: Kullanıcı kimlik bilgisi kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Belirteç sona ermesi
**Yetkilendirme** düğmesini kullanarak oluşturduğunuz yetkilendirme kodunun süresi belirli bir süre sonra sona erer. Aşağıdaki ileti, kimlik doğrulama belirteci süresinin dolduğu anlamına gelir:

Kimlik doğrulama hatası: invalid_grant - AADSTS70002: Kimlik bilgilerini doğrulayan hata. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İz KIMLIĞI: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korelasyon Id: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21-09-31Z.

Aşağıdaki tablo, farklı kullanıcı hesabı türlerinin son kullanma sürelerini gösterir:

| Kullanıcı türü | Sonra sona erer |
|:--- |:--- |
| Azure Etkin Dizintarafından *yönetilmeyen* kullanıcı @hotmail.com hesapları @live.com(örneğin, veya) |12 saat |
| Azure Active Directory tarafından yönetilen kullanıcı hesapları |Son dilim çalışmasından 14 gün sonra <br/><br/>OAuth tabanlı bağlantılı bir hizmete dayalı bir dilim en az 14 günde bir çalışıyorsa, 90 gün |

Parolanızı son kullanma tarihinden önce değiştirirseniz, belirteç hemen sona erer. Bu bölümde daha önce bahsedilen iletiyi görürsünüz.

Belirteç bağlantılı hizmeti yeniden dağıtmak için süresi dolduğunda **Yetkilendirme** düğmesini kullanarak hesabı yeniden yetkilendirebilirsiniz. Ayrıca aşağıdaki kodu kullanarak **programlı sessionId** ve **yetkilendirme** özellikleri için değerler oluşturabilirsiniz:


```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
Kodda kullanılan Veri Fabrikası sınıfları hakkında ayrıntılı bilgi için [AzureDataLakeStoreBağlantılıHizmet Sınıfı,](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx) [AzureDataLakeAnalyticsLinkedService Sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)ve [YetkilendirmeSessionGetResponse Sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) konularına bakın. Kodda kullanılan `2.9.10826.1824` `WindowsFormsWebAuthenticationDialog` sınıf `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` için sürüme bir başvuru ekleyin.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları

**Belirti:** Verileri Azure Veri Gölü **Deposu'na** kopyalarken, kopyalama etkinliğiniz aşağıdaki hatayla başarısız olursa:

  ```
  Failed to detect the region for Azure Data Lake account {your account name}. Please make sure that the Resource Group name: {resource group name} and subscription ID: {subscription ID} of this Azure Data Lake Store resource are correct.
  ```

**Kök nedeni:** Bunun 2 olası nedeni vardır:

1. Azure `resourceGroupName` Veri `subscriptionId` Gölü Deposu'na bağlı hizmette belirtilen ve/veya belirtilen ler yanlıştır;
2. Kullanıcı nın veya hizmet sorumlusunun gerekli izni yoktur.

**Çözünürlük:**

1. Bağlantılı `subscriptionId` serviste `resourceGroupName` `typeProperties` belirttiğiniz ve belirttiğiniz şeyin veri gölü hesabınızın gerçekten ait olduğu kişiler olduğundan emin olun.

2. Veri gölü hesabındaki kullanıcıya veya hizmet sorumlusuna en az **Reader** rolü verdiğinden emin olun. Bunu nasıl yapacağınız aşağıda açıklanmıştır:

    1. Azure portalına gidin -Data Lake Store hesabınıza >
    2. Veri Gölü Deposu'nun bıçak üzerinde **Erişim denetimine (IAM)** tıklayın
    3. **Rol Atama ekle'yi** tıklatın
    4. **Rolü** **Okuyucu**olarak ayarlayın ve erişim sağlamak için kopya lamak için kullandığınız kullanıcıyı veya hizmet müdürünü seçin

3. **Okuyucu** rolünü kullanıcıya veya hizmet ilkesine vermek istemiyorsanız, alternatif olarak Veri Gölü Mağazanızın konumuyla birlikte kopyalama etkinliğinde [bir yürütme konumu açıkça belirtmek](data-factory-data-movement-activities.md#global) tir. Örnek:

    ```json
    {
      "name": "CopyToADLS",
      "type": "Copy",
      ......
      "typeProperties": {
        "source": {
          "type": "<source type>"
        },
        "sink": {
          "type": "AzureDataLakeStoreSink"
        },
        "exeuctionLocation": "West US"
      }
    }
    ```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri Gölü Deposu'ndaki giriş verilerini temsil edecek bir veri kümesi belirtmek için, veri kümesinin **tür** özelliğini **AzureDataLakeStore**olarak ayarlarsınız. Veri kümesinin **linkedServiceName** özelliğini Data Lake Store bağlantılı hizmetin adına ayarlayın. Veri kümelerini tanımlamak için kullanılabilen JSON bölümlerinin ve özelliklerinin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. JSON'daki **yapı,** **kullanılabilirlik**ve **ilke**gibi bir veri kümesinin bölümleri tüm veri kümesi türleri için benzerdir (örneğin Azure SQL veritabanı, Azure blob ve Azure tablosu). **typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu ve biçimi gibi bilgiler sağlar.

**AzureDataLakeStore** türündeki bir veri kümesinin **typeProperties** bölümü aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **folderPath** |Veri Gölü Deposu'ndaki kapsayıcı ve klasöre giden yol. |Evet |
| **Dosyaadı** |Azure Veri Gölü Deposu'ndaki dosyanın adı. **fileName** özelliği isteğe bağlıdır ve büyük/küçük harf duyarlıdır. <br/><br/>**fileName**belirtirseniz, etkinlik (Kopya dahil) belirli bir dosya üzerinde çalışır.<br/><br/>**fileName** belirtilmediğinde, Copy giriş veri kümesindeki **folderPath'teki** tüm dosyaları içerir.<br/><br/>Bir çıktı veri kümesi için **dosya Adı** belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, oluşturulan dosyanın adı biçimindedir. `Data._Guid_.txt` Örneğin: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Hayır |
| **bölümlemeBy** |**BölümlemeBy** özelliği isteğe bağlıdır. Zaman serisi verileri için dinamik bir yol ve dosya adı belirtmek için kullanabilirsiniz. Örneğin, **folderPath** her saatlik veri için parametreli olabilir. Ayrıntılar ve örnekler için bölümlenmişBy özelliğine bakın. |Hayır |
| **Biçim** | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParkquetFormat**. **Biçim** altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için Dosya'daki [Metin biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [JSON biçimi,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [ORC biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine ve Azure Veri Fabrikası makalesi tarafından desteklenen sıkıştırma [biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında "olduğu gibi" `format` kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında bölümü atlayın. |Hayır |
| **Sıkıştırma** | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri **GZip,** **Deflate**, **BZip2**, ve **ZipDeflate**vardır. Desteklenen seviyeler **Optimal** ve **En Hızlı'dır.** Daha fazla bilgi için Azure [Veri Fabrikası tarafından desteklenen Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

### <a name="the-partitionedby-property"></a>BölümlenmişBy özelliği
**Bölümleme Özelliği,** Veri Fabrikası işlevleri ve sistem değişkenleri ile zaman serisi verileri için dinamik **klasörYol** ve **fileName** özelliklerini belirtebilirsiniz. Ayrıntılar için [Azure Veri Fabrikası - işlevler ve sistem değişkenleri](data-factory-functions-variables.md) makalesine bakın.


Aşağıdaki örnekte, `{Slice}` veri fabrikası sistem değişkeninin `SliceStart` değeri belirtilen biçimde`yyyyMMddHH`değiştirilir ( ). Ad, `SliceStart` dilimin başlangıç saatini ifade eder. Özellik `folderPath` her dilim için farklı, `wikidatagateway/wikisampledataout/2014100103` `wikidatagateway/wikisampledataout/2014100104`gibi ya da .

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Aşağıdaki örnekte, yıl, ay, gün ve `SliceStart` saat, ve `folderPath` `fileName` özellikleri tarafından kullanılan ayrı değişkenler halinde ayıklanır:
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Zaman serisi veri kümeleri, zamanlama ve dilimler hakkında daha fazla bilgi için Azure Veri Fabrikası ve [Veri Fabrikası zamanlama ve yürütme](data-factory-scheduling-and-execution.md) [makalelerinde Veri kümelerine](data-factory-create-datasets.md) bakın.


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, ardışık hatlar oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Bir etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişirler.

**AzureDataLakeStoreSource,** **typeProperties** bölümünde aşağıdaki özelliği destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **Özyinelemeli** |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True (varsayılan değer), False |Hayır |

**AzureDataLakeStoreSink,** **typeProperties** bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **copyBehavior** |Kopyalama davranışını belirtir. |<b>Koruma Hiyerarşisi</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><br/><b>FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyinde oluşturulur. Hedef dosyalar otomatik olarak oluşturulan adlarla oluşturulur.<br/><br/><b>Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya veya blob adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, dosya adı otomatik olarak oluşturulur. |Hayır |

### <a name="recursive-and-copybehavior-examples"></a>özyinelemeli ve copyBehavior örnekleri
Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için Kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| Özyinelemeli | copyBehavior | Sonuç davranış |
| --- | --- | --- |
| true |korumaHiyerarşisi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 kaynak la aynı yapıile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5. |
| true |flattenHiyerarşi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef Folder1 aşağıdaki yapı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |birleştirmeDosyaları |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef Folder1 aşağıdaki yapı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + Dosya 5 içeriği otomatik oluşturulan dosya adı ile bir dosya da birleştirilir |
| yanlış |korumaHiyerarşisi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulan ad<br/><br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları |Aşağıdaki yapıya sahip bir kaynak klasör Folder1 için:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5<br/><br/>hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulan dosya adı içeren bir dosyada birleştirilir. File1 için otomatik olarak oluşturulan ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

## <a name="supported-file-and-compression-formats"></a>Desteklenen dosya ve sıkıştırma biçimleri
Ayrıntılar için [Azure Veri Fabrikası makalesinde Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md) bakın.

## <a name="json-examples-for-copying-data-to-and-from-data-lake-store"></a>Data Lake Store'a ve Data Lake Store'dan veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler örnek JSON tanımlarını sağlar. [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell'i](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir boru hattı oluşturmak için bu örnek tanımları kullanabilirsiniz. Örnekler, Data Lake Store ve Azure Blob depolama alanına veri kopyalamanın nasıl yapılacağını gösterir. Ancak, veriler _doğrudan_ kaynaklardan herhangi biri tarafından desteklenen lavabolara kopyalanabilir. Daha fazla bilgi için, [Etkinlik Kopyala makalesini kullanarak Taşı verilerindeki](data-factory-data-movement-activities.md) "Desteklenen veri depoları ve biçimleri" bölümüne bakın.

### <a name="example-copy-data-from-azure-blob-storage-to-azure-data-lake-store"></a>Örnek: Azure Blob Depolama'dan Azure Veri Gölü Deposu'na veri kopyalama
Bu bölümdeki örnek kod şunları gösterir:

* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [AzureDataLakeStore](#linked-service-properties)türünde bağlantılı bir hizmet.
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureDataLakeStore](#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [AzureDataLakeStoreSink](#copy-activity-properties)kullanan bir kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnekler, Azure Blob Depolama'daki zaman serileri verilerinin her saat başı Data Lake Store'a nasıl kopyalandığını gösterir.

**Azure Depolama bağlantılı hizmet**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Azure Veri Gölü Deposu bağlantılı hizmet**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> Yapılandırma ayrıntıları için [Bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
>

**Azure Blob girdi veri kümesi**

Aşağıdaki örnekte, veriler her saat yeni bir damladan alınır (`"frequency": "Hour", "interval": 1`). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün bölümünü kullanır. Dosya adı, başlangıç zamanının saat bölümünü kullanır. Ayar, `"external": true` Veri Fabrikası hizmetine tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ]
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure Veri Gölü Deposu çıktı veri kümesi**

Aşağıdaki örnek verileri Data Lake Store'a kopyalar. Yeni veriler her saat başı Data Lake Store'a kopyalanır.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Blob kaynağı ve Veri Gölü Deposu lavabosu olan bir ardışık ardışık alanda etkinliği kopyalama**

Aşağıdaki örnekte, ardışık yapı, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan bir kopyalama etkinliği içerir. Kopyalama etkinliği her saat çalışacak şekilde zamanlanır. JSON ardışık ardışık `source` tanımda, `BlobSource`tür `sink` , ve `AzureDataLakeStoreSink`türü olarak ayarlanır.

```json
{
    "name":"SamplePipeline",
    "properties":
    {
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [
            {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureDataLakeStoreOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

### <a name="example-copy-data-from-azure-data-lake-store-to-an-azure-blob"></a>Örnek: Azure Veri Gölü Deposu'ndan azure blob'una veri kopyalama
Bu bölümdeki örnek kod şunları gösterir:

* [AzureDataLakeStore](#linked-service-properties)türünde bağlantılı bir hizmet.
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
* [AzureDataLakeStore](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
* [AzureDataLakeStoreSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan bir kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Kod, Data Lake Store'dan her saat başı bir Azure blob'una zaman serisi verilerini kopyalar.

**Azure Veri Gölü Deposu bağlantılı hizmet**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> Yapılandırma ayrıntıları için [Bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
>

**Azure Depolama bağlantılı hizmet**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Veri Gölü giriş veri kümesi**

Bu örnekte, `"external"` `true` tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirme ayarı.

```json
{
    "name": "AzureDataLakeStoreInput",
    "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
**Azure blob çıktı veri seti**

Aşağıdaki örnekte, veriler her saat yeni bir`"frequency": "Hour", "interval": 1`blob () yazılır. Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümünü kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Azure Veri Gölü Deposu kaynağı ve blob lavabosu olan bir ardışık ardışık ardışık ardışık alanda kopyalama etkinliği**

Aşağıdaki örnekte, ardışık yapı, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan bir kopyalama etkinliği içerir. Kopyalama etkinliği her saat çalışacak şekilde zamanlanır. JSON ardışık ardışık `source` tanımda, `AzureDataLakeStoreSource`tür `sink` , ve `BlobSink`türü olarak ayarlanır.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureDataLakeStoreInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

Kopyalama etkinliği tanımında, kaynak veri kümesinden lavabo veri kümesindeki sütunlara kadar sütunları da eşleyebilirsiniz. Ayrıntılar için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)bilgisine bakın.

## <a name="performance-and-tuning"></a>Performans ve ayar
Kopyalama Etkinliği performansını etkileyen etkenler ve onu nasıl optimize edeceğinizi öğrenmek için [Kopyalama Etkinliği performansı ve atokskılavuzu](data-factory-copy-activity-performance.md) makalesine bakın.
