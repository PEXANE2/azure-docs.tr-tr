---
title: Azure Güvenlik Merkezi 'ne gelen önemli değişiklikler
description: Azure Güvenlik Merkezi 'Nde, planlamanız gerekebilecek ve planlamanız gerekebilen değişiklikler
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2020
ms.author: memildin
ms.openlocfilehash: 052758079d8d413f7b0fead2a5abf3b47b9a691e
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97511340"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Azure Güvenlik Merkezi 'Nde yapılan önemli değişiklikler

> [!IMPORTANT]
> Bu sayfadaki bilgiler, yayın öncesi ürün veya özelliklerle ilgilidir ve bu durum, her zaman ticari olarak yayınlanmadan önce önemli ölçüde değiştirilebilir. Microsoft, burada belirtilen bilgilere göre, açık veya zımni hiçbir taahhüt veya garanti vermez.

Bu sayfada, Güvenlik Merkezi için planlanan değişiklikler hakkında bilgi edineceksiniz. Üründe, güvenli puan veya iş akışlarınız gibi şeyleri etkileyebilecek planlı değişiklikler açıklanmaktadır.

En son sürüm notlarını arıyorsanız, bunları [Azure Güvenlik Merkezi 'ndeki](release-notes.md)Yenilikler bölümünde bulabilirsiniz.


## <a name="planned-changes"></a>Planlanan değişiklikler

- [Azure Ilke değerlendirmelerinde "uygun değil" kaynakları "uyumlu" olarak bildirilecek kaynaklar](#not-applicable-resources-to-be-reported-as-compliant-in-azure-policy-assessments)
- [Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklendi](#35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark)

### <a name="not-applicable-resources-to-be-reported-as-compliant-in-azure-policy-assessments"></a>Azure Ilke değerlendirmelerinde "uygun değil" kaynakları "uyumlu" olarak bildirilecek kaynaklar

**Değişikliğin tahmini tarihi:** Ocak 2021

Şu anda, bir öneri için değerlendirilen ve **uygulanamaz** olarak bulunan kaynaklar Azure ilkesinde "uyumsuz" olarak görünür. Kullanıcı eylemi, durumunu "uyumlu" olarak değiştirebilir. Bu planlı değişiklikten sonra, gelişmiş açıklık için "uyumlu" olarak bildirilir.

Azure Ilkesinde, uyumlu kaynakların sayısının arttıracağı tek etkisi görünür. Azure Güvenlik Merkezi 'nde güvenli puanınızın hiçbir etkisi olmayacaktır.

### <a name="35-preview-recommendations-being-added-to-increase-coverage-of-azure-security-benchmark"></a>Azure Güvenlik kıyaslamasının kapsamını artırmak için 35 önizleme önerisi eklenmiştir

**Değişikliğin tahmini tarihi:** Aralık 2020

Azure Güvenlik kıyaslaması, yaygın uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan, Azure 'a özgü bir dizi kılavuzlardır. [Azure Güvenlik Karşılaştırması hakkında daha fazla bilgi edinin](../security/benchmarks/introduction.md).

Bu kıyaslama kapsamını artırmak için aşağıdaki 35 önizleme önerileri güvenlik merkezi 'ne eklenecektir.

Önizleme önerileri bir kaynağı sağlıksız bir şekilde işlemez ve güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunmak için bunları mümkün olduğunda düzeltin. [Azure Güvenlik Merkezi 'ndeki önerileri düzeltin](security-center-remediate-recommendations.md)bölümünde bu önerilere yanıt verme hakkında daha fazla bilgi edinin.

| Güvenlik denetimi                     | Yeni öneriler                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bekleyen şifrelemeyi etkinleştir            | -Azure Cosmos DB hesapları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-Azure Machine Learning çalışma alanları, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)<br>-MySQL sunucuları için kendi anahtar veri korumanın etkinleştirilmesi gerekir<br>-PostgreSQL sunucuları için kendi anahtar veri korumanızı getir özelliği etkinleştirilmelidir<br>Bilişsel hizmetler hesapları, müşteri tarafından yönetilen bir anahtarla (CMK) veri şifrelemeyi etkinleştirmelidir<br>-Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)<br>-SQL yönetilen örnekler, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-SQL sunucuları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır<br>-Depolama hesapları, şifreleme için müşteri tarafından yönetilen anahtar (CMK) kullanmalıdır                                                                                                                                                              |
| En iyi güvenlik uygulamalarını uygulayın    | -Abonelikler güvenlik sorunları için bir iletişim e-posta adresine sahip olmalıdır<br> -Log Analytics aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir<br> -Yüksek önem derecesine sahip uyarılar için e-posta bildirimi etkinleştirilmelidir<br> -Yüksek önem derecesi uyarıları için abonelik sahibine e-posta bildirimi etkinleştirilmelidir<br> -Anahtar kasaları Temizleme koruması etkinleştirilmelidir<br> -Anahtar kasaları geçici silme etkin olmalıdır |
| Erişimi ve izinleri yönetme        | -Function uygulamalarında ' Istemci sertifikaları (gelen istemci sertifikaları) ' etkin olmalıdır |
| DDoS saldırılarına karşı uygulamaları koruma | -Web uygulaması güvenlik duvarı (WAF) Application Gateway için etkinleştirilmelidir<br> -Azure ön kapı hizmeti hizmeti için Web uygulaması güvenlik duvarı (WAF) etkinleştirilmelidir |
| Yetkisiz ağ erişimini kısıtla | -Güvenlik duvarının Key Vault üzerinde etkinleştirilmesi gerekir<br> -Özel uç nokta Key Vault için yapılandırılmalıdır<br> -Uygulama yapılandırması özel bağlantı kullanmalıdır<br> -Redsıs için Azure önbelleği bir sanal ağ içinde yer almalıdır<br> -Azure Event Grid etki alanları özel bağlantı kullanmalıdır<br> -Azure Event Grid konular özel bağlantı kullanmalıdır<br> -Azure Machine Learning çalışma alanları özel bağlantı kullanmalıdır<br> -Azure SignalR hizmeti özel bağlantı kullanmalıdır<br> -Azure yay bulutu, ağ ekleme kullanmalıdır<br> -Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor<br> -Kapsayıcı kayıt defterleri özel bağlantı kullanmalıdır<br> -MariaDB sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -MySQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -PostgreSQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır<br> -Depolama hesabı bir özel bağlantı bağlantısı kullanmalıdır<br> -Depolama hesapları, sanal ağ kurallarını kullanarak ağ erişimini kısıtlamalı<br> -VM görüntü Oluşturucu şablonlarının özel bağlantı kullanması gerekir|
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

İlgili bağlantılar:

- [Azure Güvenlik kıyaslaması hakkında daha fazla bilgi](../security/benchmarks/introduction.md)
- [MariaDB için Azure veritabanı hakkında daha fazla bilgi edinin](../mariadb/overview.md)
- [MySQL için Azure veritabanı hakkında daha fazla bilgi edinin](../mysql/overview.md)
- [PostgreSQL için Azure veritabanı hakkında daha fazla bilgi edinin](../postgresql/overview.md)





## <a name="next-steps"></a>Sonraki adımlar

Üründe yapılan son değişiklikler için bkz. [Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md).