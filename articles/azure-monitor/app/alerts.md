---
title: Azure Application Insights uyarıları ayarlama
description: Web uygulamanızda yavaş yanıt süreleri, özel durumlar ve diğer performans veya kullanım değişiklikleri hakkında bildirim alın.
ms.topic: conceptual
ms.date: 01/23/2019
ms.reviewer: lagayhar
ms.subservice: alerts
ms.openlocfilehash: 28fd59556a586b85a6d3caf188d9e02c11d31e3b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80295089"
---
# <a name="set-alerts-in-application-insights"></a>Application Insights uyarıları ayarlama

[Azure Application Insights][start] , Web uygulamanızdaki performans veya kullanım ölçümlerinde değişiklikler yapmak için sizi uyarır. 

Application Insights, performans sorunlarını tanılamanıza ve kullanım düzenlerini anlamanıza yardımcı olmak üzere [çok çeşitli platformlarda][platforms] canlı uygulamanızı izler.

Birden çok uyarı türü vardır:

* [**Ölçüm uyarıları**](../../azure-monitor/platform/alerts-metric-overview.md) , bir ölçüm bir dönem için yanıt süreleri, özel durum sayısı, CPU kullanımı veya sayfa görünümleri gibi bir eşik değeri kesiştiği zaman size bildirir.
* [**Günlük uyarıları**](../../azure-monitor/platform/alerts-unified-log.md) , uyarı sinyalinin özel bir kusto sorgusuna dayalı olduğu uyarıları anlatmak için kullanılır.
* [**Web testleri**][availability] siteniz Internet 'te kullanılamadığında veya yavaş yanıt vermeyi bildirir. [Daha fazla bilgi edinin][availability].
* [**Proaktif Tanılamalar**](../../azure-monitor/app/proactive-diagnostics.md) , olağan dışı performans desenleri hakkında sizi bilgilendirmek üzere otomatik olarak yapılandırılır.

## <a name="how-to-set-an-exception-alert-using-custom-log-search"></a>Özel günlük aramasını kullanarak özel durum uyarısı ayarlama

Bu bölümde, sorgu tabanlı özel durum uyarısını nasıl ayarlayacağız. Bu örnekte, son 24 saat içindeki başarısız oran %10 ' dan büyük olduğunda bir uyarı istiyoruz.

1. Azure portal uygulama Insight kaynağına gidin.
2. Sol tarafta, Yapılandır ' ın altındaki **Uyarı**' ı tıklatın.

    ![Sol tarafta, Yapılandır tıklama uyarısı](./media/alerts/1appinsightalert.png)

