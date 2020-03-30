---
title: Raporlar için Azure Monitör çalışma kitapları | Microsoft Dokümanlar
description: Azure Etkin Dizin raporları için Azure Monitor çalışma kitaplarını nasıl kullanacağınızı öğrenin.
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: 4066725c-c430-42b8-a75b-fe2360699b82
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 10/30/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 2e94d9f56a865999f9169650f621a6af892c27ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74014370"
---
# <a name="how-to-use-azure-monitor-workbooks-for-azure-active-directory-reports"></a>Azure Etkin Dizin raporları için Azure Monitor çalışma kitapları nasıl kullanılır?

> [!IMPORTANT]
> Bu çalışma kitabındaki temel sorguları en iyi duruma getirmek için lütfen "Düzenle"ye tıklayın, Ayarlar simgesine tıklayın ve bu sorguları çalıştırmak istediğiniz çalışma alanını seçin. Çalışma kitapları varsayılan olarak Azure REKLAM günlüklerinizi yönlendirmeyaptığınız tüm çalışma alanlarını seçer. 

İstiyor musunuz:

- [Koşullu Erişim ilkelerinizin](../conditional-access/overview.md) kullanıcılarınızın oturum açma deneyimi üzerindeki etkisini anladınız mı?

- Kuruluşunuzun oturum açma durumunu daha iyi görmek ve sorunları hızlı bir şekilde çözmek için oturum açma hatalarını sorun giderme?

- Ortamınızda oturum açabilmek için eski kimlik doğrulamalarını kimler kullanıyor biliyor musunuz? (Eski [kimlik doğrulamasını engelleyerek](../conditional-access/block-legacy-authentication.md)kiracınızın korumaını artırabilirsiniz.)

- Koşullu Erişim ilkelerinin kiracınızdaki etkisini anlamanız mı gerekiyor?

- Gözden geçirme olanağı ister misiniz: oturum açma günlüğü sorguları, çalışma kitabı kaç kullanıcıya erişim verildiğini veya reddedildiğini ve kaynaklara erişirken koşullu erişim ilkelerini kaç kullanıcının atlayarak geçtiğini bildiriyor?

- Daha derin bir anlayış geliştirmek le ilgileniyor musunuz: bir politikanın etkisinin aygıt platformu, aygıt durumu, istemci uygulaması, oturum açma riski, konum ve uygulama dahil olmak üzere koşul başına bağlamsallaştırılaabilmesi için koşul başına çalışma kitabı ayrıntıları?

- Oturum açma günlüğü sorguları hakkında daha derin bilgiler edinin, çalışma kitabı kaç kullanıcıya erişim verildiğini veya reddedildiğini ve kaynaklara erişirken koşullu erişim ilkelerini kaç kullanıcının atlayarak geçtiğini bildirir.

- Active Directory, bu soruları yanıtlamanıza yardımcı olmak için izleme için çalışma kitapları sağlar. [Azure Monitor çalışma kitapları](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) metin, analitik sorguları, ölçümler ve parametreleri zengin etkileşimli raporlarda birleştirir.



Bu makalede:

- [Monitör çalışma kitaplarını kullanarak etkileşimli raporların](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)nasıl oluşturulacaklarına aşina olduğunuzu varsayar.

- Koşullu Erişim ilkelerinizin etkisini anlamak, oturum açma hatalarını gidermek ve eski kimlik doğrulamalarını belirlemek için Monitor çalışma kitaplarını nasıl kullanacağınızı açıklar.
 


## <a name="prerequisites"></a>Ön koşullar

Monitor çalışma kitaplarını kullanmak için şunları yapmanız gerekir:

- Premium (P1 veya P2) lisansına sahip Aktif Dizin kiracısı. [Nasıl premium lisans alacağınızı](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-get-started-premium)öğrenin.

- Günlük [Analizi çalışma alanı.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)

- Günlük analizi çalışma alanına [erişim](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#manage-access-using-workspace-permissions)
- Azure Active Directory'deki aşağıdaki roller (Azure Active Directory portalı üzerinden Günlük Analitiği'ne erişiyorsanız)
    - Güvenlik yöneticisi
    - Güvenlik okuyucusu
    - Rapor okuyucu
    - Genel yönetici

