---
title: Depolama için Azure Defender-avantajlar ve Özellikler
description: Depolama için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 4677426337a48d4fde74f61b8a4ad6fcb695f420
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91577824"
---
# <a name="introduction-to-azure-defender-for-storage"></a>Depolama için Azure Defender 'a giriş

**Depolama Için Azure Defender** , Azure depolama hesaplarınızdaki zararlı olabilecek etkinlikleri algılar. Verileriniz, blob kapsayıcıları, dosya paylaşımları veya veri Lakes olarak depolanıp saklanmadığı için korunabilir.

Bu koruma katmanı, güvenlik uzmanı olmanıza gerek *kalmadan* tehditleri sağlamanıza olanak tanır ve güvenlik izleme sistemlerinizi yönetmenize yardımcı olur.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|**Depolama Için Azure Defender** , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Korumalı Depolama türleri:|[Blob Depolama](https://azure.microsoft.com/services/storage/blobs/)<br>[Azure Dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)<br>[Azure Data Lake Storage 2. Nesil](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)|
|Larının|![Evet](./media/icons/yes-icon.png) Ticari bulutlar<br>![Evet](./media/icons/yes-icon.png) US Gov<br>![Hayır](./media/icons/no-icon.png) Çin gov, diğer gov|
|||


## <a name="what-kind-of-alerts-does-azure-defender-for-storage-provide"></a>Azure Defender, depolama için ne tür uyarılar sağlar?

Güvenlik uyarıları şunları yaparken tetiklenir:

- **Şüpheli etkinlik** -Örneğin, depolama hesabına Tor 'ın etkin çıkış düğümü olarak BILINEN bir IP adresinden başarıyla erişildi
- **Anormal davranış** ; Örneğin, erişim düzenindeki bir depolama hesabına yapılan değişiklikler
- **Karşıya yüklenen olası kötü amaçlı yazılım** -karma saygınlığı Analizi karşıya yüklenen bir dosyanın kötü amaçlı

Uyarılar, bunları tetikleyen olayın ayrıntılarını ve tehditleri İnceleme ve düzeltme önerilerini içerir.

> [!TIP]
> [Bu blog gönderisine](https://techcommunity.microsoft.com/t5/azure-security-center/validating-atp-for-azure-storage-detections-in-azure-security/ba-p/1068131)ait yönergeleri izleyerek depolama uyarılarının benzetimini yapabilirsiniz.


## <a name="what-is-hash-reputation-analysis-for-malware"></a>Kötü amaçlı yazılım için karma saygınlığı analizi nedir?

Karşıya yüklenen bir dosyanın şüpheli olup olmadığını öğrenmek için depolama için Azure Defender, [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684)tarafından desteklenen karma saygınlığı analizini kullanır. Tehdit koruması araçları karşıya yüklenen dosyaları taramaz, böylece depolama günlüklerini inceler ve yeni yüklenen dosyaların karmalarını, bilinen virüsler, Truva atları, casus yazılım ve fidye ile karşılaştırın. 

Bir dosyanın kötü amaçlı yazılım içermesi şüpheli olduğu durumlarda, güvenlik merkezi bir uyarı görüntüler ve isteğe bağlı olarak, şüpheli dosyayı silmek üzere depolama sahibini onay için e-posta ile gönderebilir. Karma saygınlığı analizinin kötü amaçlı yazılım içerdiğini gösterdiği dosyaların bu otomatik olarak kaldırılmasını ayarlamak için, ["depolama hesabına yüklenen potansiyel kötü amaçlı yazılımlar" içeren uyarılarda tetiklemek üzere bir iş akışı Otomasyonu](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-respond-to-potential-malware-uploaded-to-azure-storage/ba-p/1452005)dağıtın.

> [!NOTE]
> Güvenlik Merkezi 'nin tehdit koruması yeteneklerini etkinleştirmek için, ilgili iş yüklerini içeren abonelikte Azure Defender 'ı etkinleştirmeniz gerekir.
>
> **Depolama Için Azure Defender 'ı** abonelik düzeyinde veya kaynak düzeyinde etkinleştirebilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, depolama için Azure Defender hakkında bilgi edindiniz.

İlgili malzemeler için aşağıdaki makalelere bakın: 

- Bir uyarının Güvenlik Merkezi tarafından oluşturulup oluşturulmadığı veya Güvenlik Merkezi tarafından farklı bir güvenlik ürününden alınıp alınmayacağı, dışarı aktarabilirsiniz. Uyarılarınızı Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için [uyarıları BIR SıEM 'ye aktarma](continuous-export.md)konusundaki yönergeleri izleyin.
- [Depolama için gelişmiş Defender 'ı etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
- [Depolama uyarıları için Azure Defender listesi](alerts-reference.md#alerts-azurestorage)
- [Microsoft 'un tehdit bilgileri özellikleri](https://go.microsoft.com/fwlink/?linkid=2128684)