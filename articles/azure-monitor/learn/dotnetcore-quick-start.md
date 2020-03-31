---
title: CoreASP.NET hızlı başlat - Azure Monitör Uygulama Öngörüleri
description: Azure Monitöruygulama Öngörüleri ile izleme için hızlı bir ASP.NET Core Web Uygulaması oluşturmak için talimatlar sağlar
ms.subservice: application-insights
ms.topic: quickstart
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/26/2019
ms.custom: mvc
ms.openlocfilehash: ef46b86186d1f5e26360de891b3a090ab0ece66b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78894825"
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>ASP.NET Core Web Uygulamanızı İzlemeye Başlama

Azure Application Insights ile web uygulamanızı kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. 

Bu hızlı başlangıç, Application Insights SDK'yı mevcut bir ASP.NET Core web uygulamasına ekleyerek size yol göstersin. Visual Studio ödeme olmadan Uygulama Öngörüleri yapılandırma hakkında bilgi edinmek için bu [makalede](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

- [Visual Studio 2019'u](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) aşağıdaki iş yükleriyle yükleyin:
  - ASP.NET ve web geliştirme
  - Azure geliştirme
- [.NET Core 2.0 SDK yükleme](https://dotnet.microsoft.com/download)
- Bir Azure Aboneliği ve var olan bir .NET Core web uygulaması gerekir.

ASP.NET Core web uygulamanız yoksa, [ASP.NET Bir Çekirdek uygulaması oluşturmak ve Uygulama Öngörüleri eklemek](../../azure-monitor/app/asp-net-core.md) için adım adım kılavuzumuzu kullanabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışmasından bağımsız olarak İnternet’e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. **Kaynak** > **Oluştur Geliştirici araçları** > **Uygulama Öngörüleri'ni**seçin.

   > [!NOTE]
   >Uygulama Öngörüleri kaynağını ilk kez oluşturuyorsanız, [Uygulama Öngörüleri Oluştur Kaynak](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) dokümanını ziyaret ederek daha fazla bilgi edinebilirsiniz.

    Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

   | Ayarlar        |  Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıracak yeni kaynak grubunun adı. Yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu kullanabilirsiniz. |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur'u**tıklatın.



## <a name="configure-app-insights-sdk"></a>App Insights SDK’sını Yapılandırma

1. ASP.NET Core Web Uygulaması **projenizi** Visual Studio’da açın > **Çözüm Gezgini**’nde AppName öğesne sağ tıklayın > **Ekle** > **Application Insights Telemetrisi**’ni seçin.

    ![Application Insights Telemetrisi ekleme](./media/dotnetcore-quick-start/2vsaddappinsights.png)

2. Başlat **düğmesini** tıklatın

3. Hesabınızı ve aboneliğinizi seçin > Azure portalında oluşturduğunuz **Varolan kaynağı** seçin > **Kaydol'u**tıklatın.

4. **Proje** > **Yönet NuGet Paketleri** > **Paket kaynağını seçin: nuget.org** > Uygulama Öngörüleri SDK paketlerini en son kararlı sürümle**güncelleyin.**

5. Uygulamanızı başlatmak için Hata Ayıklama (Ctrl+F5) olmadan **Hata Ayıklama** > **Başlat'ı** seçin

    ![Application Insights’a Genel Bakış Menüsü](./media/dotnetcore-quick-start/3debug.png)

> [!NOTE]
> Verilerin portalda görünmesi 3-5 dakika sürer. Bu uygulama düşük trafikli bir test uygulaması ise, çoğu ölçümün yalnızca etkin istek veya işlem olduğunda yakalandığını aklınızda bulundurun.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatın

1. Azure portalındaki Uygulama Öngörüleri **Genel Bakış** sayfasını **Ana Sayfa'yı** seçerek yeniden açın ve en yeni kaynakların altında, şu anda çalışan uygulamanızla ilgili ayrıntıları görüntülemek için daha önce oluşturduğunuz kaynağı seçin.

   ![Application Insights’a Genel Bakış Menüsü](./media/dotnetcore-quick-start/4overview.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzeni için **Uygulama haritası**’na tıklayın. Her bileşen yük, performans, hatalar ve uyarılar gibi KPI'leri gösterir.

   ![Uygulama Eşlemesi](./media/dotnetcore-quick-start/5appmap.png)

3. ](./media/dotnetcore-quick-start/006.png) **Analytics'te** **Uygulama** ![Analizi simgesi Uygulama Haritası simgesine tıklayın. Bu işlem, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlayan **Application Insights Analizi**’ni açar. Bu örnekte, istek sayısını grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

   ![Belirli bir süre içindeki kullanıcı isteklerinin analiz grafiği](./media/dotnetcore-quick-start/6analytics.png)

4. **Genel Bakış** sayfasına dönün ve KPI Panolarını inceleyin.  Bu pano, gelen istek sayısı, bu isteklerin süresi ve oluşan hatalar dahil olmak üzere uygulamanızın sistem durumu hakkında istatistikler sağlar. 

   ![Sistem Durumuna Genel Bakış zaman çizelgesi grafikleri](./media/dotnetcore-quick-start/7kpidashboards.png)

5. **Ölçümler'de**sol a tıklama. Kaynağınızın durumunu ve kullanımını araştırmak için ölçüler gezginini kullanın. **Yeni grafik ekle**’ye tıklayarak ek özel görünümler oluşturabilir veya **Düzenle**’yi seçerek mevcut grafik türlerini, yüksekliğini, renk paletini, gruplandırmaları ve ölçümleri değiştirebilirsiniz. Örneğin, ölçümler inmeden "Tarayıcı sayfası yükleme süresi" ve toplamadan "Avg" seçerek ortalama tarayıcı sayfası yükleme süresini görüntüleyen bir grafik yapabilirsiniz. Azure Ölçümleri Explorer hakkında daha fazla bilgi edinmek için [Azure Ölçümleri Gezgini'ne başlama](../../azure-monitor/platform/metrics-getting-started.md)yı ziyaret edin.

     ![Ölçümler sekmesi: Ortalama tarayıcı sayfası yükleme süresi grafiği](./media/dotnetcore-quick-start/8metrics.png)

## <a name="video"></a>Video

- [Uygulama Öngörülerini .NET Core ve Visual Studio ile](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) sıfırdan yapılandırma hakkında harici adım adım video.
- [Uygulama Öngörülerini .NET Core ve Visual Studio Code ile](https://youtu.be/ygGt84GDync) sıfırdan yapılandırma hakkında harici adım adım video.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Sınama bittiğinde, kaynak grubunu ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin.

> [!NOTE]
> Varolan bir kaynak grubu kullandıysanız, aşağıdaki talimatlar çalışmaz ve tek tek Application Insights kaynağını silmeniz gerekir. Bir kaynak grubunu her sildiğinizde, o grubun üyesi olan tüm alt kaynaklarının silineceğini unutmayın.

1. Azure portalındaki sol menüden Kaynak **gruplarını** tıklatın ve ardından **myResourceGroup'u**tıklatın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çalışma zamanı özel durumlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-runtime-exceptions)
