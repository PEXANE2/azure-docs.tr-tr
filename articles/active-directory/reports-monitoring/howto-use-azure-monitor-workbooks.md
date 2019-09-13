---
title: Azure Active Directory raporları için Azure Izleyici çalışma kitaplarını kullanma | Microsoft Docs
description: Azure Active Directory raporları için Azure Izleyici çalışma kitaplarını nasıl kullanacağınızı öğrenin.
services: active-directory
author: cawrites
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.openlocfilehash: 288fa54a1a6dd4eb05f953a4490bf7736d6d7ff8
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70931241"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Azure Active Directory raporları için Azure Izleyici çalışma kitaplarını kullanma

Şunları yapmak istiyor musunuz:

- [Koşullu erişim ilkelerinizin](../conditional-access/overview.md) Kullanıcı oturum açma deneyiminizde etkisini anlayın.

- Kuruluşunuzun oturum açma durumu hakkında daha iyi bir görünüm almak ve sorunları hızlıca çözmek için oturum açma hataları sorunlarını giderin.

- Ortamınızda oturum açmak için kimin eski kimlik doğrulamaları kullandığını öğrenin. ( [Eski kimlik doğrulamasını engelleyerek](../conditional-access/block-legacy-authentication.md)kiracınızın korumasını geliştirebilirsiniz.)

Bu soruları ele almanıza yardımcı olmak Active Directory, izleme için çalışma kitapları sağlar. [Azure izleyici çalışma kitapları](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) metin, analiz sorguları, ölçümler ve parametreleri zengin etkileşimli raporlara birleştirir. 

Bu makalede:

- [İzleme çalışma kitaplarını kullanarak etkileşimli raporlar oluşturma](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)hakkında bilgi sahibi olduğunuz varsayılır.

- Koşullu erişim ilkelerinizin etkisini anlamak, oturum açma hatalarıyla ilgili sorunları gidermek ve eski kimlik doğrulamaları belirlemek için çalışma kitaplarının nasıl kullanılacağını açıklar.
 


## <a name="prerequisites"></a>Önkoşullar

Izleme çalışma kitaplarını kullanmak için şunlar gerekir:

- Premium (P1 veya P2) lisansıyla bir Active Directory kiracısı. [Premium lisans alma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium)hakkında bilgi edinin.

