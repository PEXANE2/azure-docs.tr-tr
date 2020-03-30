---
title: Azure Etkin Dizinile eşitlemeyen bir nesneyi sorun giderme | Microsoft Dokümanlar'
description: Azure Etkin Dizini ile eşitlenmemiş bir nesneyi sorun giderin.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 931865803328189d89c0fbae15caa801c3f7f7c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253539"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-with-azure-active-directory"></a>Azure Etkin Dizinile eşitlemeyen bir nesneyi sorun giderme

Bir nesne beklendiği gibi Microsoft Azure Etkin Dizini (Azure AD) ile eşitlemiyorsa, bunun nedeni çeşitli nedenlerden olabilir. Azure AD'den bir hata e-postası aldıysanız veya Azure AD Connect Health'te hatayı görüyorsanız, [eşitleme sırasında sorun giderme hatalarını](tshoot-connect-sync-errors.md) okuyun. Ancak nesnenin Azure AD'de olmadığı bir sorunu gidermeye devam ediyorsanız, bu makale tam size göre. Şirket içi bileşen Azure AD Connect senkronizasyonunda hataların nasıl bulunup bulunulacağı açıklanır.

>[!IMPORTANT]
>Sürüm 1.1.749.0 veya üzeri sürümlerle Azure AD Connect dağıtımı için, nesne eşitleme sorunlarını gidermek için sihirbazdaki [sorun giderme görevini](tshoot-connect-objectsync.md) kullanın. 

## <a name="synchronization-process"></a>Eşitleme işlemi

