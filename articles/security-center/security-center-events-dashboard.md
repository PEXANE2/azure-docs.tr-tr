---
title: Azure Güvenlik Merkezi 'nde güvenlik olaylarını izleme ve işleme | Microsoft Docs
description: Azure sanal makinelerinizden ve Azure olmayan bilgisayarlardan güvenlik olaylarını görmek için Güvenlik Merkezi 'nin olaylar panosunu nasıl kullanabileceğinizi öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: rkarlin
ms.openlocfilehash: 389aaee621251890cd3f75744a94b9c9b29c5695
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662392"
---
# <a name="monitoring-and-processing-security-events-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde güvenlik olaylarını izleme ve işleme
Olaylar panosu, zaman içinde toplanan güvenlik olaylarının sayısına ve ilgilenmeniz gerektirebilecek bir önemli olayları listesine genel bakış sağlar.  

> [!NOTE]
> Güvenlik olayları panosu 31 Temmuz 2019 tarihinde kullanımdan kaldırılmıştır. Daha fazla bilgi ve diğer hizmetler için bkz. [Güvenlik Merkezi özelliklerini devre dışı bırakma (2019 Temmuz)](security-center-features-retirement-july2019.md#menu_events).

## <a name="what-is-a-security-event"></a>Güvenlik olayı nedir?
Güvenlik Merkezi, makinelerinizdeki güvenlikle ilgili çeşitli yapılandırma ve olayları toplamak için Microsoft Monitoring Agent kullanır ve bu olayları çalışma alanınızda depolar. Bu verilere örnek olarak şunlar verilebilir: işletim sistemi günlükleri (Windows olay günlükleri), çalışan süreçler ve Güvenlik Merkezi ile tümleştirilmiş güvenlik çözümlerinden olaylar. Microsoft Monitoring Agent ayrıca kilitlenme bilgi dökümü dosyalarını da çalışma alanlarınıza kopyalar.

## <a name="requirements"></a>Gereksinimler
Bu özelliği kullanmak için çalışma alanınızın sürüm 2 Log Analytics çalışıyor olması ve Güvenlik Merkezi 'nin standart katmanında olması gerekir. Standart katman hakkında daha fazla bilgi için Güvenlik Merkezi [fiyatlandırma sayfasına](security-center-pricing.md) bakın.

## <a name="events-processed-dashboard"></a>İşlenen olaylar panosu
**Olaylar** panosuna Güvenlik Merkezi ana menüsünde veya Güvenlik Merkezi **'ne genel bakış** dikey penceresinden erişebilirsiniz.  

![İşlenen olaylar panosu][1]

**Güvenlik Merkezi** altındaki **Olaylar** kutucuğu, Azure sanal makinelerinizden ve Azure olmayan bilgisayarlardan Güvenlik Merkezi 'ne akan olay sayısını görüntüler.

**Olaylar panosu** , fazla mesainin işlendiği olay sayısına ve olayların listesine bir genel bakış sağlar.

 ![Pano][2]

 Panonun üst yarısında, geçen hafta içinde işlenen tüm olaylar eğilimler. Panonun alt yarısında önemli olayları ve tüm olaylar türe göre listelenmiştir:

 - **Önemli olaylar** , güvenlik merkezi 'nin sağladığı olay sorgularını ve oluşturduğunuz ve eklediğiniz olay sorgularını içerir. Pano Ayrıca her bir önemli olayının sayısına hızlı bir görünüm sağlar.
 - **Türe göre tüm olaylar** , alınmakta olan olay türlerini ve her bir tür için bir sayıyı gösterir. Olay türü örnekleri SecurityEvent, CommonSecurityLog, WindowsFirewall ve W3CIISLog.

> [!NOTE]
> Önemli olaylar [Web temeli değerlendirmesini](https://docs.microsoft.com/azure/operations-management-suite/oms-security-web-baseline-assessment)içerir. Web Temeli değerlendirmesinin amacı, savunmasız olabilecek web sunucusu ayarlarını bulmaktır.

## <a name="view-processed-event-details"></a>İşlenen olay ayrıntılarını görüntüle
1. **Güvenlik Merkezi** ana menüsünde **Olaylar**' ı seçin.
2. **Olaylar panosu** çalışma alanı seçici açılabilir. Yalnızca bir çalışma alanınız varsa, bu çalışma alanı Seçicisi görünmez. Birden fazla çalışma alanınız varsa, işlenen olay ayrıntılarını görüntülemek için bir çalışma alanı seçmeniz gerekir. Birden fazla çalışma alanınız varsa listeden bir çalışma alanı seçin.

   ![Çalışma alanı listesi][3]

3. **Olaylar panosu** , seçili çalışma alanı için olay ayrıntılarını gösteren açılır. Önemli olaylarını ve tüm olayları türüne göre görüntüleyebilirsiniz.  Bu örnekte, **Notable olayları**seçtik.

   ![Önemli etkinlik][4]

4. Bir olay türü seçerek çalışma alanı altında daha fazla veri sorgulama yapabilirsiniz. Bu örnekte, **Securityevent**' i seçtik.

   ![Olay türü seçme][5]

5. **Günlük araması** , olay türü üzerinde ek ayrıntılarla açılır.

   ![Günlük araması][6]

## <a name="add-a-notable-event"></a>Önemli olayı ekleme
Güvenlik Merkezi, kullanıma hazır önemli olayları sağlar. [Kusto sorgu dilini](../log-analytics/log-analytics-search-reference.md)kullanarak kendi sorgunuzu temel alarak önemli olayları ekleyebilirsiniz. Önemli bir olay eklemek için **Olaylar panosuna** geri döneceğiz.

1. **Notable olayı Ekle**' yi seçin.

   ![Önemli olayı ekleme][7]

2. **Özel önemli olay Ekle** açılır.  **Görünen ad**' ın altında, önemli olaylarınız için bir ad girin. **Arama sorgusu**altında olay için sorgunuzu girin.

   ![Sorgunuzu girin][8]

4. **Tamam**’ı seçin.

## <a name="update-your-workspace-for-events-processing"></a>Olay işleme için çalışma alanınızı güncelleştirme
Çalışma alanınızın sürüm 2 ' Log Analytics çalışıyor olması ve Güvenlik Merkezi 'nde olay işlemenin kullanılması için Güvenlik Merkezi 'nin standart katmanında olması gerekir. **Olaylar panosu** çalışma alanı Seçicisi, bu gereksinimleri karşılamayan çalışma alanlarını tanımlar.

![Çalışma alanı gereksinimleri karşılamıyor][9]

Çalışma alanı satırı:

- İçerir **güncelleştirme gerektiriyor** -çalışma alanınızı Log Analytics sürüm 2 ' ye güncelleştirmeniz gerekir
- **Yükseltme planını** içerir – çalışma alanınızı Güvenlik Merkezi 'nin standart katmanına yükseltmeniz gerekir
- Boş-çalışma alanınız gereksinimleri karşılıyor ve bir çalışma alanı seçmek sizi panoya götürür

> [!NOTE]
> Olaylar **panosu**' nun altında, **Olaylar** sütunu her çalışma alanındaki olay miktarını gösterir.  Bu sütun, bazı çalışma alanları için boştur çünkü güvenlik merkezi 'nin ücretsiz katmanı bu çalışma alanına uygulandı. Güvenlik Merkezi, ücretsiz katman altında olayları toplayacak, ancak olaylar Azure Izleyici günlüklerine kaydedilmez ve panoda kullanılamaz.
>
>

## <a name="update-workspace-to-log-analytics-version-2"></a>Çalışma alanını Log Analytics sürüm 2 ' ye Güncelleştir
1. **Güncelleştirme gerektiren**bir çalışma alanı seçin.
2. **Arama yükseltmesi** açılır. **Şimdi Yükselt**' i seçin.

   ![Hemen yükseltin][10]

## <a name="upgrade-to-security-centers-standard-tier"></a>Güvenlik Merkezi 'nin standart katmanına yükselt
1. **Yükseltme planına**sahip bir çalışma alanı seçin.
2. **Olaylar panosu** açılır. **Olayları dene panosunu**seçin.

   ![Panoyu dene][11]

3. **Gelişmiş güvenlik 'e ekleme**altında, yükseltmekte olduğunuz çalışma alanını seçin.
4. **Fiyatlandırma**altında **Standart**' ı seçin.
5. **Kaydet**’i seçin.

   ![Standart katmana yükselt][12]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi 'nin olay panosunu nasıl kullanacağınızı öğrendiniz. Panonun nasıl çalıştığı hakkında daha fazla bilgi edinmek ve kendi olay sorgularınızı yazmak için bkz.:

- [Azure Izleyici günlükleri nedir?](../log-analytics/log-analytics-overview.md) – Azure Izleyici günlüklerine genel bakış
- [Kusto ' de günlük aramalarını anlama](../log-analytics/log-analytics-log-search-new.md) -günlük aramalarının Azure izleyici günlüklerinde nasıl kullanıldığını açıklar ve günlük araması oluşturmadan önce anlaşılması gereken kavramlar sağlar
- [Kusto arama başvurusu](../log-analytics/log-analytics-search-reference.md) – günlükteki sorgu dilini kullanarak kendi olay sorgularınızı nasıl yazacağınızı öğrenin

Güvenlik Merkezi hakkında daha fazla bilgi için bkz.

- [Güvenlik Merkezi 'Ne genel bakış](security-center-intro.md) – Güvenlik Merkezi 'nin temel yeteneklerini açıklar

<!--Image references-->
[1]: ./media/security-center-events-dashboard/events-processed.png
[2]: ./media/security-center-events-dashboard/dashboard.png
[3]: ./media/security-center-events-dashboard/view-processed-event.png
[4]: ./media/security-center-events-dashboard/notable-event.png
[5]: ./media/security-center-events-dashboard/events-by-type.png
[6]: ./media/security-center-events-dashboard/log-search-detail.png
[7]: ./media/security-center-events-dashboard/add-notable-event.png
[8]: ./media/security-center-events-dashboard/create-query.png
[9]: ./media/security-center-events-dashboard/requires-update.png
[10]: ./media/security-center-events-dashboard/search-upgrade.png
[11]: ./media/security-center-events-dashboard/try-dashboard.png
[12]: ./media/security-center-events-dashboard/onboard-workspace.png
