---
title: .NET SDK'yı kullanarak DNS bölgeleri ve kayıt kümeleri oluşturma
titleSuffix: Azure DNS
description: Bu öğrenme yolunda, .NET SDK'yı kullanarak Azure DNS'de DNS bölgeleri ve kayıt kümeleri oluşturmaya başlayın.
services: dns
documentationcenter: na
author: rohinkoul
manager: kumudD
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: rohink
ms.openlocfilehash: c497209e456ff838786edaa19e46ebc5c1858d5f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938857"
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>.NET SDK'yı kullanarak DNS bölgeleri ve kayıt kümeleri oluşturma

.NET DNS Management kitaplığı yla DNS SDK'yı kullanarak DNS bölgeleri, kayıt kümeleri ve kayıtları oluşturmak, silmek veya güncelleştirmek için işlemleri otomatikleştirebilirsiniz. Tam bir Visual Studio projesi [burada mevcuttur.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Hizmet ana hesabı oluşturma

Genellikle, Azure kaynaklarına programlı erişim, kendi kullanıcı kimlik bilgileriniz yerine özel bir hesap aracılığıyla verilir. Bu özel hesaplara 'hizmet sorumlusu' hesapları denir. Azure DNS SDK örnek projesini kullanmak için öncelikle bir hizmet ana hesabı oluşturmanız ve doğru izinleri atabilmeniz gerekir.

1. Bir hizmet temel hesabı oluşturmak için [bu yönergeleri](../active-directory/develop/howto-authenticate-service-principal-powershell.md) izleyin (Azure DNS SDK örnek projesi parola tabanlı kimlik doğrulaması varsayar.)
2. Bir kaynak grubu oluşturun[(şu şekilde).](../azure-resource-manager/templates/deploy-portal.md)
3. Hizmet ana hesabı 'DNS Zone Katılımcısı' izinlerini kaynak grubuna vermek için Azure RBAC'ı kullanın[(şu şekilde .)](../role-based-access-control/role-assignments-portal.md)
4. Azure DNS SDK örnek projesini kullanıyorsanız, 'program.cs' dosyasını aşağıdaki gibi düzenlediyseniz:

   * 1. adımda kullanılan `tenantId` `clientId` , (hesap kimliği `secret` olarak da bilinir), `subscriptionId` (hizmet anapara hesap parolası) için doğru değerleri ekleyin.
   * Adım 2'de seçilen kaynak grubu adını girin.
   * Seçtiğiniz bir DNS bölge adını girin.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet paketleri ve namespace bildirimleri

Azure DNS .NET SDK'yı kullanmak için **Azure DNS Yönetim Kitaplığı** NuGet paketini ve diğer gerekli Azure paketlerini yüklemeniz gerekir.

1. **Visual Studio'da**bir proje veya yeni bir proje açın.
2. **Araçlara** **>** gidin **NuGet Paket Yöneticisi** **>** **Çözüm için NuGet Paketlerini Yönetin...**.
3. **Gözat'ı**tıklatın, **ön sürüm ön** kutusunu ekle'yi etkinleştirin ve arama kutusuna **Microsoft.Azure.Management.Dns** yazın.
4. Paketi seçin ve Visual Studio projenize eklemek için **Yükle'yi** tıklatın.
5. Aşağıdaki paketleri yüklemek için yukarıdaki işlemi yineleyin: **Microsoft.Rest.ClientRuntime.Azure.Authentication** ve **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Ad alanı bildirimleri ekleme

Aşağıdaki ad alanı bildirimleri ekleme

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a>DNS yönetim istemcisini başlatma

DNS `DnsManagementClient` bölgelerini ve kayıt kümelerini yönetmek için gerekli yöntemleri ve özellikleri içerir.  Aşağıdaki kod hizmet ana hesabına giriş ve `DnsManagementClient` bir nesne oluşturur.

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Bir DNS bölgesi oluşturma veya güncelleştirme

Bir DNS bölgesi oluşturmak için, önce DNS bölge parametrelerini içerecek bir "Bölge" nesnesi oluşturulur. DNS bölgeleri belirli bir bölgeye bağlı olmadığından, konum 'genel' olarak ayarlanır. Bu örnekte, bölgeye bir [Azure Kaynak Yöneticisi 'etiketi'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) de eklenir.

Azure DNS'de bölgeyi gerçekten oluşturmak veya güncelleştirmek için, bölge `DnsManagementClient.Zones.CreateOrUpdateAsyc` parametrelerini içeren bölge nesnesi yönteme geçirilir.

