---
title: Azure Izleyici 'de müşteri tarafından yönetilen depolama hesaplarını kullanma Log Analytics
description: Log Analytics senaryolar için kendi depolama hesabınızı kullanın
ms.subservice: logs
ms.topic: conceptual
author: noakup
ms.author: noakuper
ms.date: 09/03/2020
ms.openlocfilehash: 9d54e6eb84e3269eb95f8d314875474f78536652
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90526434"
---
# <a name="using-customer-managed-storage-accounts-in-azure-monitor-log-analytics"></a>Azure Izleyici 'de müşteri tarafından yönetilen depolama hesaplarını kullanma Log Analytics

Log Analytics, Azure Storage 'ı çeşitli senaryolarda kullanır. Bu kullanım genellikle otomatik olarak yönetilir. Ancak, bazı durumlar, müşteri tarafından yönetilen depolama hesabı olarak da adlandırılan kendi depolama hesabınızı sağlamanızı ve yönetmenizi gerektirir. Bu belge, WAD/LAD günlüklerinin, özel bağlantıya özgü senaryoların ve CMK şifrelemenin alımı için müşteri tarafından yönetilen depolamanın kullanımının ayrıntılarını ayrıntılarıyla ifade edecek. 

> [!NOTE]
> Bu biçimlendirmeye ve içeriğe göre değişiklik olabileceğinden, müşteri tarafından yönetilen depolama 'ya karşıya yükleme Log Analytics içerikler üzerinde bir bağımlılık yapmanızı öneririz.

