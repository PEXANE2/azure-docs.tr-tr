---
title: Azure AD, Sağlık Ve Kullanıcı Gizliliğini Bağlayın | Microsoft Dokümanlar
description: Bu belge, Azure AD Connect Health ile kullanıcı gizliliğini açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253669"
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Kullanıcı gizliliği ve Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makalede, Azure AD Connect Health ve kullanıcı gizliliği ile ilgilidir.  Azure AD Connect ve kullanıcı gizliliği hakkında daha fazla bilgi için [makaleye buradan](reference-connect-user-privacy.md)bakın.

## <a name="user-privacy-classification"></a>Kullanıcı gizliliği sınıflandırması
Azure AD Connect Health, GDPR sınıflandırmasının **veri işlemcisi** kategorisine girer. Bir veri işlemcisi boru hattı olarak, hizmet önemli ortaklara ve son tüketicilere veri işleme hizmetleri sağlar. Azure AD Connect Health kullanıcı verileri oluşturmaz ve hangi kişisel verilerin toplandığı ve nasıl kullanıldığı üzerinde bağımsız bir denetime sahip değildir. Azure AD Connect Health'te veri alma, toplama, analiz ve raporlama, mevcut şirket içi verilere dayanır. 

## <a name="data-retention-policy"></a>Veri saklama ilkesi
Azure AD Connect Health raporlar oluşturmaz, analiz yapmaz veya 30 günden fazla öngörü sağlamaz. Bu nedenle, Azure AD Connect Health 30 günden uzun bir süre boyunca hiçbir veriyi depolamaz, işlemez veya saklamaz. Bu tasarım, GDPR yönetmeliklerine, Microsoft gizlilik uyumluluk yönetmeliklerine ve Azure REKLAM veri saklama ilkeleriyle uyumludur. 

Etkin Sağlık hizmeti verilerine sahip **sunucular,** 30 günden uzun bir süredir güncel **hata** uyarılarına sahip değildir ve bu süre içinde Hiçbir veri Connect Health'e ulaşmadığını gösterir. Bu sunucular devre dışı bırakılır ve Connect Health portalında gösterilmez. Sunucuları yeniden etkinleştirmek [için, sistem durumu aracısını](how-to-connect-health-agent-install.md)kaldırmanız ve yeniden yüklemeniz gerekir. Bunun aynı uyarı türüne sahip **uyarılar** için geçerli olmadığını lütfen unutmayın. Uyarılar, uyarı aldığınız sunucuda kısmi verilerin eksik olduğunu gösterir. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Azure AD Connect Health'te veri toplama yı ve izlemeyi devre dışı
Azure AD Connect Health, izlenen her bir sunucu veya izlenen bir hizmet örneği için veri toplamayı durdurmanızı sağlar. Örneğin, Azure AD Connect Health kullanılarak izlenen tek tek ADFS (Active Directory Federation Services) sunucuları için veri toplamayı durdurabilirsiniz. Azure AD Connect Health kullanılarak izlenen tüm ADFS örneğinin veri toplamasını da durdurabilirsiniz. Bunu yapmayı seçtiğinizde, ilgili sunucular veri toplamayı durdurduktan sonra Azure AD Connect Health portalından silinir. 

>[!IMPORTANT]
> İzlenen sunucuları Azure AD Connect Health'ten silmek için Azure AD Global Administrator ayrıcalıklarına veya RBAC'daki Katılımcı rolüne ihtiyacınız vardır.
>
> Bir sunucu veya hizmet örneğini Azure AD Connect Health'ten kaldırmak geri döndürülebilir bir eylem değildir. 

### <a name="what-to-expect"></a>Ne beklemelisin?
Tek bir izlenen sunucu veya izlenen bir hizmet örneği için veri toplamayı ve izlemeyi durdurursanız, aşağıdakileri unutmayın:

