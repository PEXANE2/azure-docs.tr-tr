---
title: Azure Maliyet Yönetimi’nde gruplandırma ve filtreleme seçenekleri
description: Bu makalede, Azure Maliyet Yönetimi’ndeki gruplandırma ve filtreleme seçeneklerinin nasıl kullanıldığı açıklanmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 06/08/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 82e542eafe1578a969c4348fe7562ba1da106e2c
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88683411"
---
# <a name="group-and-filter-options-in-cost-analysis"></a>Maliyet analizinde gruplandırma ve filtreleme seçenekleri

Maliyet analizinde birçok gruplandırma ve filtreleme seçeneği vardır. Bu makale, bu seçenekleri ne zaman kullanacağınızı anlamanıza yardımcı olur.

Gruplandırma ve filtreleme seçenekleriyle ilgili bir video izlemek istiyorsanız [Maliyet Yönetimi boyutlara ve etiketlere göre raporlama](https://www.youtube.com/watch?v=2Vx7V17zbmk) videosunu izleyin. Diğer videoları izlemek için [Maliyet Yönetimi YouTube kanalını](https://www.youtube.com/c/AzureCostManagement) ziyaret edin.

>[!VIDEO https://www.youtube.com/embed/2Vx7V17zbmk]

## <a name="group-and-filter-properties"></a>Gruplandırma ve filtreleme özellikleri

Aşağıdaki tabloda, Maliyet analizindeki en yaygın kullanılan gruplandırma ve filtreleme seçenekleri ile bunları ne zaman kullanmanız gerektiği listelenmiştir.

| Özellik | Kullanılması gereken durumlar | Notlar |
| --- | --- | --- |
| **Kullanılabilirlik alanları** | AWS maliyetlerini kullanılabilirlik alanına göre ayırmanızı sağlar. | Yalnızca AWS kapsamları ve yönetim grupları için geçerlidir. Azure verileri, kullanılabilirlik alanı içermez ve **Geçerli değil** şeklinde görünür. |
| **Faturalama dönemi** | PAYG maliyetlerini faturalandırıldıkları veya faturalandırılacakları aya göre ayırmanızı sağlar. | Faturalandırılan PAYG ücretlerini tam olarak görüntülemek için **Faturalama dönemi** seçeneğini kullanabilirsiniz. Özel bir tarih aralığına göre filtreleme gerçekleştiriyorsanız faturalama döneminin önüne ve sonuna 2 gün daha ekleyin. Tam faturalama dönemi tarihleriyle sınırlamak, faturayla eşleşme yapılmasına engel olabilir. Fatura dönemindeki tüm faturaların maliyetlerini gösterir. Belirli bir faturayı filtrelemek için **Fatura Kimliği** değerini kullanın. EA ve MCA takvim ayı üzerinden faturalandırıldığından yalnızca PAYG abonelikleri için geçerlidir. EA/MCA hesapları, aynı hedefe ulaşmak için tarih seçicide takvim aylarını veya aylık ayrıntı düzeyini kullanabilir. |
| **Ücret türü** | Kullanım, satın alma, para iadesi ve kullanılmayan rezervasyon maliyetlerini ayırmanızı sağlar. | Rezervasyon satın alma işlemleri ve para iadeleri yalnızca gerçek maliyetler kullanıldığında mevcuttur, amorti edilen maliyetlerle kullanılamaz. Kullanılmayan rezervasyon maliyetleri yalnızca amorti edilen maliyetler görüntülendiğinde kullanılabilir. |
| **Departman** | Maliyetleri EA departmanına göre ayırmanızı sağlar. | Yalnızca EA ve yönetim grupları için kullanılabilir. PAYG aboneliklerinde departman yoktur ve **Geçerli değil** veya **atanmamış** olarak görünür. |
| **Kayıt hesabı** | Maliyetleri EA hesabı sahibine göre ayırmanızı sağlar. | Yalnızca EA ödeme hesapları, departmanları ve yönetim grupları için kullanılabilir. PAYG aboneliklerinde EA kaydı hesabı yoktur ve **Geçerli değil** veya **atanmamış** olarak görünür. |
| **Sıklık** | Kullanım tabanlı, tek seferlik ve yinelenen maliyetleri ayırmanızı sağlar. | |
| **Fatura Kimliği** | Maliyetleri düzenlenen faturaya göre ayırmanızı sağlar. | Faturalandırılmamış ücretler henüz bir fatura kimliğine sahip değildir ve EA maliyetleri fatura ayrıntılarını içermediğinden **Geçerli değil** şeklinde gösterilir.  |
| **Ölçüm** | Maliyetleri kullanım ölçümüne göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **Geçerli değil** şeklinde gösterilir. Satın alma işlemlerini tanımlamak için **Ücret türü**, Market ücretlerini tanımlamak için ise **Yayımcı türü** bilgilerine bakın. |
| **İşlem** | AWS maliyetlerini işleme göre ayırmanızı sağlar. | Yalnızca AWS kapsamları ve yönetim grupları için geçerlidir. Azure verileri işlem içermez ve **Geçerli değil** şeklinde görünür. Bunun yerine **Ölçüm** seçeneğini kullanabilirsiniz. |
| **Fiyatlandırma modeli** | Maliyetleri isteğe bağlı, rezervasyon veya spot kullanımına göre ayırmanızı sağlar. | Satın alma işlemleri **OnDemand** olarak gösterilir. **Geçerli değil** ifadesini görürseniz kullanımın rezervasyon mu yoksa isteğe bağlı kullanım mı olduğunu belirlemek için **Rezervasyon** ölçütüne göre, satın alma işlemlerini tanımlamak için ise **Ücret türü** ölçütüne göre gruplayın.
| **Sağlayıcı** | AWS ve Azure maliyetlerini ayırmanızı sağlar. | Yalnızca yönetim grupları için kullanılabilir. |
| **Yayımcı türü** | AWS, Azure ve Market maliyetlerini ayırmanızı sağlar. |  |
| **Rezervasyon** | Maliyetleri rezervasyona göre ayırmanızı sağlar. | Rezervasyon ile ilişkilendirilmemiş olan kullanımlar veya satın alma işlemleri **Geçerli değil** şeklinde gösterilir. Diğer Azure, AWS veya Market satın alma işlemlerini tanımlamak için **Yayımcı türü** ölçütüne göre gruplayın. |
| **Kaynak** | Maliyetleri kaynağa göre ayırmanızı sağlar. | Satın alma işlemleri EA/PAYG ödeme hesabı veya MCA faturalama profili düzeyinde uygulandığından ve belirli bir kaynakla ilişkilendirilmediğinden **Geçerli değil** şeklinde gösterilir. Diğer Azure, AWS veya Market satın alma işlemlerini tanımlamak için **Yayımcı türü** ölçütüne göre gruplayın. |
| **Kaynak grubu** | Maliyetleri kaynak grubuna göre ayırmanızı sağlar. | Satın alma işlemleri, aboneliklerle ilişkilendirilmemiş olan kiracı kaynakları, kaynak grubuna dağıtılmamış olan abonelik kaynakları ve klasik kaynaklar bir kaynak grubuna sahip olmadığından **diğer**, **klasik hizmetler**, **$system** veya **Geçerli değil** şeklinde gösterilir. |
| **Kaynak türü** | Maliyetleri kaynak türüne göre ayırmanızı sağlar. | Satın alma işlemleri ve klasik hizmetler bir Azure Resource Manager kaynak gürüne sahip olmadığından **diğer**, **klasik hizmetler** veya **Geçerli değil** şeklinde gösterilir. |
| **Kaynak konumu** | Maliyetleri konuma veya bölgeye göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **atanmamış**, **bilinmiyor**, **eşlenmemiş** veya **Geçerli değil** şeklinde gösterilebilir. |
| **Hizmet adı** veya **Ölçüm kategorisi** | Maliyetleri Azure hizmetine göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **Geçerli değil** veya **atanmamış** şeklinde gösterilir. |
| **Hizmet katmanı** veya **Ölçüm alt kategorisi** | Maliyetleri Azure kullanım ölçümü alt sınıflandırmasına göre ayırmanızı sağlar. | Satın alma işlemleri ve Market kullanımı **Geçerli değil** veya **atanmamış** şeklinde gösterilir. |
| **Abonelik** | Maliyetleri Azure aboneliğine ve bağlantılı AWS hesabına göre ayırmanızı sağlar. | Satın alma işlemleri ve kiracı kaynakları **Geçerli değil** şeklinde gösterilebilir. |
| **Tag** | Maliyetleri belirli bir etiket anahtarına ait etiket değerlerine göre ayırmanızı sağlar. | Etiketler; satın alma işlemleri, aboneliklerle ilişkilendirilmemiş olan kiracı kaynakları, kaynak grubuna dağıtılmamış abonelik kaynakları veya klasik kaynaklar için kullanılamaz. Bazı hizmetler kullanım verilerinde etiket içermez. [Kaynak türlerine göre etiket desteği](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-support) hakkında daha fazla bilgi edinin. |

Terimler hakkında daha fazla bilgi için bkz. [Azure kullanımı ve ücretleri dosyasında kullanılan terimleri anlama](../understand/understand-usage.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Maliyet analizini başlatın](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis).
