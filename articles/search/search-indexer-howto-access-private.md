---
title: Özel uç noktalar aracılığıyla güvenli kaynaklara erişen Dizin oluşturucular
titleSuffix: Azure Cognitive Search
description: Güvenli kaynaklarla iletişim kurmak için Dizin oluşturucular için özel uç noktalar ayarlamayı açıklayan kılavuz
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: bdf3863ae4683c6ef0e8dd3dd67a952bebf62d52
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89463915"
---
# <a name="accessing-secure-resources-via-private-endpoints"></a>Özel uç noktalar aracılığıyla güvenli kaynaklara erişme

Azure kaynakları (örneğin, veri kaynakları olarak kullanılan depolama hesapları), yalnızca belirli bir sanal ağ listesinden erişilebilmesi için yapılandırılabilir. Ayrıca, herhangi bir "ortak ağ" erişimine izin vermemek için de yapılandırılabilir.
Müşteriler, Dizin oluşturucular aracılığıyla bu tür veri kaynaklarından verilere güvenli bir şekilde erişmek için Azure Bilişsel Arama 'yi (giden) [özel bir uç nokta bağlantısı](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) oluşturmak üzere isteyebilir.

## <a name="shared-private-link-resources-management-apis"></a>Paylaşılan özel bağlantı kaynakları yönetim API 'Leri

