---
title: Azure Hizmet Kumaşı sürümleri
description: Azure Hizmet Kumaşı için notlar bırakın. Service Fabric'teki en son özellikler ve geliştirmeler hakkında bilgi içerir.
ms.date: 06/10/2019
ms.topic: conceptual
hide_comments: true
hideEdit: true
ms.openlocfilehash: cb4fdd56e9cf67c71ac690d423499929167f8977
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064250"
---
# <a name="service-fabric-releases"></a>Servis Kumaş bültenleri

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Sorun Giderme Kılavuzları</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">Sorun İzleme</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">Destek Seçenekleri</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">Desteklenen Sürümler</a> 
| <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Kod Örnekleri</a>

Bu makalede, Hizmet Kumaş çalışma zamanı ve SDK'lar için en son sürümler ve güncelleştirmeler hakkında daha fazla bilgi sağlar.

## <a name="whats-new-in-service-fabric"></a>Service Fabric'teki yenilikler

### <a name="service-fabric-70"></a>Servis Kumaşı 7.0

Azure Servis Kumaşı 7.0 artık kullanıma sunuldu! Azure portalı üzerinden veya Azure Kaynak Yöneticisi dağıtımı aracılığıyla 7.0'a güncelleştirebilirsiniz. Tatil dönemi yle ilgili sürümlerle ilgili müşteri geri bildirimleri nedeniyle, Ocak ayına kadar otomatik yükseltme ler alacak şekilde ayarlanmış kümeleri otomatik olarak güncellemeye başlamayacağız.
Ocak ayında, standart roll-out yordamı devam edecek ve otomatik yükseltmeleri etkin kümeleri otomatik olarak 7.0 güncelleştirmesi almaya başlayacak. Piyasaya sürülmesi başlamadan önce bir duyuru daha yapacağız.
Ayrıca, bu politikayı göz önünde bulundurabileceğimizi belirtmek için planlanan çıkış tarihlerimizi de güncelleriz. Gelecekteki sürüm programlarımızla ilgili [güncellemeler](https://github.com/Microsoft/service-fabric/#service-fabric-release-schedule)için buraya bakın.
 
Bu Hizmet Kumaş en son sürülme ve önemli özellikleri ve iyileştirmeler ile yüklenir.

### <a name="key-announcements"></a>Önemli Duyurular
 - [**Uygulama sırları için KeyVaultReference desteği (Önizleme)**](https://docs.microsoft.com/azure/service-fabric/service-fabric-keyvault-references): [Yönetilen Kimlikleri](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) etkinleştiren Service Fabric uygulamaları artık bir Key Vault gizli URL'sini ortam değişkeni, uygulama parametresi veya kapsayıcı depo kimlik bilgisi olarak doğrudan başvuruda bulunabilir. Service Fabric, uygulamanın yönetilen kimliğini kullanarak sırrı otomatik olarak çözer. 
     
- **Devletsiz hizmetler için geliştirilmiş yükseltme güvenliği**: Bir uygulama yükseltmesi sırasında kullanılabilirliği garanti etmek için, kullanılabilir kabul edilecek devletsiz hizmetler için en az sayıda örneği tanımlamak için yeni [yapılandırmalar](https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription?view=azure-dotnet) sunduk. Daha önce bu değer tüm hizmetler için 1 idi ve değiştirilemez. Bu yeni hizmet başına güvenlik denetimi yle, hizmetlerinizin uygulama yükseltmeleri, küme yükseltmeleri ve Service Fabric'in sistem ve güvenlik kontrollerine dayanan diğer bakım sırasında en az sayıda yukarı örnek tutmasını sağlayabilirsiniz.
  
- [**Kullanıcı Hizmetleri için Kaynak Sınırları**](https://docs.microsoft.com/azure/service-fabric/service-fabric-resource-governance#enforcing-the-resource-limits-for-user-services): Kullanıcılar, Service Fabric sistem hizmetlerinin kaynak tükenmesi gibi senaryoları önlemek için kullanıcı hizmetleri için bir düğüm üzerinde kaynak sınırları ayarlayabilir. 
  
- Çoğaltma türü için [**çok yüksek hizmet taşıma maliyeti.**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-movement-cost) Çok Yüksek taşıma maliyetine sahip yinelemeler, yalnızca kümede başka bir şekilde düzeltilemeyen bir kısıtlama ihlali varsa taşınır. "Çok Yüksek" taşıma maliyetinin ne zaman kullanımının makul olduğu ve ek hususlar için daha fazla bilgi için lütfen dokümanlara bakın.
  
-  **Ek küme güvenlik kontrolleri**: Bu sürümde yapılandırılabilir tohum düğümü çoğunluk güvenlik kontrolü yaptık. Bu, küme yaşam döngüsü ve yönetim senaryoları sırasında kaç tohum düğümü bulunması gerektiğini özelleştirmenize olanak tanır. Kümeyi yapılandırılan değerin altına alacak işlemler engellenir. Bugün varsayılan değer her zaman tohum düğümlerinin bir çoğunluğudur, örneğin, 7 tohum düğüme sahipseniz, sizi 5 tohum düğümünün altına alacak bir işlem varsayılan olarak engellenir. Bu değişiklikle, aynı anda yalnızca bir tohum düğümünün inmesine izin veren minimum güvenli değer 6'yı yapabilirsiniz.
   
- [**Hizmet Kumaş Explorer'da Yedekleme ve Geri Yükleme hizmetini yönetmek**](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-quickstart-azurecluster)için destek eklendi. Bu, aşağıdaki etkinlikleri doğrudan SFX içinden mümkün kılar: yedekleme ve geri yükleme hizmetini keşfetmek, yedekleme ilkesi oluşturmak, otomatik yedeklemeleri etkinleştirmek, geçici yedeklemeler almak, geri yükleme işlemlerini tetiklemek ve varolan yedeklemelere göz atmak.

- [**ReliableCollectionsMissingTypesTool**](https://github.com/hiadusum/ReliableCollectionsMissingTypesTool)kullanılabilirliğinin duyurulabilir : Bu araç, güvenilir koleksiyonlarda kullanılan türlerin yuvarlanma uygulaması yükseltmesi sırasında ileri ve geri uyumlu olduğunu doğrulamaya yardımcı olur. Bu, eksik veya uyumsuz türler nedeniyle yükseltme hatalarını veya veri kaybını ve veri bozulmasını önlemeye yardımcı olur.

- [**İkincil yinelemelerde kararlı okumaları etkinleştirin**](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-configuration#configuration-names-1):Kararlı okumalar, ikincil yinelemeleri çoğunluk-acked olan dönen değerlerle sınırlandıracaktır.

Buna ek olarak, bu sürüm diğer yeni özellikler, hata düzeltmeleri ve desteklenebilirlik, güvenilirlik ve performans iyileştirmeleri içerir. Değişikliklerin tam listesi için lütfen [sürüm notlarına](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md)bakın.

### <a name="service-fabric-70-releases"></a>Servis Kumaş 7.0 bültenleri

| Sürüm tarihi | Yayınla | Daha fazla bilgi |
|---|---|---|
| 18 Kasım 2019 | [Azure Servis Kumaşı 7.0](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Service-Fabric-7-0-Release/ba-p/1015482)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md)|
| 30 Ocak 2020 | [Azure Hizmet Kumaşı 7.0 Yenileme Sürümü](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-second-refresh-release/ba-p/1137690)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service-Fabric-70CU2-releasenotes.md)|
| 6 Şubat 2020 | [Azure Hizmet Kumaşı 7.0 Yenileme Sürümü](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-third-refresh-release/ba-p/1156508)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service-Fabric-70CU3-releasenotes.md)|

### <a name="service-fabric-65"></a>Servis Kumaşı 6.5

Bu sürüm, küme ve uygulama yaşam döngüsü yönetimini kolaylaştırmak için desteklenebilirlik, güvenilirlik ve performans iyileştirmeleri, yeni özellikler, hata düzeltmeleri ve geliştirmeler içerir.

> [!IMPORTANT]
> Service Fabric 6.5, Visual Studio 2015'te Service Fabric araçları desteği ile son sürümdür. Müşterilerin Visual Studio [2019'a](https://visualstudio.microsoft.com/vs/) geçmeleri tavsiye edilir.

Service Fabric 6.5'teki yenilikler şunlardır:

- Service Fabric Explorer, resim mağazasına yüklediğiniz uygulamaları incelemek için bir [Resim Mağazası Görüntüleyici](service-fabric-visualizing-your-cluster.md#image-store-viewer) içerir.

- [Patch Orkestrasyon Uygulaması (POA)](service-fabric-patch-orchestration-application.md) sürüm [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) birçok kendi kendine tanı iyileştirmeler içerir. POA müşterilerinin bu sürüme geçmeleri önerilir.

- Devre dışı bırakmadığınız sürece [EventStore Hizmeti,](service-fabric-visualizing-your-cluster.md#event-store) Service Fabric 6.5 kümeleri için varsayılan olarak etkinleştirilir.

- Devlet hizmetleri için [yineleme yaşam döngüsü olayları](service-fabric-diagnostics-event-generation-operational.md#replica-events) eklendi.

- Tohum düğümü sağlıksızsa küme düzeyinde uyarılar da dahil olmak üzere [tohum düğümü durumunun daha iyi görünürlüğü](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status)*(Aşağı*, *Kaldırıldı* veya *Bilinmiyor).*

- [Servis Kumaş Uygulaması Olağanüstü Kurtarma Aracı,](https://github.com/Microsoft/Service-Fabric-AppDRTool) Servis Kumaşı'nın stateful hizmetlerinin birincil küme bir felaketle karşılaştığında hızla kurtarılmasını sağlar. Birincil kümeden gelen veriler, periyodik yedekleme ve geri yükleme kullanılarak ikincil bekleme uygulamasında sürekli olarak senkronize edilir.

- [.NET Core uygulamalarını Linux tabanlı kümelere yayınlamak için](service-fabric-how-to-publish-linux-app-vs.md)Visual Studio desteği.

- [Azure Hizmet Kumaşı CLI (SFCTL),](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) Azure'da yeni bir Linux kümesini yükselttiğinde veya oluşturduğunuzda Service Fabric 6.5 (ve sonraki sürümler) için otomatik olarak yüklenir.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) varsayılan olarak MacOS/Linux OneBox kümelerine yüklenir.

Daha fazla bilgi için [Hizmet Kumaşı 6.5 Sürüm Notları'na](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)bakın.

### <a name="service-fabric-65-releases"></a>Servis Kumaş ı 6.5 bültenleri

| Sürüm tarihi | Yayınla | Daha fazla bilgi |
|---|---|---|
| 11 Haziran 2019 | [Azure Servis Kumaşı 6.5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/06/11/azure-service-fabric-6-5-release/)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)|
| 2 Temmuz 2019 | [Azure Hizmet Kumaşı 6.5 Yenile Sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/07/04/azure-service-fabric-6-5-refresh-release/)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU1.pdf)  |
| 29 Temmuz 2019 | [Azure Hizmet Kumaşı 6.5 Yenile Sürümü](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Second-Refresh-Release/ba-p/800523)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU2.pdf)  |
| 23 Ağustos 2019 | [Azure Hizmet Kumaşı 6.5 Yenile Sürümü](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Third-Refresh-Release/ba-p/818599)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU3.pdf)  |
| Ekim 14, 2019 | [Azure Hizmet Kumaşı 6.5 Yenile Sürümü](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Fifth-Refresh-Release/ba-p/913296)  | [Sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU5.md)  |


## <a name="previous-versions"></a>Önceki sürümler

### <a name="service-fabric-64-releases"></a>Servis Kumaş ı 6.4 bültenleri

| Sürüm tarihi | Yayınla | Daha fazla bilgi |
|---|---|---|
| Kasım 30, 2018 | [Azure Servis Kumaşı 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 Aralık 2018 | [Azure Hizmet Dokusu 6.4 Windows kümeleri için Sürüm Yenileme](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| Şubat 4, 2019 | [Azure Hizmet Kumaşı 6.4 Yenile Sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 Mart 2019 | [Azure Hizmet Kumaşı 6.4 Yenile Sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 Nisan 2019 | [Azure Hizmet Kumaşı 6.4 Yenile Sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| Mayıs 2, 2019 | [Azure Hizmet Kumaşı 6.4 Yenile Sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| Mayıs 28, 2019 | [Azure Hizmet Kumaşı 6.4 Yenile Sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
