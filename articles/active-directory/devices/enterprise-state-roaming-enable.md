---
title: Azure Active Directory'de Kurumsal Durumda Dolaşımı etkinleştirme
description: Windows aygıtlarında Enterprise State Roaming ayarları hakkında sık sorulan sorular.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8c44d6266f5ea8cdd4f75d0449cb49852e71c905
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672405"
---
# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Azure Active Directory'de Kurumsal Durumda Dolaşımı etkinleştirme

Kurumsal Durum Dolaşımı, Azure AD Premium veya Kurumsal Mobilite + Güvenlik (EMS) lisansına sahip tüm kuruluş tarafından kullanılabilir. Azure AD aboneliğini nasıl alacağınız hakkında daha fazla bilgi için [Azure AD ürün sayfasına](https://azure.microsoft.com/services/active-directory)bakın.

Kurumsal Durum Dolaşımını etkinleştirdiğinizde, kuruluşunuz azure bilgi korumasıiçin azure hakları yönetimi koruması için otomatik olarak ücretsiz, sınırlı kullanım lisansı verilir. Bu ücretsiz abonelik, Kurumsal Durum Dolaşımı tarafından senkronize edilen kurumsal ayarları ve uygulama verilerini şifrelemek ve çözmekle sınırlıdır. Azure Hakları Yönetimi hizmetinin tüm özelliklerini kullanmak için [ücretli bir aboneliğiniz](https://azure.microsoft.com/pricing/details/information-protection/) olması gerekir.

> [!NOTE]
> Bu makale, Temmuz 2015'te Windows 10 ile başlatılan Microsoft Edge Legacy HTML tabanlı tarayıcı için geçerlidir. Makale, 15 Ocak 2020'de yayımlanan yeni Microsoft Edge Krom tabanlı tarayıcı için geçerli değildir. Yeni Microsoft Edge için Eşitleme davranışı hakkında daha fazla bilgi için [Microsoft Edge Eşitleme](/deployedge/microsoft-edge-enterprise-sync)makalesine bakın.

## <a name="to-enable-enterprise-state-roaming"></a>Kurumsal Durum Dolaşımını etkinleştirmek için

1. [Azure AD yönetici merkezinde](https://aad.portal.azure.com/)oturum açın.
1. **Azure Etkin Dizin** &gt; **Aygıtları** &gt; **Kurumsal Durum Dolaşımı'nı**seçin.
1. Seç'i Kullanıcılar **ayarları ve uygulama verilerini cihazlar arasında senkronize edebilir.** Daha fazla bilgi için [aygıt ayarlarını nasıl yapılandırılatırabilirsiniz.](/azure/active-directory/device-management-azure-portal)
  
   ![etiketli cihaz ayarı görüntüsü Kullanıcılar, ayarlar ve uygulama verilerini cihazlar arasında senkronize edebilir](./media/enterprise-state-roaming-enable/device-settings.png)
  
Bir Windows 10 aygıtının Kurumsal Durum Dolaşım hizmetini kullanabilmesi için aygıtın Azure AD kimliğini kullanarak kimlik doğrulaması yapması gerekir. Azure AD'ye katılan aygıtlar için, kullanıcının birincil oturum açma kimliği Azure AD kimliğidir, bu nedenle ek yapılandırma gerekmez. Şirket içinde Active Directory kullanan aygıtlar için BT yöneticisinin [karma Azure Etkin Dizin birleştirilmiş aygıtları yapılandırması](hybrid-azuread-join-manual-steps.md)gerekir. 

## <a name="data-storage"></a>Veri depolama

Kurumsal Durum Dolaşım verileri, Azure Etkin Dizin örneğinde ayarlanan ülke/bölge değeriyle en iyi uyumlu olan bir veya daha fazla [Azure bölgesinde](https://azure.microsoft.com/regions/) barındırılır. Enterprise State Roaming verileri üç ana coğrafi bölgeye göre bölümlenir: Kuzey Amerika, EMEA ve APAC. Kiracıiçin Kurumsal Durum Dolaşım verileri coğrafi bölgeyle birlikte yerel olarak bulunur ve bölgeler arasında çoğaltılamaz.  Örnek:

| Ülke/bölge değeri | kendi verilerine sahip |
| -------------------- | ------------------------ |
| Fransa veya Zambiya gibi bir EMEA ülkesi/bölgesi | Avrupa'daki Azure bölgelerinden biri veya birkaçı |
| Amerika Birleşik Devletleri veya Kanada gibi bir Kuzey Amerika ülkesi/bölgesi | ABD'deki Azure bölgelerinden biri veya birkaçı |
| Avustralya veya Yeni Zelanda gibi bir APAC ülkesi/bölgesi | Asya'daki Azure bölgelerinden biri veya birkaçı |
| Güney Amerika ve Antarktika bölgeleri | ABD'de bir veya daha fazla Azure bölgesi |

Ülke/bölge değeri Azure AD dizin oluşturma işleminin bir parçası olarak ayarlanır ve daha sonra değiştirilemez. Veri depolama konumunuz hakkında daha fazla bilgiye ihtiyacınız varsa, [Azure desteğiyle](https://azure.microsoft.com/support/options/)bir bilet gönderin.

## <a name="view-per-user-device-sync-status"></a>Kullanıcı başına aygıt eşitleme durumunu görüntüleme

Kullanıcı başına aygıt eşitleme durum raporunu görüntülemek için aşağıdaki adımları izleyin.

1. [Azure AD yönetici merkezinde](https://aad.portal.azure.com/)oturum açın.
1. **Azure Etkin Dizin** &gt; **Kullanıcıları** &gt; **Tüm kullanıcıları**seçin.
1. Kullanıcıyı seçin ve ardından **Aygıtlar'ı**seçin.
1. **Göster'in**altında, eşitleme durumunu göstermek için **Cihazları eşitleme ayarlarını ve uygulama verilerini** seçin.
  
   ![cihaz eşitleme veri ayarı görüntüsü](./media/enterprise-state-roaming-enable/sync-status.png)
  
1. Bu kullanıcı için eşitleme sağlayan aygıtlar varsa, aygıtları burada gösterildiği gibi görürsünüz.
  
   ![aygıt senkronizasyonu sütun verileri görüntüsü](./media/enterprise-state-roaming-enable/device-status-row.png)

## <a name="data-retention"></a>Veri saklama

Enterprise State Roaming kullanılarak Microsoft bulutuna senkronize edilen veriler, el ile silinene veya söz konusu verilerin bayat olduğu belirlenene kadar saklanır. 

### <a name="explicit-deletion"></a>Açık silme

Açık silme işlemi, bir Azure yöneticisinin bir kullanıcıyı veya bir dizini silmesi veya verilerin silinmesini açıkça istemesidir.

* **Kullanıcı silme**: Azure AD'de bir kullanıcı silindiğinde, kullanıcı hesabı dolaşım verileri 90 ila 180 gün sonra silinir. 
* **Dizin silme**: Azure AD'de tüm bir dizinin silinmesi hemen bir işlemdir. Bu diziyle ilişkili tüm ayarlar verileri 90 ila 180 gün sonra silinir. 
* **İstek silme işlemi :** Azure AD yöneticisi belirli bir kullanıcının verilerini veya ayar verilerini el ile silmek isterse, yönetici [Azure desteğiyle](https://azure.microsoft.com/support/)bir bilet açabilir. 

### <a name="stale-data-deletion"></a>Eski veri silme

Bir yıldır erişilmeyen veriler ("bekletme süresi") eski miş gibi değerlendirilir ve Microsoft bulutundan silinebilir. Bekletme süresi değişebilir, ancak 90 günden az olamaz. Eski veriler, belirli bir Windows/uygulama ayarları kümesi veya bir kullanıcı için tüm ayarlar olabilir. Örnek:

* Hiçbir aygıt belirli bir ayar koleksiyonuna erişmezse (örneğin, bir uygulama aygıttan kaldırılır veya "Tema" gibi bir ayar grubu kullanıcının tüm aygıtları için devre dışı bırakılırsa), bu koleksiyon bekletme döneminden sonra eskimiş olur ve silinebilir . 
* Bir kullanıcı tüm aygıtlarında ayarlar eşitlemeyi kapatmışsa, ayarlar verilerinin hiçbirine erişilmez ve bu kullanıcıya ait tüm ayarlar verileri eskir ve bekletme döneminden sonra silinebilir. 
* Azure AD dizin yöneticisi tüm dizin için Enterprise State Roaming'i kapatırsa, bu dizindeki tüm kullanıcılar eşitleme ayarlarını durdurur ve tüm kullanıcılar için tüm ayarlar verileri eskir ve bekletme döneminden sonra silinebilir. 

### <a name="deleted-data-recovery"></a>Silinen veri kurtarma

Veri saklama ilkesi yapılandırılamaz. Veriler kalıcı olarak silindikten sonra kurtarılamaz. Ancak, ayarlar verileri yalnızca Microsoft bulutundan silinir, son kullanıcı aygıtından değil. Herhangi bir aygıt daha sonra Enterprise State Roaming hizmetine yeniden bağlanırsa, ayarlar yeniden eşitlenir ve Microsoft bulutunda depolanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Durumda Dolaşıma genel bakış](enterprise-state-roaming-overview.md)
* [Ayarlar ve veri dolaşımı hakkında SSS](enterprise-state-roaming-faqs.md)
* [Ayarlar eşitleme için Grup İlkesi ve MDM ayarları](enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 dolaşım ayarları başvurusu](enterprise-state-roaming-windows-settings-reference.md)
* [Sorun giderme](enterprise-state-roaming-troubleshooting.md)
