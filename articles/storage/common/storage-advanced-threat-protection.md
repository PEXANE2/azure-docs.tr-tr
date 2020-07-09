---
title: Gelişmiş tehdit koruması yapılandırma
titleSuffix: Azure Storage
description: Hesap etkinliğindeki anormallikleri algılamak ve hesabınıza erişmek için olası zararlı girişimler hakkında bildirim almak için Azure depolama için Gelişmiş tehdit koruması yapılandırın.
services: storage
author: tamram
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: 3069ee020d5f127eb0bdb8cbaf251cd3f3cef8d9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118434"
---
# <a name="configure-advanced-threat-protection-for-azure-storage"></a>Azure depolama için Gelişmiş tehdit koruması yapılandırma

Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, güvenlik uzmanı olmadan veya güvenlik izleme sistemlerini yönetmeden tehditleri ele almanıza olanak sağlar.

Güvenlik uyarıları, etkinlik durumunda olan bozukluklar gerçekleştiğinde tetiklenir. Bu güvenlik uyarıları [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleşiktir ve ayrıca, şüpheli etkinliklerin ayrıntıları ve tehditleri İnceleme ve düzeltme önerileri ile abonelik yöneticilerine e-posta yoluyla da gönderilir.

Hizmet, kaynak okuma, yazma ve silme isteklerinin, tehdit algılama için BLOB depolama ve Azure dosyaları (Önizleme) isteklerini geri alır. Gelişmiş tehdit korumasından gelen uyarıları araştırmak için Depolama Analizi günlüğünü kullanarak ilgili depolama etkinliğini görüntüleyebilirsiniz. Daha fazla bilgi için, [Azure Portal depolama hesabını izlemek](storage-monitor-storage-account.md#configure-logging)için bkz. **oturum açma yapılandırma** .

## <a name="availability"></a>Kullanılabilirlik

Azure Storage için Gelişmiş tehdit koruması Şu anda BLOB depolama, Azure dosyaları (Önizleme) ve Azure Data Lake Storage 2. (Önizleme) için kullanılabilir. Gelişmiş tehdit korumasını destekleyen hesap türleri, genel amaçlı v2, Blok Blobu ve BLOB depolama hesaplarını içerir. Gelişmiş tehdit koruması, tüm genel bulutlarda ve ABD kamu bulutlarında mevcuttur, ancak diğer sogeign veya Azure Kamu bulut bölgelerinde kullanılamaz.

Azure Blob depolama API 'Leri ve Data Lake Storage API 'Lerini kullanarak işlemleri Data Lake Storage için etkinleştirilmiş hiyerarşik ad alanları içeren hesaplar. Azure dosya paylaşımları SMB üzerinden işlemleri destekler.

Ücretsiz 30 günlük deneme sürümü de dahil olmak üzere fiyatlandırma ayrıntıları için bkz. [Azure Güvenlik Merkezi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="set-up-advanced-threat-protection"></a>Gelişmiş tehdit koruması ayarlama

Gelişmiş tehdit korumasını aşağıdaki bölümlerde açıklanan çeşitli yollarla yapılandırabilirsiniz.

### <a name="azure-security-center"></a>[Azure Güvenlik Merkezi](#tab/azure-security-center)

Azure Güvenlik Merkezi 'nde Standart katmana abone olduğunuzda, tüm depolama hesaplarınızda Gelişmiş tehdit koruması otomatik olarak ayarlanır. Belirli bir abonelik kapsamındaki depolama hesaplarınız için Gelişmiş tehdit korumasını aşağıdaki gibi etkinleştirebilir veya devre dışı bırakabilirsiniz:

1. Azure portal **Azure Güvenlik Merkezi 'ni** başlatın [Azure portal](https://portal.azure.com).
1. Ana menüden **fiyatlandırma & ayarları**' na tıklayın.
1. Depolama hesapları için tehdit korumasını etkinleştirmek veya devre dışı bırakmak istediğiniz aboneliğe tıklayın.

    ![Abonelik seçme](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-subscription.png)

1. **Fiyatlandırma katmanı**' na tıklayın.
1. **Kaynak türüne göre fiyatlandırma katmanını Seç** bölümünde, **depolama hesapları** satırında, **etkin** veya **devre dışı**' ya tıklayın.

    ![Güvenlik Merkezi 'nde ATP 'yi etkinleştir](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing2.png)
1. **Kaydet**’e tıklayın.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure Portal](https://portal.azure.com/)başlatın.
1. Azure depolama hesabınıza gidin. **Ayarlar**altında **Gelişmiş güvenlik**' i seçin.
1. Gelişmiş Güvenlik Yapılandırması sayfasında **Ayarlar** bağlantısını seçin.
1. **Gelişmiş güvenliği** **Açık**olarak ayarlayın.
1. Yeni veya güncelleştirilmiş ilkeyi kaydetmek için **Kaydet** ' e tıklayın.

    ![Azure Storage Gelişmiş tehdit korumasını aç](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="template"></a>[Şablon](#tab/template)

Gelişmiş tehdit koruması etkinleştirilmiş bir Azure depolama hesabı dağıtmak için Azure Resource Manager şablonu kullanın. Daha fazla bilgi için bkz. [Gelişmiş tehdit koruması olan depolama hesabı](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="azure-policy"></a>[Azure İlkesi](#tab/azure-policy)

Belirli bir abonelik veya kaynak grubu altındaki depolama hesapları genelinde gelişmiş tehdit korumasını etkinleştirmek için bir Azure Ilkesi kullanın.

1. Azure **Ilke tanımları** sayfasını başlatın.

1. **Depolama hesaplarında Gelişmiş tehdit koruması dağıtma** ilkesi için arama yapın.

     ![Ilke ara](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)

1. Bir Azure aboneliği veya kaynak grubu seçin.

    ![Abonelik veya grup seçin](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. İlkeyi atayın.

    ![İlke tanımları sayfası](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="rest-api"></a>[REST API](#tab/rest-api)

Belirli bir depolama hesabı için Gelişmiş tehdit koruması ayarı oluşturmak, güncelleştirmek veya almak için REST API komutlarını kullanın.

* [Gelişmiş tehdit koruması-oluştur](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Gelişmiş tehdit koruması-Get](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Aşağıdaki PowerShell cmdlet 'lerini kullanın:

* [Gelişmiş tehdit korumasını etkinleştir](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
* [Gelişmiş tehdit koruması al](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
* [Gelişmiş tehdit korumasını devre dışı bırak](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

---

## <a name="explore-security-anomalies"></a>Güvenlik anormalilerini keşfet

Depolama etkinliği bozuklukları gerçekleştiğinde, şüpheli güvenlik olayı hakkında bilgi içeren bir e-posta bildirimi alırsınız. Etkinliğin ayrıntıları şunları içerir:

* Anomali 'in doğası
* Depolama hesabı adı
* Olay saati
* Depolama türü
* Olası nedenler
* Araştırma adımları
* Düzeltme adımları

Bu e-posta, olası nedenlerdeki ayrıntıları ve olası tehdidi araştırmak ve azaltmak için önerilen eylemleri de içerir.

![Azure depolama Gelişmiş tehdit koruması uyarı e-postası](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Azure Güvenlik Merkezi 'nin [güvenlik uyarıları kutucuğunda](../../security-center/security-center-managing-and-responding-alerts.md)geçerli güvenlik uyarılarınızı gözden geçirebilir ve yönetebilirsiniz. Belirli bir uyarıyı tıklatmak, geçerli tehdidi araştırmak ve gelecekteki tehditleri ele almak için ayrıntıları ve eylemleri sağlar.

![Azure depolama Gelişmiş tehdit koruması uyarı e-postası](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Uyarılar, depolama hesaplarına erişmeye veya açıktan yararlanmaya yönelik olağan dışı ve potansiyel olarak zararlı denemelere göre oluşturulur. Azure depolama için uyarıların listesi için bkz. [Azure Güvenlik Merkezi 'nde veri Hizmetleri Için tehdit koruması](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurestorage)'ndaki **depolama** bölümü.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure depolama hesaplarında Günlükler](/rest/api/storageservices/About-Storage-Analytics-Logging) hakkında daha fazla bilgi edinin
* [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md) hakkında daha fazla bilgi edinin