- [Log Analytics çalışma alanı](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

## <a name="roles"></a>Roller
Çalışma kitaplarını yönetmek için aşağıdaki rollerden birinde ve [temel alınan Log Analytics](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-access#manage-access-using-azure-permissions) çalışma alanına erişiminizin olması gerekir:
-   Genel Yönetici
-   Güvenlik Yöneticisi
-   Güvenlik Okuyucusu
-   Rapor okuyucu
-   Uygulama Yöneticisi


## <a name="workbook-access"></a>Çalışma kitabı erişimi 

Çalışma kitaplarına erişmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol gezinti bölmesinde **Azure Active Directory**' yi seçin.

3. **İzleme** bölümünde **çalışma kitapları**' nı seçin. 

    ![Öngörüler seçin](./media/howto-use-azure-monitor-workbooks/41.png)

4. Bir rapor veya şablon seçin ya da araç çubuğunda **Aç**' ı seçin. 

    ![Aç'ı seçin](./media/howto-use-azure-monitor-workbooks/42.png)


## <a name="sign-in-analysis"></a>Oturum açma Analizi

Oturum açma çözümleme çalışma kitabına erişmek için **kullanım** bölümünde **oturum açma**işlemleri ' ni seçin. 

Bu çalışma kitabı aşağıdaki oturum açma eğilimlerini gösterir:

- Tüm oturum açma işlemleri

- Başarılı

- Bekleyen kullanıcı eylemi

- Hata

Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma Analizi](./media/howto-use-azure-monitor-workbooks/43.png)


Her eğilim için, aşağıdaki kategorilere göre bir döküm alırsınız:

- Location

    ![Konuma göre oturum açma işlemleri](./media/howto-use-azure-monitor-workbooks/45.png)

- Cihaz

    ![Cihaza göre oturum açma işlemleri](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Eski kimlik doğrulaması kullanan oturum açma işlemleri 


[Eski kimlik doğrulaması](../conditional-access/block-legacy-authentication.md)kullanan oturum açma işlemleri için çalışma kitabına erişmek üzere, **kullanım** bölümünde **eski kimlik doğrulamasını kullanarak oturum açma**işlemlerini seçin. 

Bu çalışma kitabı aşağıdaki oturum açma eğilimlerini gösterir:

- Tüm oturum açma işlemleri

- Başarılı


Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

- Protokoller

![Eski kimlik doğrulamasına göre oturum açma işlemleri](./media/howto-use-azure-monitor-workbooks/47.png)


Her eğilim için, uygulama ve protokole göre bir döküm alırsınız.

![Uygulama ve protokole göre eski-kimlik doğrulama oturum açma işlemleri](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Koşullu erişime göre oturum açma işlemleri 


Koşullu erişim [ilkelerine](../conditional-access/overview.md)göre oturum açma işlemleri için çalışma kitabına erişmek **üzere koşullu erişim bölümünde,** **koşullu erişime göre oturum açma**işlemlerini seçin. 

Bu çalışma kitabı, devre dışı bırakılan oturum açma eğilimlerini gösterir. Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Koşullu Erişimle Oturum Açma İşlemleri](./media/howto-use-azure-monitor-workbooks/49.png)


Devre dışı bırakılan oturum açma işlemleri için, koşullu erişim durumuna göre bir döküm alırsınız.

![Koşullu erişim durumu](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)








## <a name="sign-ins-by-grant-controls"></a>Atama denetimlerine göre oturum açma işlemleri

Çalışma kitabına [denetimler](../conditional-access/controls.md)vererek oturum açma işlemleri için erişmek üzere, **koşullu erişim** bölümünde, **Denetim izni vererek oturum açma**işlemlerini seçin. 

Bu çalışma kitabı, aşağıdaki devre dışı oturum açma eğilimlerini gösterir:

- MFA gerektirme
 
- Kullanım koşullarını gerekli kılma

- Gizlilik bildirimi iste

- Diğer


Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Atama denetimlerine göre oturum açma işlemleri](./media/howto-use-azure-monitor-workbooks/50.png)


Her eğilim için, uygulama ve protokole göre bir döküm alırsınız.

![Son oturum açma işlemlerinin dökümünü yapın](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Oturum açma hatası Analizi

Hata gidermek için **oturum açma hatası çözümleme** çalışma kitabını aşağıdaki gibi kullanın:

- Oturum açma işlemleri
- Koşullu erişim ilkeleri
- Eski kimlik doğrulaması 


Koşullu erişim verilerine göre oturum açma işlemleri için, **sorun giderme** bölümünde, **eski kimlik doğrulamasını kullanarak oturum açma**işlemlerini seçin. 

Bu çalışma kitabı aşağıdaki oturum açma eğilimlerini gösterir:

- Tüm oturum açma işlemleri

- Başarılı

- Bekleyen eylem

- Hata


Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma sorunlarını giderme](./media/howto-use-azure-monitor-workbooks/52.png)


Azure Izleyici, oturum açma sorunlarını gidermenize yardımcı olmak için aşağıdaki kategorilere göre bir döküm sağlar:

- En önemli hatalar

    ![En üstteki hataların Özeti](./media/howto-use-azure-monitor-workbooks/53.png)

- Kullanıcı eylemini bekleyen oturum açma işlemleri

    ![Kullanıcı eylemi için bekleyen oturum açma işlemlerinin Özeti](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Sonraki adımlar

[İzleme çalışma kitaplarını kullanarak etkileşimli raporlar oluşturun](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks).
