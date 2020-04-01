---
title: Gelişmiş tehdit koruması yapılandırma
titleSuffix: Azure Storage
description: Azure Depolama için gelişmiş tehdit koruması yapılandırarak hesap etkinliğindeki anormallikleri algılayın ve hesabınıza erişmeye yönelik zararlı olabilecek girişimlerden haberdar olun.
services: storage
author: tamram
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 195f4b5057c0e2d644ab44cc4c32e97c8662d36d
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422790"
---
# <a name="configure-advanced-threat-protection-for-azure-storage"></a>Azure Depolama için gelişmiş tehdit koruması yapılandırma

Azure Depolama için gelişmiş tehdit koruması, depolama hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, bir güvenlik uzmanı olmadan veya güvenlik izleme sistemlerini yönetmeden tehditleri ele almanızı sağlar.

Aktivitedeki anormallikler oluştuğunda güvenlik uyarıları tetiklenir. Bu güvenlik uyarıları [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirilir ve ayrıca abonelik yöneticilerine e-posta yoluyla gönderilir ve şüpheli etkinlik ayrıntıları ve tehditleri nasıl araştırıp düzelteceklerine ilişkin öneriler sunulur.

> [!NOTE]
> Azure Depolama için gelişmiş tehdit koruması şu anda yalnızca Blob depolama için kullanılabilir. Bu hizmet ABD devlet bulutlarında kullanılabilir, ancak başka egemen veya Azure devlet bulutu bölgesi yoktur. Ücretsiz 30 günlük deneme sürümü de dahil olmak üzere fiyatlandırma ayrıntıları için [Azure Güvenlik Merkezi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın.

Azure Depolama için gelişmiş tehdit koruması, tehdit algılama için Blob depolamasına okuma, yazma ve silme isteklerinin tanılama günlüklerini sindirir. Gelişmiş tehdit koruması uyarılarını araştırmak için, Depolama Analizi Günlüğü'nün kullanılabilirliğini kullanarak ilgili depolama etkinliğini görüntüleyebilirsiniz. Daha fazla bilgi için Azure [portalındaki bir depolama hesabını İzle'de](storage-monitor-storage-account.md#configure-logging) **günlüğe kaydetme'ye** bakın.

## <a name="set-up-advanced-threat-protection"></a>Gelişmiş tehdit koruması ayarlama

Gelişmiş tehdit koruması, aşağıdaki bölümlerde açıklanan çeşitli şekillerde yapılandırabilirsiniz.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Azure [portalını](https://portal.azure.com/)başlatın.
1. Azure Depolama hesabınıza gidin. **Ayarlar**altında Gelişmiş **güvenlik'i**seçin.
1. Gelişmiş güvenlik yapılandırma **sayfasındaayarlar** bağlantısını seçin.
1. **Gelişmiş güvenliği** **ON**olarak ayarlayın.
1. Yeni veya güncelleştirilmiş ilkeyi kaydetmek için **Kaydet'i** tıklatın.

    ![Azure Depolama gelişmiş tehdit koruması açma](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="azure-security-center"></a>[Azure Güvenlik Merkezi](#tab/azure-security-center)

Azure Güvenlik Merkezi'nde Standart katmana abone olduğunuzda, tüm depolama hesaplarınızda gelişmiş tehdit koruması otomatik olarak ayarlanır. Belirli bir abonelik altında depolama hesaplarınız için gelişmiş tehdit koruması etkinleştirebilir veya devre dışı kullanabilirsiniz:

1. [Azure portalında](https://portal.azure.com) **Azure Güvenlik Merkezi'ni** başlatın.
1. Ana menüden **Fiyatlandırma & ayarlarını**tıklatın.
1. Depolama hesapları için tehdit koruması sağlamak veya devre dışı kalmak istediğiniz aboneliği tıklatın.

    ![Abonelik seçme](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-subscription.png)

1. **Fiyatlandırma katmanını**tıklatın.
1. Kaynak **türüne göre fiyatlandırma katmanıseç** bölümünde, **Depolama hesapları** satırında **Etkin** veya **Devre Dışı'yı**tıklatın.

    ![Güvenlik Merkezinde ATP'yi etkinleştirin](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing2.png)
1. **Kaydet**'e tıklayın.

### <a name="template"></a>[Şablon](#tab/template)

Gelişmiş tehdit koruması etkinleştirilmiş bir Azure Depolama hesabı dağıtmak için Azure Kaynak Yöneticisi şablonu kullanın. Daha fazla bilgi için [gelişmiş tehdit koruması içeren Depolama hesabına](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/)bakın.

### <a name="azure-policy"></a>[Azure İlkesi](#tab/azure-policy)

Belirli bir abonelik veya kaynak grubu altında depolama hesaplarında gelişmiş tehdit koruması sağlamak için bir Azure İlkesi kullanın.

1. Azure **İlkesi - Tanımlar** sayfasını başlatın.

1. **Depolama Hesapları ilkesinde Gelişmiş Tehdit Koruması Dağıtma'yı** arayın.

     ![Arama Politikası](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)

1. Bir Azure aboneliği veya kaynak grubu seçin.

    ![Abonelik veya Grup Seçin](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. İlkeyi atayın.

    ![İlke Tanımları Sayfası](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="rest-api"></a>[REST API](#tab/rest-api)

Belirli bir depolama hesabı için gelişmiş tehdit koruması ayarını oluşturmak, güncelleştirmek veya almak için Rest API komutlarını kullanın.

* [Gelişmiş tehdit koruması - Oluştur](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Gelişmiş tehdit koruması - Get](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Aşağıdaki PowerShell cmdlets kullanın:

* [Gelişmiş tehdit koruması etkinleştirme](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
* [Gelişmiş tehdit koruması alın](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
* [Gelişmiş tehdit korumayı devre dışı](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

---

## <a name="explore-security-anomalies"></a>Güvenlik anormalliklerini keşfedin

Depolama etkinliği anormallikleri oluştuğunda, şüpheli güvenlik olayı yla ilgili bilgileri içeren bir e-posta bildirimi alırsınız. Etkinliğin ayrıntıları şunlardır:

* Anomalinin doğası
* Depolama hesabı adı
* Etkinlik zamanı
* Depolama türü
* Potansiyel nedenler
* Soruşturma adımları
* Düzeltme adımları

E-posta da olası nedenleri ve araştırmak ve potansiyel tehdidi azaltmak için önerilen eylemler hakkında ayrıntıları içerir.

![Azure Depolama gelişmiş tehdit koruması uyarı e-postası](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Azure Güvenlik Merkezi'nin [Güvenlik uyarıları döşemesinden](../../security-center/security-center-managing-and-responding-alerts.md)geçerli güvenlik uyarılarınızı gözden geçirebilir ve yönetebilirsiniz. Belirli bir uyarıyı tıklattığınızda, mevcut tehdidi araştırmak ve gelecekteki tehditleri ele almak için ayrıntılar ve eylemler sağlar.

![Azure Depolama gelişmiş tehdit koruması uyarı e-postası](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="security-alerts"></a>Güvenlik uyarıları

Uyarılar, depolama hesaplarına erişmek veya depolama hesaplarından yararlanmak için alışılmadık ve zararlı olabilecek girişimler tarafından oluşturulur. Azure Depolama için yapılan uyarıların listesi için, [Azure Güvenlik Merkezi'ndeki veri hizmetleri için Tehdit korumasındaki](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurestorage) **Depolama** bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Depolama hesaplarında Günlükler](/rest/api/storageservices/About-Storage-Analytics-Logging) hakkında daha fazla bilgi edinin
* [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md) hakkında daha fazla bilgi edinin