Eşitleme sorunlarını araştırmadan önce, Azure AD Connect eşitleme işlemini anlayalım:

  ![Azure AD Connect eşitleme işleminin diyagramı](./media/tshoot-connect-object-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminoloji**

* **CS:** Bağlayıcı alanı, veritabanındaki tablo
* **MV:** Metaverse, veritabanında bir tablo

### <a name="synchronization-steps"></a>**Eşitleme adımları**
Eşitleme işlemi aşağıdaki adımları içerir:

1. **AD'den alma:** Etkin Dizin nesneleri Active Directory CS'ye getirilir.

2. **Azure AD'den alma:** Azure AD nesneleri Azure AD CS'ye getirilir.

3. **Eşitleme:** Gelen eşitleme kuralları ve giden eşitleme kuralları, daha düşükten yükseğe, öncelik numarası sırasına göre çalıştırılır. Eşitleme kurallarını görüntülemek için masaüstü uygulamalarından Eşitleme Kuralları Düzenleyicisi'ne gidin. Gelen eşitleme kuralları CS'den MV'ye veri getirir. Giden eşitleme kuralları verileri MV'den CS'ye taşır.

4. **AD'ye dışa aktarma:** Eşitlemeden sonra, nesneler Active Directory CS'den Active Directory'ye dışa aktarılır.

5. **Azure AD'ye dışa aktarma:** Eşitlemeden sonra nesneler Azure AD CS'den Azure AD'ye dışa aktarılır.

## <a name="troubleshooting"></a>Sorun giderme

Hataları bulmak için, aşağıdaki sırayla birkaç farklı yere bakın:

1. İşlem, alma ve eşitleme sırasında eşitleme altyapısı tarafından tanımlanan hataları bulmak için [günlüklenir.](#operations)
2. Eksik nesneleri ve eşitleme hatalarını bulmak için [bağlayıcı alanı.](#connector-space-object-properties)
3. Veri ile ilgili sorunları bulmak için [metaverse.](#metaverse-object-properties)

Bu adımları başlatmadan önce [Eşitleme Hizmet Yöneticisi'ni](how-to-connect-sync-service-manager-ui.md) başlatın.

## <a name="operations"></a>İşlemler
Eşitleme Hizmet Yöneticisi'ndeki **İşlemler** sekmesi, sorun giderme işleminizi başlatmanız gereken yerdir. Bu sekme, en son işlemlerin sonuçlarını gösterir. 

![Seçili Operasyonlar sekmesini gösteren Eşitleme Servis Yöneticisi'nin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/operations.png)  

**İşlemler** sekmesinin üst yarısı tüm çalışır kronolojik sırada gösterir. Varsayılan olarak, işlem günlüğü son yedi gün hakkında bilgi tutar, ancak bu ayar [zamanlayıcı](how-to-connect-sync-feature-scheduler.md)ile değiştirilebilir. **Başarı** durumunu göstermeyen herhangi bir çalıştırmaarayın. Üstbilgi tıklayarak sıralama değiştirebilirsiniz.

**Durum** sütunu en önemli bilgileri içerir ve bir çalışma için en ciddi sorunu gösterir. Burada, araştırma önceliği sırasına göre en yaygın durumların hızlı bir özeti (* birkaç olası hata dizesini gösterir).

| Durum | Açıklama |
| --- | --- |
| durdu-* |Koşu bitiremedi. Bu, örneğin, uzak sistem çöktü ve bağlantı kurulamıyorsa olabilir. |
| durmuş hata sınırı |5.000'den fazla hata var. Çok sayıda hata nedeniyle çalıştırma otomatik olarak durduruldu. |
| tamamlanmış-\*-hatalar |Çalışma bitti, ancak araştırılması gereken hatalar (5.000'den az) vardır. |
| tamamlanmış\*uyarılar |Çalışma tamamlandı, ancak bazı veriler beklenen durumda değil. Hatalarınız varsa, bu ileti genellikle yalnızca bir belirtidir. Hataları gidermeden uyarıları araştırmayın. |
| başarılı |Sorun yok. |

Bir satır seçtiğinizde, **Işlemler** sekmesinin alt bölümü bu çalıştırmanın ayrıntılarını göstermek için güncelleştirilir. Bu alanın en sol tarafında, **adım #** başlıklı bir liste olabilir. Bu liste yalnızca ormanınızda birden çok etki alanınız varsa ve her etki alanı bir adımla temsil edilirse görünür. Etki alanı adı **Bölüm**başlığı altında bulunabilir. **Eşitleme İstatistikleri** başlığı altında, işlenen değişikliklerin sayısı hakkında daha fazla bilgi bulabilirsiniz. Değiştirilen nesnelerin listesini almak için bağlantıları seçin. Hataları olan nesnelerinizin varsa, bu hatalar **Eşitleme Hataları** başlığı altında gösteriş gösterir.

### <a name="errors-on-the-operations-tab"></a>İşlemler sekmesindeki hatalar
Hatalarınız olduğunda, Eşitleme Hizmet Yöneticisi hem nesnenin hata sını hem de hatanın kendisini daha fazla bilgi sağlayan bağlantılar olarak gösterir.

![Eşitleme Hizmet Yöneticisi'ndeki hataların ekran görüntüsü](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Hata dizesini seçerek başlayın. (Önceki şekilde, hata dizesi **eşitleme-kural-hata-işlev-tetiklenir.)** İlk olarak nesnenin genel bakışı ile sunulur. Gerçek hatayı görmek için **Yığın İzleme'yi**seçin. Bu izleme hata ayıklama düzeyinde bilgi sağlar.

Çağrı Yığını **Bilgileri** kutusuna sağ tıklayın, **Tümü Seç'i**tıklatın ve ardından **Kopyala'yı**seçin. Ardından yığını kopyalayın ve Notepad gibi favori düzenleyicinizdeki hataya bakın.

Hata **SyncRulesEngine'den**geliyorsa, arama yığını bilgileri önce nesnedeki tüm öznitelikleri listeler. **InnerException =>** başlığını görene kadar aşağı kaydırın.  

  ![InnerException => başlığı altında hata bilgilerini gösteren Eşitleme Hizmet Yöneticisi'nin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)
  
Başlıktan sonraki satır hatayı gösterir. Önceki şekilde, hata Fabrikam oluşturulan özel bir eşitleme kuralından.

Hata yeterli bilgi vermiyorsa, verilerin kendisine bakma nın zamanı gelmiştir. Nesne tanımlayıcısı ile bağlantıyı seçin ve [bağlayıcı alanı içe aktarılan nesne](#cs-import)sorun giderme devam edin.

## <a name="connector-space-object-properties"></a>Bağlayıcı alanı nesne özellikleri
[**İşlemler**](#operations) sekmesinde hata yoksa, Etkin Dizin'den metaverse'e ve Azure AD'ye bağlayıcı alan nesnesini izleyin. Bu yolda, sorunun nerede olduğunu bulmalısınız.

### <a name="searching-for-an-object-in-the-cs"></a>CS'de bir nesne aranıyor

Eşitleme Hizmet Yöneticisi'nde **Bağlayıcılar'ı**seçin, Active Directory Connector'ı seçin ve **Arama Bağlayıcısı Alanı'nı**seçin.

**Kapsam** kutusunda, CN özniteliğiüzerinde arama yapmak istediğinizde **RDN'yi** seçin veya **ayırt edilen Ad** özniteliğiüzerinde arama yapmak istediğinizde **DN veya çapa'yı** seçin. Bir değer girin ve **Ara'yı**seçin. 
 
![Bağlayıcı alanı aramasının ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Aradığınız nesneyi bulamazsanız, [etki alanı tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#domain-based-filtering) veya [OU tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering)ile filtrelenmiş olabilir. Filtrelemenin beklendiği gibi yapılandırıldığından doğrulamak için [Azure AD Connect eşitlemesini okuyun: Filtrelemeyi yapılandırın.](how-to-connect-sync-configure-filtering.md)

Azure AD Bağlayıcısı'nı seçerek başka bir yararlı arama gerçekleştirebilirsiniz. **Kapsam** kutusunda, **Bekleyen İçe Aktarma'yı**seçin ve ardından **Ekle** onay kutusunu seçin. Bu arama, Azure AD'de şirket içi bir nesneyle ilişkilendirilemeyen tüm eşitlenmiş nesneleri sağlar.  

![Bağlayıcı alanı aramasında yetimlerin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cssearchorphan.png) 
 
Bu nesneler başka bir eşitleme altyapısı veya farklı bir filtreleme yapılandırması ile bir eşitleme altyapısı tarafından oluşturuldu. Bu yetim nesneler artık yönetilmiyor. Bu listeyi gözden geçirin ve [Azure AD PowerShell](https://aka.ms/aadposh) cmdlets'i kullanarak bu nesneleri kaldırmayı düşünün.

### <a name="cs-import"></a>CS alma
Bir CS nesnesini açtığınızda, en üstte birkaç sekme vardır. **İçe Aktarma** sekmesi, bir alma işleminden sonra sahnelenen verileri gösterir.  

![Bağlayıcı Alan Nesne Özellikleri penceresinin ekran görüntüsü, Alma sekmesi seçili](./media/tshoot-connect-object-not-syncing/csobject.png)    

**Eski Değer** sütunu şu anda Bağlan'da depolananları gösterir ve **Yeni Değer** sütunu kaynak sistemden alınan ve henüz uygulanmamış olanları gösterir. Nesneüzerinde bir hata varsa, değişiklikler işlenmez.

**Eşitleme Hatası** sekmesi, Bağlayıcı **Alan Nesne Özellikleri** penceresinde yalnızca nesneyle ilgili bir sorun varsa görünür. Daha fazla bilgi [ **için, Işlemler** sekmesinde eşitleme hatalarını](#errors-on-the-operations-tab)nasıl giderin irdeleyin.

![Bağlayıcı Alan Nesne Özellikleri penceresinde eşitleme hatası sekmesinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  

### <a name="cs-lineage"></a>CS soyu
Bağlayıcı Alan Nesne **Özellikleri** penceresindeki **Çizgi sekmesi,** bağlayıcı alan nesnesinin metaverse nesneyle nasıl ilişkili olduğunu gösterir. Bağlayıcının bağlı sistemden en son ne zaman bir değişiklik alındığını ve metaverse'deki verileri doldurmak için hangi kuralların uygulandığını görebilirsiniz.  

![Bağlayıcı Alan Nesne Özellikleri penceresinde Kihat sekmesini gösteren ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cslineage.png)  

Önceki şekilde, **Eylem** sütununda eylem **Hükmü**ile gelen bir eşitleme kuralı nı gösterir. Bu, bu bağlayıcı uzay nesnesi olduğu sürece metaverse nesnenin kaldığını gösterir. Eşitleme kuralları listesi bunun yerine bir **Provision** eylemi ile giden bir eşitleme kuralı gösterirse, metaverse nesnesi silindiğinde bu nesne silinir.  

![Bağlayıcı Alan Nesne Özellikleri penceresindeki Soy sekmesinde bir soy penceresinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/cslineageout.png)  

Önceki şekilde, **PasswordSync** sütununda, bir eşitleme kuralı **True**değerine sahip olduğundan, gelen bağlayıcı alanının parolada değişikliklere katkıda bulunabileceğini de görebilirsiniz. Bu parola, giden kural aracılığıyla Azure AD'ye gönderilir.

**Soy** sekmesinden [**Metaverse Nesne Özellikleri'ni**](#mv-attributes)seçerek metaverse'e ulaşabilirsiniz.

### <a name="preview"></a>Önizleme
**Bağlayıcı Alan Nesne Özellikleri** penceresinin sol alt köşesinde **Önizleme** düğmesi yer alıyor. Tek bir nesneyi eşitleyebileceğiniz **Önizleme** sayfasını açmak için bu düğmeyi seçin. Bu sayfa, bazı özel eşitleme kuralları sorun giderme ve tek bir nesne üzerinde bir değişikliğin etkisini görmek istiyorsanız yararlıdır. **Tam eşitleme** veya **Delta eşitleme**seçebilirsiniz. Yalnızca bellekdeğişikliğini engelleyen **Önizlemeoluştur'u**da seçebilirsiniz. Veya metaverse'i güncelleyen ve tüm değişiklikleri hedef bağlayıcı boşluklarına aşamalı olarak gerçekleştiren **Öngiyi Seç'i**seçin.  

![Önizleme sayfasının ekran görüntüsü, Başlangıç Önizleme'si seçili](./media/tshoot-connect-object-not-syncing/preview.png)  

Önizlemede nesneyi inceleyebilir ve belirli bir öznitelik akışı için hangi kuralın uygulandığını görebilirsiniz.  

![Alma Öznitelik Akışını gösteren Önizleme sayfasının ekran görüntüsü](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Günlük
**Önizleme** düğmesinin yanında, **Log** sayfasını açmak için **Günlük** düğmesini seçin. Burada parola eşitleme durumunu ve geçmişini görebilirsiniz. Daha fazla bilgi için Azure [AD Connect eşitlemeile Sorun Giderme parola karma senkronizasyonu'na](tshoot-connect-password-hash-synchronization.md)bakın.

## <a name="metaverse-object-properties"></a>Metaverse nesne özellikleri
Genellikle kaynak Active Directory bağlayıcı alanından aramaya başlamak daha iyidir. Ama aynı zamanda metaverse aramaya başlayabilirsiniz.

### <a name="searching-for-an-object-in-the-mv"></a>MV'de bir nesne aranıyor
Eşitleme Hizmet Yöneticisi'nde, aşağıdaki şekilde olduğu gibi **Metaverse Arama'yı**seçin. Kullanıcıyı bulduğunu bildiğiniz bir sorgu oluşturun. **AccountName** (**sAMAccountName**) ve **userPrincipalName**gibi ortak öznitelikleri arayın. Daha fazla bilgi için, [Eşitleme Hizmet Yöneticisi Metaverse arama](how-to-connect-sync-service-manager-ui-mvsearch.md)bakın.

![Metaverse Arama sekmesi seçili senkronizasyon Servis Yöneticisi ekran görüntüsü](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

Arama **Sonuçları** penceresinde nesneyi tıklatın.

Nesneyi bulamadıysanız, henüz metaverse ulaşmamıştır. Etkin Dizin [bağlayıcısı alanında](#connector-space-object-properties)nesneyi aramaya devam edin. Nesneyi Active Directory bağlayıcı sı'nda bulursanız, nesnenin metaverse gelmesini engelleyen bir eşitleme hatası olabilir veya eşitleme kuralı kapsam filtresi uygulanabilir.

### <a name="object-not-found-in-the-mv"></a>MV'de bulunamayan nesne
Nesne Active Directory CS'de yse ancak MV'de yoksa, kapsamlandırma filtresi uygulanır. Kapsam filtresine bakmak için masaüstü uygulama menüsüne gidin ve **Eşitleme Kuralları Düzenleyicisi'ni**seçin. Aşağıdaki filtreyi ayarlayarak nesneye uygulanan kuralları filtreleyin.

  ![Gelen eşitleme kuralları araması gösteren Eşitleme Kuralları Düzenleyicisi ekran görüntüsü](./media/tshoot-connect-object-not-syncing/syncrulessearch.png)

Listedeki her kuralı yukarıdan görüntüleyin ve Kapsam filtresini kontrol **edin.** Aşağıdaki kapsam filtresinde, **isCriticalSystemObject** değeri null veya FALSE veya boşsa, kapsam altındadır.

  ![Gelen eşitleme kuralı aramasında kapsam filtresinin ekran görüntüsü](./media/tshoot-connect-object-not-syncing/scopingfilter.png)

[CS Alma](#cs-import) öznitelik listesine gidin ve hangi filtrenin cismin MV'ye taşınmasını engellediğini denetleyin. **Bağlayıcı Alanı** öznitelik listesi yalnızca null olmayan ve boş olmayan öznitelikleri gösterir. Örneğin, **isCriticalSystemObject** listede görünmüyorsa, bu özniteliğin değeri boş veya boştur.

### <a name="object-not-found-in-the-azure-ad-cs"></a>Azure AD CS'de bulunmayan nesne
Nesne Azure AD'nin bağlayıcı alanında yoksa ancak MV'de mevcutsa, ilgili bağlayıcı alanının giden kurallarının kapsam filtresine bakın ve [MV öznitelikleri](#mv-attributes) ölçütlere uymadığı için nesnenin filtre uygulanıp filtrelenmediğini öğrenin.

Giden kapsam filtresine bakmak için aşağıdaki filtreyi ayarlayarak nesne için geçerli kuralları seçin. Her kuralı görüntüleyin ve karşılık gelen [MV öznitelik](#mv-attributes) değerine bakın.

  ![Synchronization Rules Editor'da giden eşitleme kuralları aramasının ekran görüntüsü](./media/tshoot-connect-object-not-syncing/outboundfilter.png)


### <a name="mv-attributes"></a>MV Öznitelikleri
**Öznitelikler** sekmesinde, değerleri ve bunların katkıda bulunduğu değerleri görebilirsiniz.  

![Metaverse Object Properties penceresinin ekran görüntüsü, Öznitelikler sekmesi seçili](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Bir nesne eşitlemiyorsa, metaverse öznitelik durumları hakkında aşağıdaki soruları sorun:
- Öznitelik **cloudFiltered** mevcut ve **True**olarak ayarlanmış mı? Varsa, [öznitelik tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#attribute-based-filtering)adımları göre filtrelenmiş.
- Öznitelik **kaynağıAnchor** mevcut mu? Değilse, bir hesap kaynağı orman topolojisi var mı? Bir nesne bağlı bir posta kutusu olarak tanımlanırsa **(öznitelik msExchRecipientTypeDetails** **değeri 2'ye**sahiptir ), **kaynak Çapa'ya** etkin active directory hesabı yla katkıda bulunular. Ana hesabın doğru şekilde içe aktarıldığından ve eşitlendirildidiğinden emin olun. Ana hesap nesne için [bağlayıcılar](#mv-connectors) arasında listelenmelidir.

### <a name="mv-connectors"></a>MV konektörler
**Bağlayıcılar** sekmesi, nesnenin temsiline sahip tüm bağlayıcı boşluklarını gösterir. 
 
![Metaverse Object Properties penceresinin ekran görüntüsü, Bağlayıcılar sekmesi seçili](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  

Bir bağlayıcınız olmalıdır:

- Kullanıcının temsil edildiği her Active Directory ormanı. Bu gösterim **yabancıGüvenlik İlkeleri** ve **İlgili Kişi** nesneleri içerebilir.
- Azure AD'de bir bağlayıcı.

Azure AD'nin bağlayıcısını kaçırıyorsanız, Azure AD'ye sağlama ölçütlerini doğrulamak için [MV öznitelikleri](#mv-attributes) bölümünü inceleyin.

**Bağlayıcılar** sekmesinden [bağlayıcı alan nesnesine](#connector-space-object-properties)de gidebilirsiniz. Bir satır seçin ve **Özellikler'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md)hakkında daha fazla bilgi edinin.
- [Karma kimlik](whatis-hybrid-identity.md)hakkında daha fazla bilgi edinin.