## <a name="ingesting-azure-diagnostics-extension-logs-wadlad"></a>Azure Tanılama uzantısı günlüklerini geri yükleme (WAD/LAD)
Azure Tanılama uzantısı aracıları (sırasıyla, Windows ve Linux aracıları için WAD ve LAD olarak da bilinir), çeşitli işletim sistemi günlükleri toplar ve bunları müşteri tarafından yönetilen bir depolama hesabında depolar. Daha sonra bu günlükleri gözden geçirmek ve analiz etmek için Log Analytics kullanabilirsiniz.
Depolama hesabınızdan Azure Tanılama uzantısı günlüklerini toplama, depolama hesabını [Azure Portal](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-logs#collect-logs-from-azure-storage) kullanarak veya [Storage Insights API](https://docs.microsoft.com/rest/api/loganalytics/connectedsources/storage%20insights/createorupdate)'sini çağırarak depolama alanı veri kaynağı olarak Log Analytics çalışma alanınıza bağlayın.

Desteklenen veri türleri:
* Syslog
* Windows olayları
* Service Fabric
* ETW olayları
* IIS Günlükleri

## <a name="using-private-links"></a>Özel bağlantıları kullanma
Azure Izleyici kaynaklarına bağlanmak için özel bağlantılar kullanıldığında, müşteri tarafından yönetilen depolama hesapları bazı kullanım durumlarında gereklidir. Bu tür bir durum, özel günlüklerin veya IIS günlüklerinin alımı olur. Bu veri türleri önce bir ara Azure depolama hesabına Bloblar olarak yüklenir ve ardından bir çalışma alanına alınır. Benzer şekilde, bazı Azure Izleyici çözümleri, Azure Güvenlik Merkezi çözümü tarafından kullanılan Watson döküm dosyaları gibi büyük dosyaları depolamak için depolama hesaplarını kullanabilir. 

##### <a name="private-link-scenarios-that-require-a-customer-managed-storage"></a>Müşteri tarafından yönetilen depolama gerektiren özel bağlantı senaryoları
* Özel günlüklerin ve IIS günlüklerinin alımı
* ASC çözümünün Watson döküm dosyalarını toplamasına izin verme

### <a name="how-to-use-a-customer-managed-storage-account-over-a-private-link"></a>Özel bir bağlantı üzerinden müşteri tarafından yönetilen depolama hesabı kullanma
##### <a name="workspace-requirements"></a>Çalışma alanı gereksinimleri
Azure Izleyici 'ye bir özel bağlantı üzerinden bağlanılırken, Log Analytics aracılar yalnızca bir özel bağlantı üzerinden ağınıza bağlı olan çalışma alanlarına Günlükler gönderebilecektir. Bu kural, bir Azure Izleyici özel bağlantı kapsamı (AMPLS) nesnesini doğru bir şekilde yapılandırmanızı, çalışma alanınıza bağlamayı ve ardından AMPLS 'yi ağınıza özel bir bağlantı üzerinden bağlamayı gerektirir. AMPLS yapılandırma yordamı hakkında daha fazla bilgi için bkz. [Azure özel bağlantı kullanarak ağları güvenli bir şekilde Azure izleyici 'ye bağlama](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security). 
##### <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri
Depolama hesabının özel bağlantısına başarıyla bağlanması için şunları yapmanız gerekir:
* VNet 'iniz veya eşlenmiş bir ağ üzerinde yer alır ve özel bir bağlantı üzerinden sanal ağınıza bağlı olun. Bu, VNet 'teki aracıların depolama hesabına Günlükler göndermesini sağlar.
* Bağlandığı çalışma alanıyla aynı bölgede yer alır.
* Azure Izleyici 'nin depolama hesabına erişmesine izin verin. Yalnızca belirli ağların depolama hesabınıza erişmesine izin vermeyi seçtiyseniz, bu özel duruma de izin vermeniz gerekir: "Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver". Bu, Log Analytics bu depolama hesabına alınan günlükleri okumasına olanak tanır.
* Çalışma alanınız diğer ağlardan gelen trafiği de işlediğinde, depolama hesabını ilgili ağlardan/Internet 'ten gelen trafiğe izin verecek şekilde yapılandırmanız gerekir.

##### <a name="link-your-storage-account-to-a-log-analytics-workspace"></a>Depolama hesabınızı bir Log Analytics çalışma alanına bağlama
[Azure CLI](https://docs.microsoft.com/cli/azure/monitor/log-analytics/workspace/linked-storage) veya [REST API](https://docs.microsoft.com/rest/api/loganalytics/linkedstorageaccounts)aracılığıyla depolama hesabınızı çalışma alanına bağlayabilirsiniz. Uygulanabilir dataSourceType değerleri:
* CustomLogs: alma sırasında özel Günlükler ve IIS günlükleri için depolamayı kullanmak için.
* AzureWatson – ASC (Azure Güvenlik Merkezi) çözümü tarafından karşıya yüklenen Watson döküm dosyalarını depolamayı kullanın. Saklama Yönetimi, bağlı bir depolama hesabını değiştirme ve depolama hesabı etkinliğinizi izleme hakkında daha fazla bilgi için bkz. [bağlı depolama hesaplarını yönetme](#managing-linked-storage-accounts). 

## <a name="encrypting-data-with-cmk"></a>CMK ile verileri şifreleme
Azure depolama, bir depolama hesabındaki bekleyen tüm verileri şifreler. Varsayılan olarak, verileri Microsoft tarafından yönetilen anahtarlarla (MMK) şifreler. Ancak Azure depolama, Azure Anahtar Kasası 'ndan, depolama verilerinizi şifrelemek için müşteri tarafından yönetilen bir anahtar (CMK) kullanmanıza olanak tanır. Kendi anahtarlarınızı Azure Key Vault içeri aktarabilir ya da anahtar oluşturmak için Azure Key Vault API 'Lerini kullanabilirsiniz.
##### <a name="cmk-scenarios-that-require-a-customer-managed-storage-account"></a>Müşteri tarafından yönetilen bir depolama hesabı gerektiren CMK senaryoları
* CMK ile günlük uyarı sorgularını şifreleme
* Kaydedilen sorguları CMK ile şifreleme

### <a name="how-to-apply-cmk-to-customer-managed-storage-accounts"></a>CMK 'yi müşteri tarafından yönetilen depolama hesaplarına uygulama
##### <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri
Depolama hesabı ve Anahtar Kasası aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure depolama şifreleme ve anahtar yönetimi hakkında daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

##### <a name="apply-cmk-to-your-storage-accounts"></a>Depolama hesaplarınıza CMK uygulama
Azure depolama hesabınızı Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırmak için [Azure Portal](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal?toc=/azure/storage/blobs/toc.json), [PowerShell](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-powershell?toc=/azure/storage/blobs/toc.json) veya [CLI](https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-cli?toc=/azure/storage/blobs/toc.json)kullanın. 

## <a name="managing-linked-storage-accounts"></a>Bağlı depolama hesaplarını yönetme

Depolama hesaplarını çalışma alanınıza bağlamak veya bağlantısını kaldırmak için [Azure CLI](https://docs.microsoft.com/cli/azure/monitor/log-analytics/workspace/linked-storage) veya [REST API](https://docs.microsoft.com/rest/api/loganalytics/linkedstorageaccounts)kullanın.

##### <a name="create-or-modify-a-link"></a>Bağlantı oluşturma veya değiştirme
Bir depolama hesabını bir çalışma alanına bağladığınızda, Log Analytics hizmetin sahip olduğu depolama hesabı yerine onu kullanmaya başlayacaktır. Şunları yapabilirsiniz 
* Günlüklerin yükünü aralarında yaymak için birden fazla depolama hesabı kaydetme
* Birden çok çalışma alanı için aynı depolama hesabını yeniden kullanma

##### <a name="unlink-a-storage-account"></a>Depolama hesabının bağlantısını kaldırma
Bir depolama hesabı kullanmayı durdurmak için, depolamanın çalışma alanıyla bağlantısını kaldırın. Tüm depolama hesaplarının bir çalışma alanından bağlantısı kesmek, Log Analytics hizmet tarafından yönetilen depolama hesaplarına güvenmeye çalışacağı anlamına gelir. Ağınızın internet erişimi sınırlı ise, bu depolar kullanılamayabilir ve depolamaya bağlı olan herhangi bir senaryo başarısız olur.

##### <a name="replace-a-storage-account"></a>Bir depolama hesabını değiştirme
Alma işlemi için kullanılan bir depolama hesabını değiştirmek için
1.  **Yeni bir depolama hesabının bağlantısını oluşturun.** Günlüğe kaydetme aracıları güncelleştirilmiş yapılandırmayı alacak ve yeni depolama alanına veri göndermeye başlayacak. İşlem birkaç dakika sürebilir.
2.  **Ardından, aracıların kaldırılan hesaba yazmayı durdurması için eski depolama hesabının bağlantısını kaldırın.** Alma işlemi, tüm içeri alınana kadar bu hesaptan veri okumayı sürdürür. Tüm Günlükler alınana kadar depolama hesabını silmeyin.

### <a name="maintaining-storage-accounts"></a>Depolama hesaplarını sürdürme
##### <a name="manage-log-retention"></a>Günlük bekletmesini yönetme
Kendi depolama hesabınızı kullanırken, bekletme size aittir. Diğer bir deyişle, Log Analytics özel depolama ortamınızda depolanan günlükleri silmez. Bunun yerine, tercihlerinize göre yükü işlemek için bir ilke oluşturmanız gerekir.

##### <a name="consider-load"></a>Yüklemeyi düşünün
Depolama hesapları, istek azaltma isteklerini başlatmadan önce belirli bir okuma ve yazma isteği yükünü işleyebilir (daha fazla ayrıntı için bkz. [BLOB depolama Için ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account) ). Azaltma, günlüklerin alınması için gereken süreyi etkiler. Depolama Hesabınız aşırı yüklenmişse, yükü aralarında dağıtmak için ek bir depolama hesabı kaydedin. Depolama hesabınızın kapasitesini izlemek için [Azure Portal öngörülerini]( https://docs.microsoft.com/azure/azure-monitor/insights/storage-insights-overview)gözden geçirin.

### <a name="related-charges"></a>İlgili ücretler
Depolama hesapları, depolanan verilerin hacmi, depolama türü ve artıklık türü tarafından ücretlendirilir. Ayrıntılar için bkz. [Blok Blobu fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/blobs) ve [Tablo Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/tables).


## <a name="next-steps"></a>Sonraki adımlar

- [Ağları Azure izleyici 'ye güvenli bir şekilde bağlamak Için Azure özel bağlantısını kullanma](private-link-security.md) hakkında bilgi edinin
- [Azure izleyici müşterinin yönettiği anahtarlar](customer-managed-keys.md) hakkında bilgi edinin
