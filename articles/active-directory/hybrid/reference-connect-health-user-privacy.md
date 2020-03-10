---
title: Azure AD Connect Health ve kullanıcı gizliliği | Microsoft Docs
description: Bu belge, Azure AD Connect Health ile Kullanıcı gizliliğini açıklamaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 04/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58cddcde71e6e86b9abe07eb4200f13ad55ea659
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78376417"
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Kullanıcı gizliliği ve Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makale Azure AD Connect Health ve kullanıcı gizliliği ile ilgilidir.  Azure AD Connect ve kullanıcı gizliliği hakkında bilgi için [buradaki](reference-connect-user-privacy.md)makaleye bakın.

## <a name="user-privacy-classification"></a>Kullanıcı Gizlilik sınıflandırması
Azure AD Connect Health, GDPR sınıflandırmasının **veri işlemcisi** kategorisine denk gelir. Veri işlemcisi işlem hattı olarak hizmet, önemli iş ortaklarına ve son tüketicilere veri işleme hizmeti sağlar. Azure AD Connect Health, Kullanıcı verileri oluşturmaz ve kişisel verilerin toplandığı ve nasıl kullanıldığı üzerinde bağımsız bir denetime sahip değildir. Azure AD Connect Health veri alımı, toplama, analiz ve raporlama, mevcut şirket içi verileri temel alır. 

## <a name="data-retention-policy"></a>Veri bekletme ilkesi
Azure AD Connect Health rapor oluşturmaz, analiz gerçekleştirmez veya öngörüleri 30 gün boyunca sağlar. Bu nedenle, Azure AD Connect Health 30 günden daha fazla veriyi depolamaz, işlemez veya korumaz. Bu tasarım, GDPR yönetmelikleri, Microsoft gizliliği uyumluluk düzenlemeleri ve Azure AD veri saklama ilkeleriyle uyumludur. 

Etkin **sistem durumu hizmeti verileri olan sunucular, en** fazla 30 ardışık gün için **hata** uyarıları güncel değildir ve bu süre boyunca Connect Health 'e ulaşmadığını önerin. Bu sunucular, Connect Health portalında devre dışı bırakılacak ve gösterilmez. Sunucuları yeniden etkinleştirmek için [sistem durumu aracısını kaldırmanız ve yeniden yüklemeniz](how-to-connect-health-agent-install.md)gerekir. Bu, aynı uyarı türüne sahip **Uyarılar** için uygulanmadığını lütfen unutmayın. Uyarılar, uyarı aldığınız sunucuda kısmi verilerin eksik olduğunu gösterir. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Azure AD Connect Health veri toplamayı ve izlemeyi devre dışı bırakma
Azure AD Connect Health, izlenen her sunucu için veya izlenen bir hizmetin bir örneği için veri toplamayı durdurmanızı sağlar. Örneğin, Azure AD Connect Health kullanılarak izlenen tek bir ADFS (Active Directory Federasyon Hizmetleri (AD FS)) sunucusu için veri toplamayı durdurabilirsiniz. Ayrıca, Azure AD Connect Health kullanılarak izlenmekte olan tüm ADFS örneği için veri toplamayı durdurabilirsiniz. Bunu seçtiğinizde, veri toplamayı durdurduktan sonra karşılık gelen sunucular Azure AD Connect Health portalından silinir. 

>[!IMPORTANT]
> İzlenen sunucuları Azure AD Connect Health silmek için Azure AD Genel yönetici ayrıcalıklarına ya da RBAC 'de katkıda bulunan rolüne sahip olmanız gerekir.
>
> Azure AD Connect Health bir sunucu veya hizmet örneğinin kaldırılması, geri alınamaz bir eylem değildir. 

### <a name="what-to-expect"></a>Beklensin mi?
Tek bir izlenen sunucu veya izlenen bir hizmetin bir örneği için veri toplamayı ve izlemeyi durdurursanız şunları göz önünde bulabilirsiniz:

- İzlenen bir hizmetin bir örneğini sildiğinizde, örnek portalda Azure AD Connect Health izleme hizmeti listesinden kaldırılır. 
- İzlenen bir sunucuyu veya izlenen bir hizmetin bir örneğini sildiğinizde, sistem durumu Aracısı kaldırılmaz veya sunucularınızdan kaldırılmaz. Sistem Durumu Aracısı, Azure AD Connect Health veri göndermeyecek şekilde yapılandırıldı. Daha önce izlenen sunucularda sistem durumu aracısını el ile kaldırmanız gerekir.
- Bu adımı gerçekleştirmeden önce sistem durumu aracısını kaldırmadıysanız, sistem durumu aracısıyla ilgili sunucu (lar) da hata olayları görebilirsiniz.
- İzlenen hizmet örneğine ait olan tüm veriler Microsoft Azure veri saklama Ilkesine göre silinir.

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>İzlenen bir hizmetin örneği için veri toplamayı ve izlemeyi devre dışı bırakma
[Azure AD Connect Health bir hizmet örneğini kaldırma](how-to-connect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service)bölümüne bakın.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>İzlenen bir sunucu için veri toplamayı ve izlemeyi devre dışı bırakma
Bkz. [Azure AD Connect Health bir sunucuyu kaldırma](how-to-connect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-all-monitored-services-in-azure-ad-connect-health"></a>Azure AD Connect Health tüm izlenen hizmetler için veri toplamayı ve izlemeyi devre dışı bırak
Azure AD Connect Health, Kiracıdaki **Tüm** kayıtlı hizmetlerin veri toplamayı durdurma seçeneği de sağlar. Eylemi gerçekleştirmeden önce, tüm genel yöneticilerin dikkatli bir şekilde dikkate alınması ve tam onayı olması önerilir. İşlem başladıktan sonra, Connect Health Service, tüm hizmetlerinizin verilerini alma, işleme ve raporlama işlemlerini durdurur. Connect Health hizmetindeki mevcut veriler 30 günden daha uzun bir süre boyunca tutulacaktır.
Belirli bir sunucunun veri toplamayı durdurmak istiyorsanız, lütfen belirli sunucuları silme adımlarını izleyin. Kiracı odaklı veri toplamayı durdurmak için, veri toplamayı durdurmak ve kiracının tüm hizmetlerini silmek için aşağıdaki adımları izleyin.

1. Ana dikey pencerede yapılandırma altında **Genel ayarlar** ' a tıklayın. 
2. Dikey pencerenin üst kısmındaki **veri toplamayı durdur** düğmesine tıklayın. İşlem başladıktan sonra Kiracı yapılandırma ayarlarının diğer seçenekleri devre dışı bırakılır.  
 
   ![Veri toplamayı durdur](./media/reference-connect-health-user-privacy/gdpr4.png)
  
3. Veri koleksiyonlarını durdurarak etkilenen eklendi hizmetleri listesinin bulunduğundan emin olun. 
4. Eylemi **Sil** düğmesini etkinleştirmek için tam kiracı adını girin
5. Tüm hizmetlerin silinmesini tetiklemek için **Sil** ' e tıklayın. Connect Health, eklendi hizmetlerinden gönderilen tüm verileri alma, işleme ve raporlama işlemlerini durdurur. Tüm süreci 24 saate kadar sürebilir. Bu adımın geri döndürülemez olduğuna dikkat edin. 
6. İşlem tamamlandıktan sonra, Connect Health içinde herhangi bir kayıtlı hizmeti daha görmezsiniz. 

   ![Veri toplama durdurulduktan sonra](./media/reference-connect-health-user-privacy/gdpr5.png)

## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Azure AD Connect Health veri toplamayı ve izlemeyi yeniden etkinleştirin
Daha önce silinmiş bir izlenen hizmette Azure AD Connect Health izlemeyi yeniden etkinleştirmek için, [sistem durumu aracısını](how-to-connect-health-agent-install.md) tüm sunuculara kaldırmanız ve yeniden yüklemeniz gerekir.

### <a name="re-enable-data-collection-and-monitoring-for-all-monitored-services"></a>İzlenen tüm hizmetler için veri toplamayı ve izlemeyi yeniden etkinleştirin

Kiracı odaklı veri toplama Azure AD Connect Health devam edebilir. Eylemi gerçekleştirmeden önce, tüm genel yöneticilerin dikkatli bir şekilde dikkate alınması ve tam onayı olması önerilir.

>[!IMPORTANT]
> Aşağıdaki adımlar, 24 saat devre dışı bırakma eyleminden sonra kullanılabilir olacaktır.
> Veri toplamayı etkinleştirdikten sonra, Connect Health 'teki sunulan Öngörüler ve izleme verileri, daha önce toplanan eski verileri göstermez. 

1. Ana dikey pencerede yapılandırma altında **Genel ayarlar** ' a tıklayın. 
2. Dikey pencerenin üst kısmındaki **veri toplamayı etkinleştir** düğmesine tıklayın. 
 
   ![Veri toplamayı etkinleştirme](./media/reference-connect-health-user-privacy/gdpr6.png)
 
3. **Etkinleştir** düğmesini etkinleştirmek için tam kiracı adını girin.
4. Connect Health hizmetinde veri toplamaya izin vermek için **Etkinleştir** düğmesine tıklayın. Bu değişiklik kısa bir süre içinde uygulanır. 
5. Aracıyı izlenecek sunuculara yeniden yüklemek için [yükleme işlemini](how-to-connect-health-agent-install.md) izleyin ve hizmetler portalda mevcut olacaktır.  


## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi 'nde Microsoft gizlilik ilkesini gözden geçirme](https://www.microsoft.com/trustcenter)
* [Azure AD Connect ve kullanıcı gizliliği](reference-connect-user-privacy.md)

