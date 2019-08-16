---
title: Microsoft Dynamics CRM ve Azure Application Insights | Microsoft Docs
description: Application Insights kullanarak Microsoft Dynamics CRM Online 'dan telemetri alın. Kurulum, veri alma, görselleştirme ve dışarı aktarma hakkında yönergeler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 470f723782ca29409549e0df8e900edf86cd446e
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534286"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Çözüm: Application Insights kullanarak Microsoft Dynamics CRM Online için telemetri etkinleştirme
Bu makalede, [Azure Application Insights](https://azure.microsoft.com/services/application-insights/)kullanarak [Microsoft Dynamics CRM Online](https://www.dynamics.com/) 'dan telemetri verilerini nasıl alacağınız gösterilmektedir. Uygulamanıza Application Insights betiği ekleme, verileri yakalama ve veri görselleştirme işlemlerini adım adım inceleyeceğiz.

> [!NOTE]
> [Örnek çözüme gözatamazsınız](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Yeni veya mevcut CRM Online örneğine Application Insights ekleyin
Uygulamanızı izlemek için uygulamanıza bir Application Insights SDK 'Sı eklersiniz. SDK, güçlü analiz ve tanılama araçlarımızı kullanabileceğiniz ya da verileri depolama alanına dışarı aktarbileceğiniz [Application Insights portalına](https://portal.azure.com)telemetri gönderir.

### <a name="create-an-application-insights-resource-in-azure"></a>Azure 'da bir Application Insights kaynağı oluşturma
1. [Microsoft Azure bir hesap](https://azure.com/pricing)alın. 
2. [Azure Portal](https://portal.azure.com) oturum açın ve yeni bir Application Insights kaynağı ekleyin. Bu, verilerinizin işleneceği ve görüntüleneceği yerdir.

    ![+, Geliştirici Hizmetleri, Application Insights ' a tıklayın.](./media/sample-mscrm/01.png)

    Uygulama türü olarak ASP.NET’i seçin.
3. [Uygulamanızın JAVASCRIPT SDK betiğini almak](../../azure-monitor/app/javascript.md)için yönergeleri Izleyin, JavaScript kod parçacığını kopyalayın ve izleme anahtarını Application Insights kaynağınız için doğru değerle değiştirdiğinizden emin olun.

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Microsoft Dynamics CRM 'de bir JavaScript Web kaynağı oluşturma
1. CRM Online örneğinizi açın ve yönetici ayrıcalıklarıyla oturum açın.
2. Microsoft Dynamics CRM ayarları, özelleştirmeler ' i açın, sistemi özelleştirin

    ![Microsoft Dynamics CRM ayarları](./media/sample-mscrm/00001.png)

    ![Ayarları > özelleştirmeleri](./media/sample-mscrm/00002.png)

1. Bir JavaScript kaynağı oluşturun.

    ![Yeni Web kaynağı iletişim kutusu](./media/sample-mscrm/07.png)

    Bir ad verin, **betik (JScript)** seçeneğini belirleyin ve metin düzenleyicisini açın.

    ![Metin düzenleyicisini açın](./media/sample-mscrm/00004.png)
2. Izleme anahtarınızı daha önce yapılandırdığınız Application Insights JavaScript SDK ' dan kodu kopyalayın. Kopyalama sırasında, komut dosyası etiketlerini göz ardı ettiğinizden emin olun. Aşağıdaki ekran görüntüsüne bakın:

    ![İzleme anahtarınızı ayarlama](./media/sample-mscrm/000005.png)

    Kod, Application Insights kaynağınızı tanımlayan izleme anahtarını içerir.
3. Kaydet ve Yayımla.

    ![Kaydet ve yayımla](./media/sample-mscrm/00006.png)

### <a name="instrument-forms"></a>Araç formları
1. Microsoft CRM Online 'da, hesap formunu açın

    ![Hesap formu](./media/sample-mscrm/00007.png)
2. Form özelliklerini aç

    ![Form özellikleri](./media/sample-mscrm/00008.png)
3. Oluşturduğunuz JavaScript Web kaynağını ekleyin

    ![Menü ekle](./media/sample-mscrm/13.png)

4. Form özelleştirmelerinizi kaydedin ve yayımlayın.

## <a name="metrics-captured"></a>Yakalanan ölçümler
Form için telemetri yakalamayı ayarlamış oldunuz. Her kullanıldığında, veriler Application Insights kaynağına gönderilir.

Göreceğiniz verilerin örnekleri aşağıda verilmiştir.

#### <a name="application-health"></a>Uygulama sistem durumu
![Örnek sayfa yükleme süresi](./media/sample-mscrm/15.png)

![Örnek sayfa görünümleri grafiği](./media/sample-mscrm/16.png)

Tarayıcı özel durumları:

![Tarayıcı özel durumlar grafiği](./media/sample-mscrm/17.png)

Daha ayrıntılı bilgi almak için grafiğe tıklayın:

![Özel durum listesi](./media/sample-mscrm/18.png)

#### <a name="usage"></a>Kullanım
![Kullanıcılar, oturumlar ve sayfa görünümleri](./media/sample-mscrm/19.png)

![Oturum grafikleri](./media/sample-mscrm/20.png)

![Tarayıcı sürümleri](./media/sample-mscrm/21.png)

#### <a name="browsers"></a>Tarayıcılar
![Sayfa yükleme zamanının dökümünü](./media/sample-mscrm/22.png)

![Tarayıcı sürümüne göre oturum sayısı](./media/sample-mscrm/23.png)

#### <a name="geolocation"></a>Coğrafi Konum
![Ülkeye göre oturum sayısı](./media/sample-mscrm/24.png)

![Ülkeye göre oturumlar ve kullanıcılar](./media/sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Sayfa görüntüleme isteği içinde
![Sayfa görünümü Özeti](./media/sample-mscrm/26.png)

![Sayfa görüntüleme olaylarında ara](./media/sample-mscrm/27.png)

![Benzer sayfa görüntülemeler](./media/sample-mscrm/28.png)

![Sayfa görünümü özellikleri](./media/sample-mscrm/29.png)

![Oturum başına sayfa sayısı](./media/sample-mscrm/30.png)

## <a name="sample-code"></a>Örnek kod
[Örnek koda gözatamazsınız](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
[Verileri Microsoft Power BI 'e aktardığınızda](../../azure-monitor/app/export-power-bi.md )daha derin bir analiz yapabilirsiniz.

## <a name="sample-microsoft-dynamics-crm-solution"></a>Örnek Microsoft Dynamics CRM çözümü
[Microsoft DYNAMICS CRM 'de uygulanan örnek çözüm aşağıda verilmiştir](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Application Insights nedir?](../../azure-monitor/app/app-insights-overview.md)
* [Web sayfaları için Application Insights](../../azure-monitor/app/javascript.md)
* [Daha fazla örnek ve İzlenecek yol](../../azure-monitor/app/app-insights-overview.md)
