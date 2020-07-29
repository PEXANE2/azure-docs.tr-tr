---
title: Hızlı başlangıç ASP.NET Core-Azure Izleyici Application Insights
description: Azure Izleyici ile izleme için ASP.NET Core bir Web uygulamasını hızlıca ayarlamaya yönelik yönergeler sağlar Application Insights
ms.subservice: application-insights
ms.topic: quickstart
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/26/2019
ms.custom: mvc
ms.openlocfilehash: c9787a88cba051dcfaa58a188bf43fb03cafe70a
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87325177"
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>ASP.NET Core Web Uygulamanızı İzlemeye Başlama

Azure Application Insights ile web uygulamanızı kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. 

Bu hızlı başlangıç, var olan bir ASP.NET Core Web uygulamasına Application Insights SDK ekleme konusunda size rehberlik eder. Visual Studio 'Yu Application Insights yapılandırma hakkında bilgi edinmek için bu [makaleye](../app/asp-net-core.md)göz atın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

- Aşağıdaki iş yükleriyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) ' i yükledikten sonra:
  - ASP.NET ve web geliştirme
  - Azure geliştirme
- [.NET Core 2.0 SDK yükleme](https://dotnet.microsoft.com/download)
- Bir Azure Aboneliği ve var olan bir .NET Core web uygulaması gerekir.

ASP.NET Core Web uygulamanız yoksa, [ASP.NET Core bir uygulama oluşturmak ve Application Insights eklemek](../app/asp-net-core.md) için adım adım kılavuzumuzu kullanabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışmasından bağımsız olarak İnternet’e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. **Kaynak oluştur**  >  **Geliştirici Araçları**  >  **Application Insights**seçin.

   > [!NOTE]
   >İlk kez bir Application Insights kaynağı oluşturuyorsanız [Application Insights kaynak oluştur belge oluştur](../app/create-new-resource.md) ' a giderek daha fazla bilgi edinebilirsiniz.

    Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

   | Ayarlar        |  Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıracak yeni kaynak grubunun adı. Yeni bir kaynak grubu oluşturabilir veya var olan bir grup kullanabilirsiniz. |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.



## <a name="configure-app-insights-sdk"></a>App Insights SDK’sını Yapılandırma

1. ASP.NET Core Web Uygulaması **projenizi** Visual Studio’da açın > **Çözüm Gezgini**’nde AppName öğesne sağ tıklayın > **Ekle** > **Application Insights Telemetrisi**’ni seçin.

    ![Application Insights Telemetrisi ekleme](./media/dotnetcore-quick-start/2vsaddappinsights.png)

2. **Başlarken** düğmesine tıklayın

3. Hesabınızı ve aboneliğinizi seçin > Azure portal oluşturduğunuz **mevcut kaynağı** seçin > **Kaydet**' e tıklayın.

4. **Proje**seçin  >  **NuGet paketleri**  >  **paket kaynağını seçin: NuGet.org**  >  Application Insights SDK paketlerini en son kararlı sürüme**güncelleştirin** .

5. Uygulamanızı başlatmak için **hata ayıklama**  >  **olmadan Başlat** ' ı seçin (CTRL + F5)

    ![Application Insights’a Genel Bakış Menüsü](./media/dotnetcore-quick-start/3debug.png)

> [!NOTE]
> Verilerin portalda görünmesi 3-5 dakika sürer. Bu uygulama düşük trafikli bir test uygulaması ise, çoğu ölçümün yalnızca etkin istek veya işlem olduğunda yakalandığını aklınızda bulundurun.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatın

1. **Ana** sayfa ' yı seçerek Azure Portal Application Insights **genel bakış** sayfasını yeniden açın ve son zamanlarda, çalışmakta olan uygulamanız hakkındaki ayrıntıları görüntülemek için daha önce oluşturduğunuz kaynağı seçin.

   ![Application Insights’a Genel Bakış Menüsü](./media/dotnetcore-quick-start/4overview.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzeni için **Uygulama haritası**’na tıklayın. Her bileşen yük, performans, hatalar ve uyarılar gibi KPI'leri gösterir.

   ![Uygulama Eşlemesi](./media/dotnetcore-quick-start/5appmap.png)

3. Analytics 'teki uygulama **Analizi** simgesi ![ uygulama Haritası simge ](./media/dotnetcore-quick-start/006.png) **görünümü**' ne tıklayın. Bu işlem, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlayan **Application Insights Analizi**’ni açar. Bu örnekte, istek sayısını grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

   ![Belirli bir süre içindeki kullanıcı isteklerinin analiz grafiği](./media/dotnetcore-quick-start/6analytics.png)

4. **Genel bakış** sayfasına dönün ve KPI panolarını inceleyin.  Bu pano, gelen istek sayısı, bu isteklerin süresi ve oluşan hatalar dahil olmak üzere uygulamanızın sistem durumu hakkında istatistikler sağlar. 

   ![Sistem Durumuna Genel Bakış zaman çizelgesi grafikleri](./media/dotnetcore-quick-start/7kpidashboards.png)

5. Sağ tarafta **ölçümler**' e tıklayın. Kaynağınızın sistem durumunu ve kullanımını araştırmak için ölçüm gezginini kullanın. **Yeni grafik ekle**’ye tıklayarak ek özel görünümler oluşturabilir veya **Düzenle**’yi seçerek mevcut grafik türlerini, yüksekliğini, renk paletini, gruplandırmaları ve ölçümleri değiştirebilirsiniz. Örneğin, ölçümler açılan penceresinden "tarayıcı sayfa yükleme süresi" ni ve toplamasının "Ort" öğesini seçerek ortalama tarayıcı sayfa yükleme süresini görüntüleyen bir grafik yapabilirsiniz. Azure Ölçüm Gezgini hakkında daha fazla bilgi edinmek için [azure Ölçüm Gezgini](../platform/metrics-getting-started.md)kullanmaya başlama makalesini ziyaret edin.

     ![Ölçümler sekmesi: ortalama tarayıcı sayfa yükleme süresi grafiği](./media/dotnetcore-quick-start/8metrics.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Testi tamamladığınızda, kaynak grubunu ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin.

> [!NOTE]
> Var olan bir kaynak grubunu kullandıysanız aşağıdaki yönergeler çalışmaz ve yalnızca tek bir Application Insights kaynağını silmeniz gerekir. Bir kaynak grubunu her sildiğinizde, bu grubun üyesi olan tüm kaynaklar silinir.

1. Azure portal sol taraftaki menüden **kaynak grupları** ' na ve ardından **myresourcegroup**' a tıklayın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çalışma zamanı özel durumlarını bulma ve tanılama](./tutorial-runtime-exceptions.md)

