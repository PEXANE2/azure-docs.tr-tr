---
title: Azure Active Directory Etki Alanı Hizmetlerinin durumunu kontrol edin | Microsoft Dokümanlar
description: Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanının sistem durumunu nasıl denetlerve Azure portalını kullanarak durum iletilerini nasıl anlayacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: fa1cba2d791cd40a46f8ad182c123a726143faec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614259"
---
# <a name="check-the-health-of-an-azure-active-directory-domain-services-managed-domain"></a>Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanının sistem durumunu denetleme

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), yönetilen etki alanını sağlıklı ve güncel tutmak için bazı arka plan görevlerini yürütür. Bu görevler arasında yedekleme almak, güvenlik güncelleştirmeleri uygulamak ve Azure AD'den verileri eşitlemek yer almaktadır. Azure AD DS yönetilen etki alanıyla ilgili sorunlar varsa, bu görevler başarıyla tamamlanmamış olabilir. Herhangi bir sorunu gözden geçirmek ve çözmek için, Azure portalını kullanarak Azure AD DS yönetilen etki alanının sistem durumu durumunu denetleyebilirsiniz.

Bu makalede, Azure AD DS sistem durumu durumunu nasıl görüntülayacağınızı ve gösterilen bilgileri veya uyarıları nasıl anlayacağınızı gösterir.

## <a name="view-the-health-status"></a>Sağlık durumunu görüntüleme

Azure AD DS yönetilen etki alanının sistem durumu, Azure portalı kullanılarak görüntülenir. Son yedekleme zamanı ve Azure AD ile eşitleme hakkındaki bilgilerin yanı sıra yönetilen etki alanının durumuyla ilgili bir sorun olduğunu gösteren uyarılar görülebilir. Azure AD DS yönetilen bir etki alanının sistem durumu durumunu görüntülemek için aşağıdaki adımları tamamlayın:

1. Azure **portalında, Azure AD Etki Alanı Hizmetlerini**arayın ve seçin.
1. azure AD DS yönetilen etki alanınızı seçin( örneğin *aaddscontoso.com.*
1. Azure AD DS kaynak penceresinin sol **tarafında, Sistem Durumu'nu**seçin. Aşağıdaki örnek ekran görüntüsü, sağlıklı bir Azure AD DS yönetilen etki alanını ve son yedekleme ve Azure AD eşitleme durumunu gösterir:

    ![Azure portalında Azure Etkin Dizin Etki Alanı Hizmetleri durumunu gösteren sistem durumu sayfasına genel bakış](./media/check-health/health-page.png)

Sistem durumu sayfasının *son değerlendirilen* zaman damgası, Azure AD DS yönetilen etki alanının en son ne zaman denetlenildiğini gösterir. Azure AD DS yönetilen bir etki alanının durumu her saat başı değerlendirilir. Azure AD DS yönetilen etki alanında herhangi bir değişiklik yaparsanız, güncelleştirilmiş sistem durumu durumunu görüntülemek için bir sonraki değerlendirme döngüsüne kadar bekleyin.

Sağ üstteki durum, Azure AD DS yönetilen etki alanının genel durumunu gösterir. Durum, etki alanınızdaki varolan tüm uyarıları etkenler. Aşağıdaki tabloda kullanılabilir durum göstergeleri ayrıntılı olarak açıkları yer almaktadır:

| Durum | Simge | Açıklama |
| --- | :----: | --- |
| Çalışıyor | <img src= "./media/active-directory-domain-services-alerts/running-icon.png" width = "15" alt="Green check mark for running"> | Azure AD DS yönetilen etki alanı doğru çalışıyor ve herhangi bir kritik veya uyarı uyarısı yok. Etki alanı nın bilgi uyarıları olabilir. |
| Dikkat (uyarı) ihtiyacı | <img src= "./media/active-directory-domain-services-alerts/warning-icon.png" width = "15" alt="Yellow exclamation mark for warning"> | Azure AD DS yönetilen etki alanında kritik uyarı yoktur, ancak ele alınması gereken bir veya daha fazla uyarı uyarısı vardır. |
| Dikkat gerektirir (kritik) | <img src= "./media/active-directory-domain-services-alerts/critical-icon.png" width = "15" alt="Red exclamation mark for critical"> | Azure AD DS yönetilen etki alanında ele alınması gereken bir veya daha fazla kritik uyarı vardır. Ayrıca uyarı ve / veya bilgilendirme uyarıları olabilir. |
| Dağıtma | <img src= "./media/active-directory-domain-services-alerts/deploying-icon.png" width = "15" alt="Blue circular arrows for deploying"> | Azure AD DS etki alanı dağıtılıyor. |

## <a name="understand-monitors-and-alerts"></a>Monitörleri ve uyarıları anlama

Azure AD DS yönetilen etki alanının sistem durumu iki tür bilgi gösterir ( *monitörler*ve *uyarılar.* İzleyiciler, temel arka plan görevlerinin tamamlandığı zamanı gösterir. Uyarılar, yönetilen etki alanının kararlılığını artırmak için bilgi veya öneriler sağlar.

### <a name="monitors"></a>İzleyiciler

İzleyiciler, Azure AD DS yönetilen etki alanının düzenli olarak denetlenen alanlarıdır. Azure AD DS yönetilen etki alanı için etkin uyarılar varsa, monitörlerden birinin bir sorunu bildirmesine neden olabilir. Azure AD Etki Alanı Hizmetleri'nde şu anda aşağıdaki alanlar için monitörler bulunmaktadır:

* Backup
* Azure AD ile eşitleme

#### <a name="backup-monitor"></a>Yedek monitör

Yedekleme monitörü, Azure AD DS yönetilen etki alanının otomatik düzenli yedeklemelerinin başarıyla çalıştırıldığını denetler. Aşağıdaki tablo, kullanılabilir yedek monitör durumunu ayrıntılarıyla anlatır:

| Ayrıntı değeri | Açıklama |
| --- | --- |
| Hiç yedeklenmedi | Bu durum, yeni Azure AD DS yönetilen etki alanları için normaldir. İlk yedekleme, Azure AD DS yönetilen etki alanı dağıtıldıktan 24 saat sonra oluşturulmalıdır. Bu durum devam ederse, [bir Azure destek isteği açın.][azure-support] |
| Son yedekleme 1-14 gün önce alındı | Bu zaman aralığı yedek monitör için beklenen durumdur. Bu dönemde otomatik düzenli yedeklemeler yapılmalıdır. |
| Son destek 14 gün önce alınmış. | İki haftadan uzun bir zaman aşımı, otomatik normal yedeklemelerle ilgili bir sorun olduğunu gösterir. Etkin kritik uyarılar, Azure AD DS yönetilen etki alanının yedeklemesini engelleyebilir. Azure AD DS yönetilen etki alanı için etkin uyarıları çözümle. Yedekleme monitörü yeni bir yedeklemeyi bildirmek için durumu güncelleştirmezse, [bir Azure destek isteği açın.][azure-support] |

#### <a name="synchronization-with-azure-ad-monitor"></a>Azure AD monitörü ile senkronizasyon

Azure AD DS yönetilen bir etki alanı düzenli olarak Azure Etkin Dizini ile eşitlenir. Kullanıcı ve grup nesnelerinin sayısı ve son eşitlemeden bu yana Azure REKLAM dizininde yapılan değişikliklerin sayısı, eşitlemenin ne kadar sürdüğünü etkiler. Azure AD DS yönetilen etki alanı en son üç gün önce eşitlendiyse, etkin uyarıları denetleyin ve çözümleyin. Eşitleme monitörü, etkin uyarılara değindikten sonra son eşitleme göstermek için durumu güncelleştirmezse, [bir Azure destek isteği açın.][azure-support]

### <a name="alerts"></a>Uyarılar

Hizmetin düzgün çalışması için ele alınması gereken Azure AD DS yönetilen etki alanında sorunlar için uyarılar oluşturulur. Her uyarı sorunu açıklar ve sorunu çözmek için belirli adımları özetleyen bir URL verir. Olası uyarılar ve çözümleri hakkında daha fazla bilgi için [Sorun Giderme uyarılarına](troubleshoot-alerts.md)bakın.

Sistem durumu uyarıları aşağıdaki önem düzeylerine ayrılır:

 * **Kritik uyarılar,** Azure AD DS yönetilen etki alanını ciddi şekilde etkileyen sorunlardır. Bu uyarılar derhal ele alınmalıdır. Azure platformu, sorunlar çözülene kadar yönetilen etki alanını izleyip yönetemez, yönetemez, yarayaramaz ve eşitleme yapamaz.
 * **Uyarı uyarıları,** sorun devam ederse Azure AD DS yönetilen etki alanı işlemlerini etkileyebilecek sorunlar hakkında sizi bilgilendirir. Bu uyarılar, yönetilen etki alanını güvence altına almak için öneriler de sunar.
 * **Bilgilendirme uyarıları,** Azure AD DS yönetilen etki alanını olumsuz etkilemeyen bildirimlerdir. Bilgilendirme uyarıları, yönetilen etki alanında neler olduğuna dair bazı bilgiler sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Sistem durumu durumu sayfasında gösterilen uyarılar hakkında daha fazla bilgi için yönetilen [etki alanınızdaki uyarıları][troubleshoot-alerts] çözümle'ye bakın

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[troubleshoot-alerts]: troubleshoot-alerts.md