- İzlenen bir hizmetörneğini sildiğinizde, örnek portaldaki Azure AD Connect Sistem Durumu izleme hizmet listesinden kaldırılır. 
- İzlenen bir sunucuyu veya izlenen bir hizmetörneğini sildiğinizde, Sistem Durumu Aracısı kaldırılmaz veya sunucularınızdan kaldırılır. Sistem Durumu Aracısı, Azure AD Connect Health'e veri göndermemek üzere yapılandırıldı. Daha önce izlenen sunucularda Sistem Durumu Aracısı'nı el ile kaldırmanız gerekir.
- Bu adımı gerçekleştirmeden önce Sağlık Aracısı'nı kaldırmadıysanız, Sunucuda Sağlık Aracısı ile ilgili hata olayları görebilirsiniz.
- İzlenen hizmetörneğine ait tüm veriler Microsoft Azure Veri Saklama İlkesi uyarınca silinir.

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>İzlenen bir hizmet örneği için veri toplama ve izlemeyi devre dışı
[Azure AD Connect Health'ten bir hizmet örneğini nasıl kaldırılacak üzerinde](how-to-connect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service)görün.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>İzlenen bir sunucu için veri toplama ve izlemeyi devre dışı
[Bir sunucunun Azure AD Connect Health'ten nasıl kaldırılabildiğini](how-to-connect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service)görün.

### <a name="disable-data-collection-and-monitoring-for-all-monitored-services-in-azure-ad-connect-health"></a>Azure AD Connect Health'te izlenen tüm hizmetler için veri toplama yı ve izlemeyi devre dışı
Azure AD Connect Health, kiracıdaki **tüm** kayıtlı hizmetlerin veri toplanmasını durdurma seçeneği de sunar. Harekete geçmeden önce tüm küresel yöneticilerin dikkatli bir şekilde değerlendirilmesini ve tam olarak kabul edilmesinizi öneririz. İşlem başladıktan sonra, Connect Health hizmeti tüm hizmetlerinizin verilerini almayı, işlemeyi ve raporlamayı durdurur. Connect Health hizmetindeki varolan veriler en fazla 30 gün süreyle saklanır.
Belirli bir sunucunun veri toplamasını durdurmak istiyorsanız, lütfen belirli sunucuların silinme adımlarını izleyin. Kiracı açısından veri toplamayı durdurmak için, veri toplamayı durdurmak ve kiracının tüm hizmetlerini silmek için aşağıdaki adımları izleyin.

1. Ana bıçakta yapılandırma altında **Genel Ayarlar'a** tıklayın. 
2. Bıçağın üst kısmındaki **Veri Toplamayı Durdur** düğmesine tıklayın. İşlem başladıktan sonra kiracı yapılandırma ayarlarının diğer seçenekleri devre dışı bırakılır.  
 
   ![Veri toplamayı durdurma](./media/reference-connect-health-user-privacy/gdpr4.png)
  
3. Veri toplamaları durduran yerleşik hizmetlerin listesini emin olun. 
4. **Eylem sil** düğmesini etkinleştirmek için tam kiracı adını girin
5. Tüm hizmetlerin silinmesini tetiklemek için **Sil'i** tıklatın. Connect Health, yerleşik hizmetlerinizden gönderilen verileri almayı, işlemeyi ve raporlamayı durdurur. Tüm süreç 24 saat kadar sürebilir. Bu adımın geri döndürülemez olduğuna dikkat edin. 
6. İşlem tamamlandıktan sonra, Connect Health'te artık kayıtlı hizmetler görmezsiniz. 

   ![Veri toplama durdurulduktan sonra](./media/reference-connect-health-user-privacy/gdpr5.png)

## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Azure AD Connect Health'te veri toplama ve izlemeyi yeniden etkinleştirme
Azure AD Connect Health'te daha önce silinmiş bir hizmet için izlemeyi yeniden etkinleştirmek için, tüm sunucularda [sistem durumu aracısını](how-to-connect-health-agent-install.md) kaldırmanız ve yeniden yüklemeniz gerekir.

### <a name="re-enable-data-collection-and-monitoring-for-all-monitored-services"></a>Tüm izlenen hizmetler için veri toplama ve izlemeyi yeniden etkinleştirin

Azure AD Connect Health'te kiracı bazında veri toplamaya devam edilebilir. Harekete geçmeden önce tüm küresel yöneticilerin dikkatli bir şekilde değerlendirilmesini ve tam olarak kabul edilmesinizi öneririz.

>[!IMPORTANT]
> Aşağıdaki adımlar, 24 saatlik devre dışı atma eyleminden sonra kullanılabilir olacaktır.
> Veri toplama etkinleştirildikten sonra, Connect Health'te sunulan içgörü ve izleme verileri daha önce toplanan eski verileri göstermez. 

1. Ana bıçakta yapılandırma altında **Genel Ayarlar'a** tıklayın. 
2. Bıçağın üst **kısmındaki Veri Toplamayı Etkinleştir** düğmesine tıklayın. 
 
   ![Veri toplamayı etkinleştirme](./media/reference-connect-health-user-privacy/gdpr6.png)
 
3. **Etkinleştir** düğmesini etkinleştirmek için tam kiracı adını girin.
4. Connect Health hizmetinde veri toplama izni vermek için **Etkinleştir** düğmesini tıklatın. Değişiklik kısa süre içinde uygulanacaktır. 
5. İzlenecek sunucularda aracıyı yeniden yüklemek için [yükleme işlemini](how-to-connect-health-agent-install.md) izleyin ve hizmetler portalda bulunacaktır.  


## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi'ndeki Microsoft Gizlilik ilkesini inceleyin](https://www.microsoft.com/trustcenter)
* [Azure AD Connect ve Kullanıcı Gizliliği](reference-connect-user-privacy.md)

