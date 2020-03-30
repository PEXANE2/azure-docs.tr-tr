---
title: Webİşler ile arka plan görevlerini çalıştırma
description: Azure Uygulama Hizmeti'nde arka plan görevlerini çalıştırmak için Webİşler'i nasıl kullanacağınızı öğrenin. Çeşitli komut dosyası biçimleri arasından seçim yapın ve bunları CRON ifadeleriyle çalıştırın.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ms.reviewer: msangapu;david.ebbo;suwatch;pbatum;naren.soni
ms.custom: seodec18
ms.openlocfilehash: 4c568c95a5dbc1799a765c95a2b224de53dfbe9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279149"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Azure Uygulama Hizmetinde Web İşleri ile arka plan görevlerini çalıştırma

Bu makalede, yürütülebilir veya komut dosyası yüklemek için [Azure portalını](https://portal.azure.com) kullanarak Web İşlerin nasıl dağıtılancayacağın gösterilmektedir. Visual Studio'u kullanarak Web İşlerin nasıl geliştirilip dağıtılabildiğini öğrenmek için [bkz.](webjobs-dotnet-deploy-vs.md)

## <a name="overview"></a>Genel Bakış
WebJobs, Azure [Uygulama Hizmeti'nin](index.yml) bir programı veya komut dosyasını bir web uygulaması, API uygulaması veya mobil uygulamayla aynı bağlamda çalıştırmanızı sağlayan bir özelliğidir. Webİşler'i kullanmak için ek bir ücret yoktur.

> [!IMPORTANT]
> WebJobs henüz Linux'taki Uygulama Hizmeti için desteklenmedi.

Azure Web İşleri SDK'sı, birçok programlama görevini basitleştirmek için Webİşler ile birlikte kullanılabilir. Daha fazla bilgi için [WebJobs SDK nedir'](https://github.com/Azure/azure-webjobs-sdk/wiki)e bakın.

Azure İşlevleri, programları ve komut dosyalarını çalıştırmak için başka bir yol sağlar. Web İşler ve İşlevler arasında karşılaştırma yapmak için [akış, Mantık Uygulamaları, İşlevler ve Web İşleri arasında seçim](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md)yapma'ya bakın.

## <a name="webjob-types"></a>WebJob türleri

Aşağıdaki *tabloda sürekli* ve *tetiklenen* Web İşler arasındaki farklar açıklanmaktadır.


|Sürekli  |Tetik -lenen  |
|---------|---------|
| WebJob oluşturulduğunda hemen başlar. İşin sona ermemesi için, program veya komut dosyası genellikle işini sonsuz bir döngü içinde yapar. İş biterse, yeniden başlatabilirsiniz. | Yalnızca el ile veya zamanlamada tetiklendiğinde başlar. |
| Web uygulamasının üzerinde çalıştığı tüm örneklerde çalışır. WebJob'u isteğe bağlı olarak tek bir örnekle kısıtlayabilirsiniz. |Azure'un yük dengeleme için seçtiği tek bir örnekte çalışır.|
| Uzaktan hata ayıklamayı destekler. | Uzaktan hata ayıklamayı desteklemiyor.|

[!INCLUDE [webjobs-always-on-note](../../includes/webjobs-always-on-note.md)]

## <a name="supported-file-types-for-scripts-or-programs"></a><a name="acceptablefiles"></a>Komut dosyaları veya programlar için desteklenen dosya türleri

Aşağıdaki dosya türleri desteklenir:

* .cmd, .bat, .exe (Windows cmd kullanarak)
* .ps1 (PowerShell kullanarak)
* .sh (Bash kullanarak)
* .php (PHP kullanarak)
* .py (Python kullanarak)
* .js (Düğüm.js kullanarak)
* .jar (Java kullanarak)

## <a name="create-a-continuous-webjob"></a><a name="CreateContinuous"></a>Sürekli bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. Azure [portalında,](https://portal.azure.com)Uygulama Hizmeti web uygulamanızın, API uygulamanızın veya mobil uygulamanızın **Uygulama Hizmeti** sayfasına gidin.

2. **Webİşler'i**seçin.

   ![Web İşlerini Seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. **Webİşler** sayfasında **Ekle'yi**seçin.

    ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Tabloda belirtildiği gibi **Webİş Ekle** ayarlarını kullanın.

   ![WebJob sayfası ekle](./media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Adı** | myContinuousWebJob | Bir App Service uygulamasında benzersiz olan bir ad. Bir harf veya sayı ile başlamalıdır ve "-" ve "_" dışındaki özel karakterler içeremez. |
   | **Dosya Yükleme** | ConsoleApp.zip | Yürütülebilir veya komut dosyası dosyanızın yanı sıra programı veya komut dosyasını çalıştırmak için gereken destekleyici dosyaları içeren bir *.zip* dosyası. Desteklenen yürütülebilir veya komut dosyası dosya türleri [Desteklenen dosya türleri](#acceptablefiles) bölümünde listelenir. |
   | **Tür** | Sürekli | [WebJob türleri](#webjob-types) bu makalede daha önce açıklanmıştır. |
   | **Ölçeklendirme** | Çok örnek | Yalnızca Sürekli Web İşleri için kullanılabilir. Programın veya komut dosyasının tüm örneklerde mi yoksa tek bir örnekte mi çalıştığını belirler. Birden çok örnekte çalışma seçeneği, Ücretsiz veya Paylaşılan [fiyatlandırma katmanları](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)için geçerli değildir. | 

4. **Tamam**'a tıklayın.

   Yeni Webİş, Web **İşler** sayfasında görünür.

   ![Webİşler listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

2. Sürekli bir WebJob'u durdurmak veya yeniden başlatmak için listedeki WebJob'u sağ tıklatın ve **Durdur** veya **Başlat'ı**tıklatın.

    ![Sürekli bir WebJob'u durdurun](./media/web-sites-create-web-jobs/continuousstop.png)

## <a name="create-a-manually-triggered-webjob"></a><a name="CreateOnDemand"></a>El ile tetiklenen bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. Azure [portalında,](https://portal.azure.com)Uygulama Hizmeti web uygulamanızın, API uygulamanızın veya mobil uygulamanızın **Uygulama Hizmeti** sayfasına gidin.

2. **Webİşler'i**seçin.

   ![Web İşlerini Seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. **Webİşler** sayfasında **Ekle'yi**seçin.

    ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Tabloda belirtildiği gibi **Webİş Ekle** ayarlarını kullanın.

   ![WebJob sayfası ekle](./media/web-sites-create-web-jobs/addwjtriggered.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Adı** | myTriggeredWebJob | Bir App Service uygulamasında benzersiz olan bir ad. Bir harf veya sayı ile başlamalıdır ve "-" ve "_" dışındaki özel karakterler içeremez.|
   | **Dosya Yükleme** | ConsoleApp.zip | Yürütülebilir veya komut dosyası dosyanızın yanı sıra programı veya komut dosyasını çalıştırmak için gereken destekleyici dosyaları içeren bir *.zip* dosyası. Desteklenen yürütülebilir veya komut dosyası dosya türleri [Desteklenen dosya türleri](#acceptablefiles) bölümünde listelenir. |
   | **Tür** | Tetik -lenen | [WebJob türleri](#webjob-types) bu makalede daha önce açıklanmıştır. |
   | **Tetikleyiciler** | El ile | |

4. **Tamam**'a tıklayın.

   Yeni Webİş, Web **İşler** sayfasında görünür.

   ![Webİşler listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

7. WebJob'u çalıştırmak için, listede adını sağ tıklatın ve **Çalıştır'ı**tıklatın.
   
    ![WebJob çalıştırın](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="create-a-scheduled-webjob"></a><a name="CreateScheduledCRON"></a>Zamanlanmış bir WebJob oluşturma

<!-- 
Several steps in the three "Create..." sections are identical; 
when making changes in one don't forget the other two.
-->

1. Azure [portalında,](https://portal.azure.com)Uygulama Hizmeti web uygulamanızın, API uygulamanızın veya mobil uygulamanızın **Uygulama Hizmeti** sayfasına gidin.

2. **Webİşler'i**seçin.

   ![Web İşlerini Seçin](./media/web-sites-create-web-jobs/select-webjobs.png)

2. **Webİşler** sayfasında **Ekle'yi**seçin.

   ![WebJob sayfası](./media/web-sites-create-web-jobs/wjblade.png)

3. Tabloda belirtildiği gibi **Webİş Ekle** ayarlarını kullanın.

   ![WebJob sayfası ekle](./media/web-sites-create-web-jobs/addwjscheduled.png)

   | Ayar      | Örnek değer   | Açıklama  |
   | ------------ | ----------------- | ------------ |
   | **Adı** | myScheduledWebJob | Bir App Service uygulamasında benzersiz olan bir ad. Bir harf veya sayı ile başlamalıdır ve "-" ve "_" dışındaki özel karakterler içeremez. |
   | **Dosya Yükleme** | ConsoleApp.zip | Yürütülebilir veya komut dosyası dosyanızın yanı sıra programı veya komut dosyasını çalıştırmak için gereken destekleyici dosyaları içeren bir *.zip* dosyası. Desteklenen yürütülebilir veya komut dosyası dosya türleri [Desteklenen dosya türleri](#acceptablefiles) bölümünde listelenir. |
   | **Tür** | Tetik -lenen | [WebJob türleri](#webjob-types) bu makalede daha önce açıklanmıştır. |
   | **Tetikleyiciler** | Zamanlanan | Zamanlamanın güvenilir çalışması için Her Zaman Açık özelliğini etkinleştirin. Always On yalnızca Temel, Standart ve Premium fiyatlandırma katmanlarında kullanılabilir.|
   | **CRON İfadesi** | 0 0/20 * * * * | [CRON ifadeleri](#ncrontab-expressions) aşağıdaki bölümde açıklanmıştır. |

4. **Tamam**'a tıklayın.

   Yeni Webİş, Web **İşler** sayfasında görünür.

   ![Webİşler listesi](./media/web-sites-create-web-jobs/listallwebjobs.png)

## <a name="ncrontab-expressions"></a>NCRONTAB ifadeleri

Portala bir [NCRONTAB ifadesi](../azure-functions/functions-bindings-timer.md#ncrontab-expressions) girebilir veya `settings.job` aşağıdaki örnekte olduğu gibi WebJob *.zip* dosyanızın köküne bir dosya ekleyebilirsiniz:

```json
{
    "schedule": "0 */15 * * * *"
}
```

Daha fazla bilgi edinmek [için, tetiklenen bir WebJob zamanlama'ya](webjobs-dotnet-deploy-vs.md#scheduling-a-triggered-webjob)bakın.

[!INCLUDE [webjobs-cron-timezone-note](../../includes/webjobs-cron-timezone-note.md)]

## <a name="view-the-job-history"></a><a name="ViewJobHistory"></a>İş geçmişini görüntüleme

1. Geçmişi görmek istediğiniz WebJob'u seçin ve ardından **Günlükler** düğmesini seçin.
   
   ![Günlükler düğmesi](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. **WebJob Ayrıntıları** sayfasında, tek bir çalışma için ayrıntıları görmek için bir zaman seçin.
   
   ![WebJob Detayları](./media/web-sites-create-web-jobs/webjobdetails.png)

3. **WebJob Run Ayrıntıları** sayfasında, günlük içeriğinin metnini görmek için **Çıkış'ı Toggle'ı** seçin.
   
    ![Web iş çalıştırma ayrıntıları](./media/web-sites-create-web-jobs/webjobrundetails.png)

   Çıktı metnini ayrı bir tarayıcı penceresinde görmek için **karşıdan yükleme'yi**seçin. Metnin kendisini indirmek için **indir'e** sağ tıklayın ve dosya içeriğini kaydetmek için tarayıcı seçeneklerinizi kullanın.
   
5. Web İşler listesine gitmek için sayfanın üst kısmındaki **Webİşler** kırıntısı bağlantısını seçin.

    ![WebJob ekmek kırıntısı](./media/web-sites-create-web-jobs/breadcrumb.png)
   
    ![Geçmiş panosundaki Web İşler listesi](./media/web-sites-create-web-jobs/webjobslist.png)
   
## <a name="next-steps"></a><a name="NextSteps"></a>Sonraki adımlar

Azure Web İşleri SDK'sı, birçok programlama görevini basitleştirmek için Webİşler ile birlikte kullanılabilir. Daha fazla bilgi için [WebJobs SDK nedir'](https://github.com/Azure/azure-webjobs-sdk/wiki)e bakın.