> [!NOTE]
> DnsManagementClient üç çalışma modunu destekler: senkron ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), veya HTTP yanıtına erişim eki olan asenkron ('CreateOrUpdateWithHttpMessagesAsync').  Uygulama gereksinimlerinize bağlı olarak bu modlardan herhangi birini seçebilirsiniz.

Azure DNS, [Etags](dns-getstarted-create-dnszone.md)adı verilen iyimser eşzamanlılığı destekler. Bu örnekte, 'If-None-Match' üstbilgisi için "*" belirtilmesi, Azure DNS'ye zaten yoksa bir DNS bölgesi oluşturmasını söyler.  Verilen kaynak grubunda zaten ad verilen bir bölge varsa arama başarısız olur.

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create an Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create the actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a>DNS kayıt kümeleri ve kayıtları oluşturma

DNS kayıtları bir kayıt kümesi olarak yönetilir. Kayıt kümesi, bir bölge içinde aynı ada ve kayıt türüne sahip bir kayıt kümesidir.  Kayıt kümesi adı, tam nitelikli DNS adı değil, bölge adı ile görelidir.

Bir kayıt kümesi oluşturmak veya güncelleştirmek için bir "RecordSet" parametreleri nesnesi oluşturulur ve .'ye `DnsManagementClient.RecordSets.CreateOrUpdateAsync`geçirilir. DNS bölgelerinde olduğu gibi, üç çalışma modu vardır: senkron ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), veya HTTP yanıtına erişim eki olan eşzamanlı ('CreateOrUpdateWithHttpMessagesAsync').

DNS bölgelerinde olduğu gibi, kayıt kümeleri üzerindeki işlemler arasında iyimser eşzamanlılık desteği de yer alır.  Bu örnekte, ne 'If-Match' ne de 'If-None-Match' belirtilmediğından, kayıt kümesi her zaman oluşturulur.  Bu çağrı, bu DNS bölgesinde aynı ada ve kayıt türüne sahip varolan tüm kayıtların üzerine yazar.

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create the actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a>Bölgeleri ve kayıt kümelerini alın

Ve `DnsManagementClient.Zones.Get` `DnsManagementClient.RecordSets.Get` yöntemler sırasıyla tek tek bölgeleri ve kayıt kümelerini alır. RecordSets, türüne, adına ve içinde bulunduğu bölge ve kaynak grubuna göre tanımlanır. Bölgeler adlarına ve içinde bulunduğu kaynak grubuna göre tanımlanır.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Varolan bir kayıt kümesini güncelleştirme

Varolan bir DNS kayıt kümesini güncelleştirmek için önce kayıt kümesini alın, sonra kayıt kümesi içeriğini güncelleştirin, sonra değişikliği gönderin.  Bu örnekte, 'If-Match' parametresinde ayarlanan alınan kayıttan 'Etag' olarak belirtiriz. Eşzamanlı bir işlem bu arada ayarlanan kaydı değiştirmişse, arama başarısız olur.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Liste bölgeleri ve kayıt kümeleri

Bölgeleri listelemek için, belirli bir kaynak grubundaki tüm bölgeleri veya belirli bir Azure aboneliğindeki tüm bölgeleri (kaynak grupları arasında) listelemeyi destekleyen *DnsManagementClient.Zone.List...* yöntemlerini kullanın. Kayıt kümelerini listelemek için, belirli bir bölgede tüm kayıt kümelerini veya yalnızca belirli bir türdeki kayıt kümelerini listelemeni destekleyen *DnsManagementClient.RecordSets.List...* yöntemlerini kullanın.

Bölgeleri ve kayıt kümelerini listelerken, sonuçların paginated olabileceğine dikkat edin.  Aşağıdaki örnek, sonuçların sayfaları aracılığıyla nasıl yinelenir gösterilmektedir. (Sayfalamayı zorlamak için yapay olarak küçük bir sayfa boyutu '2' kullanılır; uygulamada bu parametre atlanmalıdır ve varsayılan sayfa boyutu kullanılmalıdır.)

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
// In practice, to improve performance you would use a large page size or just use the system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure [DNS .NET SDK'nın](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)nasıl kullanılacağına ilişkin diğer örnekleri içeren azure DNS .NET SDK örnek projesini ve diğer DNS kayıt türlerine ilişkin örnekleri indirin.
