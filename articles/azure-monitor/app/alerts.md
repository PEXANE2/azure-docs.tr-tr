---
title: Azure Uygulama Öngörülerinde Uyarıları Ayarlama
description: Web uygulamanızdaki yavaş yanıt süreleri, özel durumlar ve diğer performans veya kullanım değişiklikleri hakkında bilgilendirilin.
ms.topic: conceptual
ms.date: 01/23/2019
ms.reviewer: lagayhar
ms.subservice: alerts
ms.openlocfilehash: 28fd59556a586b85a6d3caf188d9e02c11d31e3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295089"
---
# <a name="set-alerts-in-application-insights"></a>Uygulama Öngörülerinde Uyarıları Ayarlama

[Azure Uygulama Öngörüleri,][start] web uygulamanızdaki performans veya kullanım ölçümlerindeki değişiklikler konusunda sizi uyarabilir. 

Application Insights, performans sorunlarını tanılamanıza ve kullanım kalıplarını anlamanıza yardımcı olmak için canlı uygulamanızı [çok çeşitli platformlarda][platforms] izler.

Birden çok tür de uyarı vardır:

* [**Metrik uyarılar,**](../../azure-monitor/platform/alerts-metric-overview.md) bir metnin yanıt süreleri, özel durum sayıları, CPU kullanımı veya sayfa görünümleri gibi bir eşik değerini ne zaman geçtiğini gösterir.
* [**Günlük Uyarıları,**](../../azure-monitor/platform/alerts-unified-log.md) uyarı sinyalinin özel bir Kusto sorgusuna dayandığı uyarıları tanımlamak için kullanılır.
* [**Web testleri,**][availability] sitenizin internette kullanılamadığı veya yavaş yanıt verdiğini söyler. [Daha fazla bilgi edinin][availability].
* [**Proaktif tanılama,**](../../azure-monitor/app/proactive-diagnostics.md) olağandışı performans desenleri hakkında sizi bilgilendirmek için otomatik olarak yapılandırılır.

## <a name="how-to-set-an-exception-alert-using-custom-log-search"></a>Özel günlük aramasını kullanarak özel durum uyarısı nasıl ayarlanır?

Bu bölümde, sorgu tabanlı özel durum uyarısı nasıl ayarlanır üzerinden gidecek. Bu örnekte, son 24 saat içinde başarısız oran %10'dan büyük olduğunda bir uyarı istediğimizi varsayalım.

1. Azure portalındaki Application Insight kaynağınıza gidin.
2. Solda, yapılandırılan **Uyarı'ya**tıklayın.

    ![Yapılatı tıklama uyarısı altında solda](./media/alerts/1appinsightalert.png)

3. Uyarı sekmesinin üst kısmında **Yeni uyarı kuralını**seçin.

     ![Uyarı sekmesinin üst kısmında yeni uyarı kuralını tıklatın](./media/alerts/2createalert.png)