## <a name="roles"></a>Roller
Çalışma kitaplarını yönetmek için aşağıdaki rollerden birinde olmalı ve [temel Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#manage-access-using-azure-permissions) çalışma alanına erişmelisiniz:
-   Genel yönetici
-   Güvenlik yöneticisi
-   Güvenlik okuyucusu
-   Rapor okuyucu
-   Uygulama yöneticisi

## <a name="workbook-access"></a>Çalışma kitabı erişimi 

Çalışma kitaplarına erişmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure **Etkin Dizin** > **İzleme** > **Çalışma Kitaplarına**gidin. 

1. Bir rapor veya şablon seçin veya araç çubuğunda **Aç'ı**seçin. 

![Azure AD'de Azure Monitörçalışma kitaplarını bulun](./media/howto-use-azure-monitor-workbooks/azure-monitor-workbooks-in-azure-ad.png)

## <a name="sign-in-analysis"></a>Oturum açma analizi

Oturum açma çözümlemesi çalışma kitabına **erişmek için, Kullanım** bölümünde Oturum **Açma**seçeneğini belirleyin. 

Bu çalışma kitabı aşağıdaki oturum açma eğilimlerini gösterir:

- Tüm oturum açma

- Başarılı

- Bekleyen kullanıcı eylemi

- Hata

Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma analizi](./media/howto-use-azure-monitor-workbooks/43.png)


Her eğilim için, aşağıdaki kategorilere göre bir döküm alırsınız:

- Konum

    ![Konuma göre oturum açma](./media/howto-use-azure-monitor-workbooks/45.png)

- Cihaz

    ![Cihaza göre oturum açma](./media/howto-use-azure-monitor-workbooks/46.png)


## <a name="sign-ins-using-legacy-authentication"></a>Eski kimlik doğrulamasını kullanarak oturum açma 


**Kullanım** bölümünde [eski kimlik doğrulaması](../conditional-access/block-legacy-authentication.md)kullanan oturum açma lar için çalışma kitabına erişmek **için, Eski Kimlik Doğrulaması'nı kullanarak Oturum Açma'yı**seçin. 

Bu çalışma kitabı aşağıdaki oturum açma eğilimlerini gösterir:

- Tüm oturum açma

- Başarılı


Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

- Protokoller

![Eski kimlik doğrulamaya göre oturum açma](./media/howto-use-azure-monitor-workbooks/47.png)


Her eğilim için, uygulama ve protokol e göre bir arıza elde elabilirsiniz.

![Uygulama ve protokole göre eski kimlik doğrulama oturum açma](./media/howto-use-azure-monitor-workbooks/48.png)



## <a name="sign-ins-by-conditional-access"></a>Koşullu Erişime Göre Oturum Açma 


[Koşullu Erişim ilkelerine](../conditional-access/overview.md)göre oturum açma çalışma kitabına erişmek için **Koşullu Erişim** bölümünde **Koşullu Erişim'e göre Oturum Açma**seçeneğini belirleyin. 

Bu çalışma kitabı, devre dışı oturum açma eğilimlerini gösterir. Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Koşullu Erişimle Oturum Açma İşlemleri](./media/howto-use-azure-monitor-workbooks/49.png)


Devre dışı oturum açma lar için Koşullu Erişim durumuna göre bir döküm alırsınız.

![Koşullu Erişim durumu](./media/howto-use-azure-monitor-workbooks/conditional-access-status.png)


## <a name="conditional-access-insights"></a>Koşullu Erişim İçgörüleri

### <a name="overview"></a>Genel Bakış

Çalışma kitapları, BT yöneticilerinin kiracılarındaki Koşullu Erişim ilkelerinin etkisini izlemelerine yardımcı olabilecek oturum açma günlüğü sorguları içerir. Kaç kullanıcıya erişim izni verileceğini veya erişimin reddedildiğini bildirme olanağınız vardır. Çalışma kitabı, oturum açma sırasında bu kullanıcıların özelliklerine göre koşullu erişim ilkelerini kaç kullanıcının atlamış olabileceğine ilişkin öngörüler içerir. Aygıt platformu, aygıt durumu, istemci uygulaması, oturum açma riski, konum ve uygulama dahil olmak üzere, bir ilkenin etkisinin durum başına bağlamsallaştırılaabilmesi için koşul başına ayrıntılar içerir.