3. Uyarı sekmesinin en üstünde **Yeni uyarı kuralı**' nı seçin.

     ![Uyarı sekmesinin en üstünde yeni uyarı kuralı ' na tıklayın.](./media/alerts/2createalert.png)

4. Kaynağınızın otomatik olarak seçilmesi gerekir. Bir koşul ayarlamak için **Koşul Ekle**' ye tıklayın.

    ![Koşul Ekle ' ye tıklayın](./media/alerts/3addcondition.png)

5. Sinyal mantığını Yapılandır sekmesinde **özel günlük araması** ' nı seçin.

    ![Özel günlük araması ' na tıklayın](./media/alerts/4customlogsearch.png)

6. Özel günlük araması sekmesinde sorgunuzu "arama sorgusu" kutusuna girin. Bu örnekte, aşağıdaki kusto sorgusunu kullanacağız.
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
    > Ayrıca, bu adımları sorgu tabanlı diğer uyarı türleri için de uygulayabilirsiniz. Kusto sorgu dili hakkında daha fazla bilgi edinmek için bu [kusto kullanmaya başlama belgesi](https://docs.microsoft.com/azure/kusto/concepts/) veya bu [SQL to kusto chyiyecek sayfasını](https://docs.microsoft.com/azure/kusto/query/sqlcheatsheet) kullanabilirsiniz

7. "Uyarı mantığı" altında, bunun sonuç sayısına veya ölçüm ölçüsüne göre yapılıp yapılmayacağını seçin. Sonra koşulu (büyüktür, eşittir, küçüktür) ve bir eşiği seçin. Bu değerleri değiştirirken, koşul önizleme cümlesi değişikliklerinin olduğunu fark edebilirsiniz. Bu örnekte, "eşittir" olarak kullanıyoruz.

    ![Uyarı mantığı altında, ve koşulunu temel alarak belirtilen seçeneklerden seçim yapın ve ardından bir eşik yazın](./media/alerts/6alertlogic.png)

8. "Değerlendirilen tabanlı" altında, dönemi ve sıklığı ayarlayın. Buradaki süre, Yukarıdaki sorguda dönem için koyduğumuz değerle eşleşmelidir. Ardından **bitti**' ye tıklayın.

    ![En alttaki dönemi ve sıklığı ayarlayıp bitti ' ye tıklayın.](./media/alerts/7evaluate.png)

9. Şimdi, tahmini aylık maliyet ile oluşturduğumuz koşulu görüyoruz. Aşağıda ["eylem grupları"](../platform/action-groups.md) altında, yeni bir grup oluşturabilir veya var olan bir grubu seçebilirsiniz. İsterseniz, eylemleri özelleştirebilirsiniz.

    ![eylem grubu altında Seç veya Oluştur düğmesine tıklayın](./media/alerts/8actiongroup.png)

10. Son olarak uyarı ayrıntılarınızı (uyarı kuralı adı, açıklama, önem derecesi) ekleyin. İşiniz bittiğinde, en altta **Uyarı kuralı oluştur** ' a tıklayın.

    ![Uyarı ayrıntısı altında uyarı kuralı adınızı yazın, bir açıklama yazın ve önem derecesi seçin](./media/alerts/9alertdetails.png)

## <a name="how-to-unsubscribe-from-classic-alert-e-mail-notifications"></a>Klasik uyarı e-posta bildirimlerinin aboneliğini kaldırma

Bu bölüm, klasik **kullanılabilirlik uyarıları**, **Klasik Application Insights ölçüm uyarıları**ve **Klasik hata bozukluklar uyarıları**için geçerlidir.

Aşağıdakilerden biri geçerliyse, bu klasik uyarılar için e-posta bildirimleri alıyorsunuz:

* E-posta adresiniz, uyarı kuralı ayarlarındaki bildirim e-posta alıcıları alanında listelenir.

* Abonelik için belirli rolleri tutan kullanıcılara e-posta bildirimleri gönderme seçeneği etkinleştirilir ve söz konusu Azure aboneliği için ilgili bir rol tutabilirsiniz.

![Uyarı bildirimi ekran görüntüsü](./media/alerts/alert-notification.png)

Güvenlik ve gizliliğinizi daha iyi denetlemek için, **bildirim e-posta alıcıları** alanında klasik uyarılarınız için bildirim alıcılarını açıkça belirtmenizi öneririz. Geri uyumluluk için belirli rolleri tutan tüm kullanıcılara bildirme seçeneği sağlanır.

Belirli bir uyarı kuralı tarafından oluşturulan e-posta bildirimlerinin aboneliğini kaldırmak için, **bildirim e-posta alıcıları** alanından e-posta adresinizi kaldırın.

E-posta adresiniz açıkça listelenmiyorsa, belirli rollerin tüm üyelerini otomatik olarak bildirme seçeneğini devre dışı bırakmanız ve bunun yerine bildirim e-posta alıcıları alanındaki ilgili uyarı kuralına yönelik bildirimleri alması gereken tüm kullanıcı e-postalarını listeetmenizi öneririz.

## <a name="who-receives-the-classic-alert-notifications"></a>(Klasik) uyarı bildirimlerini kim alıyor?

Bu bölüm yalnızca, klasik uyarılar için geçerlidir ve yalnızca istediğiniz alıcıların bildirimleri almasını sağlamak için uyarı bildirimlerinizi iyileştirmenize yardımcı olur. [Klasik uyarılar](../platform/alerts-classic.overview.md) ve yeni uyarılar deneyimi arasındaki fark hakkında daha fazla bilgi edinmek için [uyarılara genel bakış makalesine](../platform/alerts-overview.md)bakın. Yeni uyarılar deneyiminde uyarı bildirimini denetlemek için [eylem grupları](../platform/action-groups.md)' nı kullanın.

* Klasik uyarı bildirimleri için belirli alıcıların kullanılmasını öneririz.

* Tüm Application Insights ölçümlerinde (kullanılabilirlik ölçümleri dahil) uyarılar için, **toplu/grup** onay kutusu seçeneği etkinse, abonelikte sahip, katkıda bulunan veya okuyucu rolleriyle kullanıcılara gönderir. Aslında, aboneliğe erişimi olan _Tüm_ kullanıcılar Application Insights kaynak kapsamdadır ve bildirimler alacaktır.

> [!NOTE]
> Şu anda **toplu/grup** onay kutusu seçeneğini kullanırsanız ve devre dışı bıraktığınızda, değişikliği döndüremezsiniz.

Kullanıcılara rollerine göre bildirimde bulunan yeni uyarı deneyimini veya neredeyse gerçek zamanlı uyarıları kullanın. [Eylem gruplarıyla](../platform/action-groups.md), katkıda bulunan/sahip/okuyucu rollerinin herhangi birine sahip kullanıcılar için e-posta bildirimleri yapılandırabilirsiniz (tek bir seçenek olarak birlikte birleştirilemez).

## <a name="automation"></a>Otomasyon
* [Uyarıları ayarlamayı otomatikleştirmek için PowerShell 'i kullanma](../../azure-monitor/app/powershell-alerts.md)
* [Uyarıları yanıtlamayı otomatikleştirmek için Web kancalarını kullanma](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
* [Uyarıları ayarlamayı otomatikleştirme](../../azure-monitor/app/powershell-alerts.md)
* [Öngörülü tanılama](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md