Müşteri isteğiyle Azure Bilişsel Arama tarafından oluşturulan özel uç noktalar, "güvenli" kaynaklara erişmek için *paylaşılan özel bağlantı kaynakları*olarak adlandırılır. Müşteri, [Azure özel bağlantı hizmetine](https://azure.microsoft.com/services/private-link/)eklenmediyse 'e sahip olan bir kaynağa (örneğin, bir depolama hesabı) erişimi paylaşıyor.

Azure Bilişsel Arama, arama yönetimi API 'SI aracılığıyla, [paylaşılan özel bağlantı kaynakları oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate)özelliği aracılığıyla sunulur. Bu API 'yi, Azure Bilişsel Arama Dizin oluşturucudan güvenli bir kaynağa erişimi yapılandırmak için diğer *paylaşılan özel bağlantı kaynakları* yönetim API 'leriyle birlikte kullanacaksınız.

Bazı kaynaklara yönelik özel uç nokta bağlantıları `2020-08-01-Preview` , yalnızca aşağıdaki tabloda bulunan "Önizleme" etiketiyle belirtilen arama YÖNETIMI API 'sinin () önizleme sürümü ile oluşturulabilir. "Önizleme" etiketini içermeyen kaynaklar hem önizleme API 'si hem de GA API 'SI () ile oluşturulabilir `2020-08-01`

Azure Bilişsel Arama giden özel uç noktaların oluşturulabilmesi için Azure kaynaklarının listesi aşağıda verilmiştir. `groupId` Aşağıdaki tabloda listelenen bir paylaşılan özel bağlantı kaynağı oluşturmak için API 'de tam olarak (büyük/küçük harfe duyarlı) kullanılması gerekir.

| Azure Kaynağı | Grup Kimliği |
| --- | --- |
| Azure depolama-blob (veya) ADLS Gen 2 | `blob`|
| Azure depolama-tablolar | `table`|
| Azure Cosmos DB-SQL API 'SI | `Sql`|
| Azure SQL Veritabanı | `sqlServer`|
| MySQL için Azure veritabanı (Önizleme) | `mysqlServer`|
| Azure Key Vault | `vault` |
| Azure işlevleri (Önizleme) | `sites` |

Giden özel uç nokta bağlantılarının desteklendiği Azure kaynakları listesi, [desteklenen API listesi](https://docs.microsoft.com/rest/api/searchmanagement/privatelinkresources/listsupported)aracılığıyla da sorgulanabilir.

Bu kılavuzun amaçları doğrultusunda, REST API çağrılarını göstermek için [Armclient](https://github.com/projectkudu/ARMClient) ve [Postman](https://www.postman.com/) karışımı kullanılır.

> [!NOTE]
> Bu kılavuz boyunca, arama hizmeti adının, abonelik kimliği __00000000-0000-0000-0000-000000000000__olan bir aboneliğin __contoso__ kaynak grubunda bulunan __contoso-arama__ olduğunu varsayalım. Bu arama hizmetinin kaynak kimliği `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search`

Kılavuzun geri kalanında __contoso-Search__ hizmetinin, dizin oluşturucularının güvenli depolama hesabından verilere erişebilmeleri için nasıl yapılandırılabileceğini gösterilecektir. `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage`

## <a name="securing-your-storage-account"></a>Depolama hesabınızın güvenliğini sağlama

Depolama hesabını [yalnızca belirli alt ağlardan erişime izin verecek](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-a-virtual-network)şekilde yapılandırın. Azure portal aracılığıyla, bu seçeneği denetleyip kümeyi boş bırakırsanız, herhangi bir sanal ağdan hiçbir trafiğe izin verilmeyeceği anlamına gelir.

   ![Sanal ağ erişimi](media\search-indexer-howto-secure-access\storage-firewall-noaccess.png "Sanal ağ erişimi")

> [!NOTE]
> [Güvenilen Microsoft hizmet yaklaşımı](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services) , bu tür bir depolama hesabında sanal ağ veya IP kısıtlamalarını atlamak için kullanılabilir ve arama hizmetinin [nasıl yapılır kılavuzunda](search-indexer-howto-access-trusted-service-exception.md)açıklandığı gibi, depolama hesabındaki verilere erişmesini sağlayabilir. Ancak, Azure Bilişsel Arama ile depolama hesabı arasındaki bu yaklaşım iletişim, güvenli Microsoft omurga ağı üzerinden depolama hesabının genel IP adresi aracılığıyla gerçekleşir.

## <a name="step-1-create-a-shared-private-link-resource-to-the-storage-account"></a>1. Adım: depolama hesabına paylaşılan bir özel bağlantı kaynağı oluşturma

Depolama hesabına giden özel uç nokta bağlantısı oluşturmak için Azure Bilişsel Arama istemek üzere aşağıdaki API çağrısını yapın

`armclient PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01 create-pe.json`

`create-pe.json`Dosyanın içeriği (API 'nin istek gövdesini temsil eden) aşağıdaki gibidir:

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve"
      }
}
```

Başarılı olduğunda bir `202 Accepted` yanıt döndürülür; giden özel uç nokta oluşturma işlemi uzun süredir çalışan (zaman uyumsuz) bir işlemdir. Aşağıdaki kaynakların dağıtılmasını içerir-

1. Bir durumda özel bir IP adresi ile ayrılmış özel bir uç nokta `"Pending"` . Özel IP adresi, arama hizmeti özel özel Dizin Oluşturucu yürütme ortamının sanal ağına ayrılan adres alanından alınır. Özel uç nokta onaylandıktan sonra Azure Bilişsel Arama 'dan depolama hesabına olan tüm iletişimler özel IP adresinden ve güvenli bir özel bağlantı kanalından kaynaklanır.
2. Kaynak türüne göre özel bir DNS bölgesi `groupId` . Bu, özel kaynağa yönelik herhangi bir DNS aramasının özel uç noktayla ilişkili IP adresini kullandığından emin olur.

`groupId`Özel uç noktasını oluşturmakta olduğunuz kaynak türü için doğru öğesini belirttiğinizden emin olun. Herhangi bir uyuşmazlık, başarılı olmayan bir yanıt iletisi oluşmasına neden olur.

Tüm zaman uyumsuz Azure işlemlerine benzer şekilde, `PUT` çağrı `Azure-AsyncOperation` aşağıdaki gibi görünecek bir üst bilgi değeri döndürür:

`"Azure-AsyncOperation": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

Bu URI, işlemin durumunu elde etmek için düzenli aralıklarla yoklanabilir. Devam etmeden önce, paylaşılan özel bağlantı kaynağı işlem durumu bir Terminal durumuna (yani) ulaşana kadar beklemeyi öneririz `succeeded` .

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

```json
{
    "status": "running" | "succeeded" | "failed"
}
```

## <a name="step-2a-approve-the-private-endpoint-connection-for-the-storage-account"></a>Adım 2a: depolama hesabı için özel uç nokta bağlantısını onaylama

> [!NOTE]
> Bu bölüm, depolama için özel bir uç noktanın onay akışında gezinmek üzere Azure portal kullanır. Bunun yerine depolama kaynak sağlayıcısı (RP) ile kullanılabilen [REST API](https://docs.microsoft.com/rest/api/storagerp/privateendpointconnections) de kullanılabilir.
>
> CosmosDB, Azure SQL Server vb. gibi diğer sağlayıcılar da özel uç nokta bağlantılarını yönetmek için benzer RP API 'Leri sunar.

Azure portal depolama hesabının "**Özel uç nokta bağlantıları**" sekmesine gidin. Önceki API çağrısından (zaman uyumsuz işlem __başarılı__olduktan sonra) istek iletisiyle birlikte özel bir uç nokta bağlantısı için bir istek olması gerekir.

   ![Özel uç nokta onayı](media\search-indexer-howto-secure-access\storage-privateendpoint-approval.png "Özel uç nokta onayı")

Azure Bilişsel Arama tarafından oluşturulan özel uç noktayı seçin (önceki API 'de belirtilen ada göre özel uç nokta bağlantısını belirlemek için "özel uç nokta" sütununu kullanın) ve uygun bir iletiyle "Onayla" yı seçin (ileti önemli değildir). Özel uç nokta bağlantısının şu şekilde göründüğünden emin olun (Bu durum, portalda güncelleştirilmesini sağlamak için 1-2 dakikadan fazla bir yerde olabilir)

![Özel uç nokta onaylandı](media\search-indexer-howto-secure-access\storage-privateendpoint-after-approval.png "Özel uç nokta onaylandı")

Özel uç nokta bağlantısı isteği onaylandıktan sonra, trafiğin özel uç nokta üzerinden akışı *yapılabilir olduğu anlamına* gelir. Özel uç nokta onaylandıktan sonra Azure Bilişsel Arama, kendisi için oluşturulan DNS bölgesinde gerekli DNS bölgesi eşlemelerini oluşturacaktır.

## <a name="step-2b-query-the-status-of-the-shared-private-link-resource"></a>Adım 2b: paylaşılan özel bağlantı kaynağının durumunu sorgulama

 Paylaşılan özel bağlantı kaynağının onay sonrasında güncelleştirildiğini onaylamak için, [Get API 'si](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/get)aracılığıyla durumunu elde edin.

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01`

`properties.provisioningState`Kaynak `Succeeded` ve `properties.status` ise, `Approved` paylaşılan özel bağlantı kaynağının işlevsel olduğu ve Dizin oluşturucuların özel uç nokta üzerinden iletişim kurmak üzere yapılandırılabileceği anlamına gelir.

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve",
        "status": "Approved",
        "resourceRegion": null,
        "provisioningState": "Succeeded"
      }
}

```

## <a name="step-3-configure-indexer-to-run-in-the-private-environment"></a>3. Adım: Dizin oluşturucuyu özel ortamda çalışacak şekilde yapılandırma

> [!NOTE]
> Bu adım, Özel uç nokta bağlantısı onaylanmadan önce bile gerçekleştirilebilir. Özel uç nokta bağlantısı onaylanana kadar, güvenli bir kaynakla (depolama hesabı gibi) iletişim kurmayı denediği herhangi bir dizin oluşturucu geçici bir hata durumunda sona acaktır. Yeni Dizin oluşturucular oluşturulamaz. Özel uç nokta bağlantısı onaylandığında, Dizin oluşturucular özel depolama hesabına erişebilecektir.

1. Güvenli depolama hesabına ve depolama hesabı içinde uygun bir kapsayıcıya işaret eden [bir veri kaynağı oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-data-source) . Aşağıda Postman aracılığıyla gerçekleştirilen bu istek gösterilmektedir.
![Veri Kaynağı Oluşturma](media\search-indexer-howto-secure-access\create-ds.png "Veri kaynağı oluşturma")

2. Benzer şekilde [bir dizin oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-index) ve isteğe bağlı olarak REST API kullanarak [bir beceri oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset) .

3. Yukarıda oluşturulan veri kaynağına, dizine ve beceri işaret eden [bir Dizin Oluşturucu oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-indexer) . Ayrıca, Dizin Oluşturucu yapılandırma özelliğini olarak ayarlayarak, Dizin oluşturucuyu özel yürütme ortamında çalışacak şekilde zorlayın `executionEnvironment` `"Private"` .
![Dizin Oluşturucu oluştur](media\search-indexer-howto-secure-access\create-idr.png "Dizin Oluşturucu oluşturma")

Dizin Oluşturucu başarıyla oluşturulmalıdır ve özel uç nokta bağlantısı üzerinden depolama hesabından ilerleme-dizin oluşturma içeriği yapılmalıdır. Dizin oluşturucunun durumu, [Dizin Oluşturucu durum API 'si](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)aracılığıyla izlenebilir.

> [!NOTE]
> Zaten mevcut dizin oluşturucular varsa, öğesini olarak ayarlamak için [PUT API 'si](https://docs.microsoft.com/rest/api/searchservice/create-indexer) aracılığıyla yalnızca bunları güncelleştirebilirsiniz `executionEnvironment` `"Private"` .

## <a name="troubleshooting-issues"></a>Sorun giderme

- Bir Dizin Oluşturucu oluştururken, oluşturma "veri kaynağı kimlik bilgileri geçersiz" ile benzer bir hata iletisiyle başarısız olursa, Özel uç nokta bağlantısının *onaylanmadığı* ya da işlevsel olmadığı anlamına gelir.
[Get API](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/get)'sini kullanarak paylaşılan özel bağlantı kaynağının durumunu alın. *Onaylanmış* ise `properties.provisioningState` kaynağın konumunu denetleyin. `Incomplete`Bu durumda, bu kaynak için temel alınan bağımlılıkların bir kısmının sağlanması başarısız oldu `PUT` . sorunu çözmesi için paylaşılan özel bağlantı kaynağını "yeniden oluşturma" isteği yeniden gönderin. Yeniden onay gerekli olabilir. doğrulamak için kaynağın durumunu yeniden kontrol edin.
- Dizin Oluşturucu, ayarı yapılmadan oluşturulduysa `executionEnvironment` , Dizin Oluşturucu oluşturma işlemi başarılı olur ancak yürütme geçmişi, dizin oluşturucunun çalışmasının başarısız olduğunu gösterir. Yürütme ortamını belirtmek için [Dizin oluşturucuyu güncelleştirmeniz](https://docs.microsoft.com/rest/api/searchservice/update-indexer) gerekir.
- Dizin Oluşturucu, ayarı yapılmadan oluşturulduysa `executionEnvironment` ve başarıyla çalıştıktan sonra, Azure bilişsel arama 'nin yürütme ortamının, arama hizmeti 'ne özgü "özel" ortam olduğunu karar verdiği anlamına gelir. Bununla birlikte, bu, çeşitli faktörlere (Dizin Oluşturucu tarafından kullanılan kaynaklar, arama hizmeti üzerindeki yük vb.) göre değişebilir ve daha sonraki bir noktada başarısız olabilir. bu sayede, `executionEnvironment` `"Private"` gelecekte başarısız olmamasını sağlamak için olarak ayarlamanızı öneririz.
- [Kotalar ve sınırlar](search-limits-quotas-capacity.md) , kaç tane paylaşılan özel bağlantı kaynağı oluşturulup arama hizmetinin SKU 'suna bağlı olarak belirlenir.

## <a name="next-steps"></a>Sonraki adımlar

Özel uç noktalar hakkında daha fazla bilgi edinin:

- [Özel uç noktalar nelerdir?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)
- [Özel uç noktalar için gereken DNS yapılandırması](https://docs.microsoft.com/azure/private-link/private-endpoint-dns)