### <a name="instructions"></a>Yönergeler 
Koşullu Erişim Öngörüleri çalışma kitabına erişmek için Koşullu Erişim Bölümündeki **Koşullu Erişim Öngörüleri** çalışma kitabını seçin. Bu çalışma kitabı, kiracınızdaki her Koşullu Erişim ilkesinin beklenen etkisini gösterir. Açılan listeden bir veya daha fazla Koşullu Erişim ilkeleri seçin ve aşağıdaki filtreleri uygulayarak çalışma kitabının kapsamını daraltın: 

- **Zaman Aralığı**

- **Kullanıcı**

- **Uygulamalar**

- **Veri Görünümü**

![Koşullu Erişim durumu](./media/howto-use-azure-monitor-workbooks/access-insights.png)


Etki Özeti, seçili ilkelerin belirli bir sonucu olan kullanıcı veya oturum açma sayısını gösterir. Toplam, seçili zaman aralığında seçili ilkelerin değerlendirildiği kullanıcı veya oturum açma sayısıdır. Çalışma kitabındaki verileri bu sonuç türüne göre filtrelemek için bir döşemeyi tıklatın. 

![Koşullu Erişim durumu](./media/howto-use-azure-monitor-workbooks/impact-summary.png)

Bu çalışma kitabı ayrıca, seçilen ilkelerin altı koşulun her biri tarafından ayrılmış etkisini de gösterir: 
- **Aygıt durumu**
- **Cihaz platformu**
- **İstemci uygulamaları**
- **Oturum açma riski**
- **Konum**
- **Uygulamalar**

![Koşullu Erişim durumu](./media/howto-use-azure-monitor-workbooks/device-platform.png)

Çalışma kitabında seçilen parametrelere göre filtrelenen tek tek oturum açma ları da araştırabilirsiniz. Oturum açma sıklığına göre sıralanmış tek tek kullanıcıları arayın ve ilgili oturum açma olaylarını görüntüleyin. 

![Koşullu Erişim durumu](./media/howto-use-azure-monitor-workbooks/filtered.png)





## <a name="sign-ins-by-grant-controls"></a>Hibe denetimleri ile oturum açma

**Koşullu Erişim** bölümünde hibe [denetimleri](../conditional-access/controls.md)ile oturum açma çalışma kitabına erişmek **için, Hibe Denetimleri ile Oturum Açma'yı**seçin. 

Bu çalışma kitabı aşağıdaki devre dışı oturum açma eğilimlerini gösterir:

- MFA gerektirme
 
- Kullanım koşullarını gerekli kılma

- Gizlilik bildirimi ni talep edin

- Diğer


Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Hibe denetimleri ile oturum açma](./media/howto-use-azure-monitor-workbooks/50.png)


Her eğilim için, uygulama ve protokol e göre bir arıza elde elabilirsiniz.

![Son oturum açmaların dökümü](./media/howto-use-azure-monitor-workbooks/51.png)




## <a name="sign-ins-failure-analysis"></a>Oturum açma hatası analizi

Aşağıdakilerle ilgili hataları gidermek için **Oturum Açma hata analizi** çalışma kitabını kullanın:

- Oturum açma işlemleri
- Koşullu Erişim ilkeleri
- Eski kimlik doğrulaması 


Sorun **Giderme** bölümünde, Koşullu Erişim verilerine göre oturum açma lara erişmek için **Eski Kimlik Doğrulaması'nı kullanarak Oturum Açma'yı**seçin. 

Bu çalışma kitabı aşağıdaki oturum açma eğilimlerini gösterir:

- Tüm oturum açma

- Başarılı

- Bekleyen eylem

- Hata


Her eğilimi aşağıdaki kategorilere göre filtreleyebilirsiniz:

- Zaman aralığı

- Uygulamalar

- Kullanıcılar

![Oturum açma sorunları](./media/howto-use-azure-monitor-workbooks/52.png)


Azure Monitor, oturum açma ile ilgili sorun gidermenize yardımcı olmak için aşağıdaki kategorilere göre bir döküm sağlar:

- En iyi hatalar

    ![En iyi hataların özeti](./media/howto-use-azure-monitor-workbooks/53.png)

- Kullanıcı eylemini bekleyen oturum açma

    ![Kullanıcı eyleminde bekleyen oturum açmaların özeti](./media/howto-use-azure-monitor-workbooks/54.png)






## <a name="next-steps"></a>Sonraki adımlar

[Monitör çalışma kitaplarını kullanarak etkileşimli raporlar oluşturun.](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks)