4. Kaynağınız otomatik olarak seçilmelidir. Bir koşul ayarlamak için **koşul ekle'yi**tıklatın.

    ![Koşul ekle'yi tıklatın](./media/alerts/3addcondition.png)

5. Yapılandırma sinyali mantığı sekmesinde **Özel günlük aramasını** seçin

    ![Özel günlük arama'ya tıklayın](./media/alerts/4customlogsearch.png)

6. Özel günlük arama sekmesinde, "Arama sorgusu" kutusuna sorgunuzu girin. Bu örnekiçin, aşağıdaki Kusto sorgusunu kullanacağız.
    ```kusto
    let percentthreshold = 10;
    let period = 24h;
    requests
    | where timestamp >ago(period)
    | summarize requestsCount = sum(itemCount)
    | project requestsCount, exceptionsCount = toscalar(exceptions | where timestamp >ago(period) | summarize sum(itemCount))
    | extend exceptionsRate = toreal(exceptionsCount)/toreal(requestsCount) * 100
    | where exceptionsRate > percentthreshold

    ```

    ![Arama sorgu kutusuna sorgu yazın](./media/alerts/5searchquery.png)
    
    > [!NOTE]
    > Bu adımları diğer sorgu tabanlı uyarı türlerine de uygulayabilirsiniz. Bu Kusto sorgu dili hakkında daha fazla bilgi edinebilirsiniz bu [Kusto doc](https://docs.microsoft.com/azure/kusto/concepts/) veya [Kusto hile sayfası bu SQL](https://docs.microsoft.com/azure/kusto/query/sqlcheatsheet) başlarken

7. "Uyarı mantığı" altında, sonuç sayısına mı yoksa metrik ölçüme mi dayalı olduğunu seçin. Sonra durumu (daha büyük, eşit, daha az) ve bir eşik seçin. Bu değerleri değiştirirken, koşul önizleme cümlesi değişikliklerini fark edebilirsiniz. Bu örnekte "eşit" kullanıyoruz.

    ![Uyarı mantığı altında, duruma ve koşula göre sağlanan seçenekler arasından seçim yapın, ardından bir eşik yazın](./media/alerts/6alertlogic.png)

8. "Değerlendirildi based" altında, dönemi ve sıklığı ayarlayın. Buradaki dönem, yukarıdaki sorguda dönem için koyduğumuz değerle eşleşmelidir. Sonra **bitti**tıklayın.

    ![En altta nokta ve frekansı ayarlayın ve ardından bitti'yi tıklatın](./media/alerts/7evaluate.png)

9. Şimdi tahmini aylık maliyet ile oluşturduğunuz durumu görüyoruz. Aşağıda ["Eylem Grupları"](../platform/action-groups.md) altında yeni bir grup oluşturabilir veya varolan bir grup seçebilirsiniz. İsterseniz eylemleri özelleştirebilirsiniz.

    ![eylem grubunun altındaki seç veya oluştur düğmelerine tıklayın](./media/alerts/8actiongroup.png)

10. Son olarak uyarı bilgilerinizi ekleyin (uyarı kuralı adı, açıklama, önem derecesi). İşiniz bittiğinde, en altta **uyarı kuralı oluştur'u** tıklatın.

    ![Uyarı ayrıntısı altında uyarı kural adınızı yazın, bir açıklama yazın ve bir önem seçin](./media/alerts/9alertdetails.png)

## <a name="how-to-unsubscribe-from-classic-alert-e-mail-notifications"></a>Klasik uyarı e-posta bildirimlerinden aboneliğinizi iptal etme

Bu bölüm **klasik kullanılabilirlik uyarıları,** **klasik Application Insights metrik uyarıları**ve klasik arıza **anomaliuyarıları**için geçerlidir.

Aşağıdakilerden herhangi biri geçerliyse, bu klasik uyarılar için e-posta bildirimleri alırsınız:

* E-posta adresiniz, bildirim kuralı ayarlarında Bildirim e-posta alıcıları alanında listelenir.

* Abonelik için belirli rollere sahip kullanıcılara e-posta bildirimleri gönderme seçeneği etkinleştirilir ve söz konusu Azure aboneliği için ilgili bir rol tutarsınız.

![Uyarı bildirimi ekran görüntüsü](./media/alerts/alert-notification.png)

Güvenliğinizi ve gizliliğinizi daha iyi kontrol etmek için genellikle **Bildirim e-posta alıcıları** alanında klasik uyarılarınız için bildirim alıcılarını açıkça belirtmenizi öneririz. Geriye dönük uyumluluk için belirli rollere sahibi olan tüm kullanıcıları bilgilendirme seçeneği sağlanır.

Belirli bir uyarı kuralı tarafından oluşturulan e-posta bildirimlerinden aboneliğinizi kaldırmak için, e-posta adresinizi **Bildirim e-posta alıcıları** alanından kaldırın.

E-posta adresiniz açıkça listelenmemişse, belirli rollerin tüm üyelerini otomatik olarak bilgilendirme seçeneğini devre dışı kalmanızı ve bunun yerine Bildirim e-postasında bu uyarı kuralı için bildirim alması gereken tüm kullanıcı e-postalarını listele etmenizi öneririz alıcılar alanı.

## <a name="who-receives-the-classic-alert-notifications"></a>(Klasik) uyarı bildirimlerini kim alır?

Bu bölüm yalnızca klasik uyarılar için geçerlidir ve yalnızca istediğiniz alıcıların bildirim aldığından emin olmak için uyarı bildirimlerinizi optimize etmenize yardımcı olur. [Klasik uyarılar](../platform/alerts-classic.overview.md) ve yeni uyarılar deneyimi arasındaki fark hakkında daha fazla bilgi için [uyarılara genel bakış makalesine](../platform/alerts-overview.md)bakın. Yeni uyarılar deneyiminde uyarı bildirimini denetlemek için [eylem gruplarını](../platform/action-groups.md)kullanın.

* Klasik uyarı bildirimleri için belirli alıcıların kullanılmasını öneririz.

* Herhangi bir Uygulama Öngörüsü ölçümlerindeki (kullanılabilirlik ölçümleri dahil) uyarılar için, **toplu/grup** onay kutusu seçeneği, etkinse, abonelikte sahibi, katılımcısı veya okuyucu rolü olan kullanıcılara gönderir. Sonuç olarak, abonelik erişimi olan _tüm_ kullanıcılar Uygulama Öngörüleri kaynağı kapsamındadır ve bildirim alırsınız.

> [!NOTE]
> Şu anda **toplu/grup** onay kutusu seçeneğini kullanıyorsanız ve devre dışı ederseniz, değişikliği geri alamazsınız.

Kullanıcıları rollerine göre bilgilendirmeniz gerekiyorsa, yeni uyarı deneyimini/neredeyse gerçek zamanlı uyarıları kullanın. [Eylem gruplarıyla,](../platform/action-groups.md)e-posta bildirimlerini katkıda bulunan/sahip/okuyucu rollerinden herhangi biriyle kullanıcılara yapılandırabilirsiniz (tek bir seçenek olarak biraraya getirilmez).

## <a name="automation"></a>Automation
* [Uyarıları ayarlamayı otomatikleştirmek için PowerShell'i kullanın](../../azure-monitor/app/powershell-alerts.md)
* [Uyarılara yanıt vermeyi otomatikleştirmek için webhooks'u kullanma](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
* [Uyarıları ayarlamayı otomatikleştirin](../../azure-monitor/app/powershell-alerts.md)
* [Öngörülü tanılama](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md

