---
title: Azure AD Domain Services-yönetilen etki alanınız için sistem durumunu kontrol edin | Microsoft Docs
description: Azure portal sistem durumu sayfasını kullanarak yönetilen etki alanınız için sistem durumunu kontrol edin.
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 6b808126fe4366d3ca3cc19c674b489ec3055665
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234149"
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen bir etki alanının durumunu denetleme

## <a name="overview-of-the-health-page"></a>Sağlık sayfasına genel bakış
Azure portal sistem durumu sayfasını kullanarak, yönetilen etki alanında neler olduğunu takip edebilirsiniz. Bu makale, sistem durumu sayfasının öğelerine kılavuzluk eder.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>Yönetilen etki alanının durumunu görüntüleme
1. Azure portal [Azure AD Domain Services sayfasına](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) gidin.
2. Durumunu görüntülemek istediğiniz etki alanına tıklayın.
3. Sol taraftaki Gezinti bölmesinde **sistem durumu**' na tıklayın.

Aşağıdaki resimde bir örnek sistem durumu sayfası gösterilmektedir: ![Örnek sistem durumu sayfası](./media/active-directory-domain-services-alerts/health-page.png)

>[!NOTE]
> Yönetilen etki alanı sistem sağlığı her saat değerlendirilir. Yönetilen etki alanında değişiklikler yaptıktan sonra, yönetilen etki alanının güncelleştirilmiş sistem durumunu görüntülemek için bir sonraki değerlendirme döngüsüne kadar bekleyin. Sağ üst köşedeki "son değerlendirilen" zaman damgası, yönetilen etki alanının sistem durumunun en son ne zaman değerlendirileceğini gösterir.
>

### <a name="status-of-your-managed-domain"></a>Yönetilen etki alanının durumu
Sistem durumu sayfanızın sağ üst kısmındaki durum, yönetilen etki alanının genel sistem durumunu gösterir. Etki alanındaki tüm mevcut uyarılarda durum faktörleri. Ayrıca, Azure AD Domain Services Genel Bakış sayfasında etki alanının durumunu da görüntüleyebilirsiniz.

| Durum | Simge | Açıklama |
| --- | :----: | --- |
| Çalışıyor | <img src= "./media/active-directory-domain-services-alerts/running-icon.png" width = "15" alt="Green check mark for running"> | Yönetilen etki alanınız düzgün çalışıyor ve kritik veya uyarı uyarıları yok. Bu etki alanında bilgilendirici uyarılar olabilir. |
| İlgilenilmesi gerekiyor (uyarı) | <img src= "./media/active-directory-domain-services-alerts/warning-icon.png" width = "15" alt="Yellow exclamation mark for warning"> | Yönetilen etki alanında kritik uyarı yok, ancak giderilmesi gereken bir veya daha fazla uyarı uyarısı var. |
| İlgilenilmesi gerekiyor (kritik) | <img src= "./media/active-directory-domain-services-alerts/critical-icon.png" width = "15" alt="Red exclamation mark for critical"> | Yönetilen etki alanında bir veya daha fazla kritik uyarı var. Ayrıca uyarı ve/veya bilgilendirici uyarılarınız olabilir. |
| Dağıtılmasını | <img src= "./media/active-directory-domain-services-alerts/deploying-icon.png" width = "15" alt="Blue circular arrows for deploying"> | Etki alanınız dağıtım sürecinde. |

## <a name="monitors"></a>İzleyiciler
İzleyiciler, yönetilen etki alanınız tarafından düzenli olarak Azure AD Domain Services izleyicilerin yönlerini oluşturur. İzleyicilerini sağlıklı durumda tutmanın en iyi yolu, yönetilen etki alanınız için etkin uyarıların çözümlenme yöntemidir.

Azure AD Domain Services Şu anda şunları izler:
 - Backup
 - Azure AD ile eşitleme

### <a name="the-backup-monitor"></a>' Backup ' İzleyicisi
Bu, yönetilen etki alanınızı düzenli olarak gerçekleştirilen yedeklemelerin gerçekleştirilip gerçekleştirilmediğini izler. Aşağıdaki tabloda, yedekleme izleyicisinin Ayrıntılar sütununda bekleneceğiniz açıklanmaktadır:

| Ayrıntı değeri | Açıklama |
| --- | --- |
|"Hiç yedeklenmedi" | Yeni oluşturulan bir yönetilen etki alanı için bu durum normaldir. Genellikle, ilk yedekleme, yönetilen etki alanınız sağlandıktan sonra 24 saat sonra oluşturulur. Yönetilen etki alanınız yeni oluşturulmadıysa veya olağandışı bir süre boyunca bu durumu görüyorsanız, [desteğe başvurun](contact-us.md). |
| Son yedekleme 1-14 gün önce gerçekleştirildi | Genel olarak, bu değer yedekleme İzleyicisi için beklenmektedir. |
| Son yedekleme 14 günden daha önce alındı. | Son yedeklememenizden bu yana iki haftadan daha uzun süre alışılmadık bir süredir. Etkin kritik uyarılar, yönetilen etki alanınızı düzenli olarak yedeklenmesini engelleyebilir. İlk olarak, yönetilen etki alanınız için etkin uyarıları çözümleyin ve sorun devam ederse [desteğe başvurun](contact-us.md). |


### <a name="the-synchronization-with-azure-ad-monitor"></a>' Azure AD ile eşitleme ' İzleyicisi
Microsoft, yönetilen etki alanınızı Azure Active Directory ile ne sıklıkta eşitleneceğini izler. Son eşitlemeden bu yana, her ikisi de bir eşitleme döneminin ne kadar sürebileceğinden, nesne sayısı (kullanıcılar & Gruplar) ve Azure AD dizininizde yapılan değişikliklerin sayısı. Yönetilen etki alanınız üç gün önce son kez eşitleniyorsa [desteğe başvurun](contact-us.md).

## <a name="alerts"></a>Uyarılar
Yönetilen etki alanında Azure AD Domain Services çalışması için giderilmesi gereken sorunlar için uyarılar oluşturulur. Her uyarı sorunu açıklar ve sorunu çözmek için belirli adımları özetleyen bir çözümleme URL 'SI sağlar. Tüm uyarıları ve bunların çözümlerini görüntülemek için [sorun giderme uyarıları](troubleshoot-alerts.md) makalesini ziyaret edin.

### <a name="alert-severity"></a>Uyarı önem derecesi
Uyarılar üç farklı önem düzeyine kategorize edilir: kritik, uyarı ve bilgilendirme.

 * **Kritik uyarılar** , yönetilen etki alanınızı ciddi bir şekilde etkileyen sorunlardır. Bu uyarılar, Microsoft tarafından yönetilen etki alanınızı izleyemediği, yönetmediğinden, düzeltme eki uygulama ve eşitlemeyeceğinden hemen değinilmesi gerekir. 
 * **Uyarı uyarıları** , gelecekte yönetilen etki alanınızı etkileyebilecek sorunları bilgilendirir. Bu uyarılar, yönetilen etki alanınızı güvenli hale getirmeye yönelik öneriler sunar.
 * **Bilgilendirici uyarılar** , etki alanınızı olumsuz şekilde etkilemeden oluşan bildirimlerdir. Bilgilendirici uyarılar, etki alanınızı ve Azure AD Domain Services neler olduğunu öğrenmek için tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanındaki uyarıları çözümleyin](troubleshoot-alerts.md)
- [Azure AD Domain Services hakkında daha fazla bilgi edinin](overview.md)
- [Ürün ekibine başvurun](contact-us.md)
