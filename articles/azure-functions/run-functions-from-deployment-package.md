---
title: Azure İşlevlerinizi bir paketten çalıştırın
description: Azure İşlevleri çalışma zamanı, işlev uygulama proje dosyalarınızı içeren bir dağıtım paketi dosyası oluşturarak işlevlerinizi çalıştırın.
ms.topic: conceptual
ms.date: 07/15/2019
ms.openlocfilehash: d40896d6a4659945dbeda9ca965366f0b2ca4bd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365280"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Azure İşlevlerinizi bir paket dosyasından çalıştırma

Azure'da, işlevlerinizi doğrudan işlev uygulamanızdaki bir dağıtım paketi dosyasından çalıştırabilirsiniz. Diğer seçenek, dosyalarınızı işlev `d:\home\site\wwwroot` uygulamanızın dizininde dağıtmaktır.

Bu makalede, işlevlerinizi bir paketten çalıştırmanın yararları açıklanmaktadır. Ayrıca, işlev uygulamanızda bu işlevselliğin nasıl etkinleştirilen gösterir.

> [!IMPORTANT]
> İşlevlerinizi [Premium plandaki](functions-scale.md#premium-plan)bir Linux işlev uygulamasına dağıtırken, her zaman paket dosyasından çalışmalı ve [Azure İşleme Temel Araçları'nı kullanarak uygulamanızı yayımlamalısınız.](functions-run-local.md#project-file-deployment)

## <a name="benefits-of-running-from-a-package-file"></a>Paket dosyasından çalıştırmanın yararları
  
Bir paket dosyasından çalıştırmanın çeşitli avantajları vardır:

+ Dosya kopyalama kilitleme sorunları riskini azaltır.
+ Bir üretim uygulamasına dağıtılabilir (yeniden başlatılır).
+ Uygulamanızda çalışan dosyalardan emin olabilirsiniz.
+ [Azure Kaynak Yöneticisi dağıtımlarının](functions-infrastructure-as-code.md)performansını artırır.
+ Özellikle büyük npm paket ağaçlarıyla JavaScript işlevleri için soğuk başlangıç sürelerini azaltabilir.

Daha fazla bilgi için [bu duyuruya](https://github.com/Azure/app-service-announcements/issues/84)bakın.

## <a name="enabling-functions-to-run-from-a-package"></a>Fonksiyonların paketten çalışmasını etkinleştirme

İşlev uygulamanızın bir paketten çalışmasını sağlamak `WEBSITE_RUN_FROM_PACKAGE` için, işlev uygulama ayarlarınıza bir ayar eklemeniz gereken bir ayar oluşturmanız. Ayar `WEBSITE_RUN_FROM_PACKAGE` aşağıdaki değerlerden birine sahip olabilir:

| Değer  | Açıklama  |
|---------|---------|
| **`1`**  | Windows'da çalışan işlev uygulamaları için önerilir. İşlev uygulamanızın klasöründeki `d:\home\data\SitePackages` bir paket dosyadan çalıştırın. Zip [dağıtımı ile dağıtılamiyorsa,](#integration-with-zip-deployment)bu seçenek klasörün de adlı `packagename.txt`bir dosyaolmasını gerektirir. Bu dosya, herhangi bir boşluk olmadan, klasörde paket dosyasının yalnızca adını içerir. |
|**`<URL>`**  | Çalıştırmak istediğiniz belirli bir paket dosyasının konumu. Blob depolama alanını kullanırken, İşlevler'in pakete erişmesini sağlamak için [Paylaşılan Erişim İmzası (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) içeren özel bir kapsayıcı kullanmanız gerekir. Paket dosyalarını Blob depolama hesabınıza yüklemek için [Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md) kullanabilirsiniz. Bir URL belirttiğiniz zaman, güncelleştirilmiş bir paketi yayımladıktan sonra [tetikleyicileri](functions-deployment-technologies.md#trigger-syncing) eşitlemeniz gerekir. |

> [!CAUTION]
> Windows'da bir işlev uygulaması çalıştırırken, harici URL seçeneği daha kötü soğuk başlangıç performansı sağlar. İşlev uygulamanızı Windows'a dağıtırken, zip dağıtımıyla ayarlamanız `WEBSITE_RUN_FROM_PACKAGE` `1` ve yayınlamanız gerekir.

Aşağıda, Azure Blob depolama alanında barındırılan bir .zip dosyasından çalışacak şekilde yapılandırılan bir işlev uygulaması gösterilmektedir:

![WEBSITE_RUN_FROM_ZIP uygulama ayarı](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Şu anda yalnızca .zip paket dosyaları desteklenir.

## <a name="integration-with-zip-deployment"></a>Zip dağıtımı ile tümleştirme

[Zip dağıtımı,][Zip deployment for Azure Functions] Azure Uygulama Hizmeti'nin işlev uygulama projenizi `wwwroot` dizine dağıtmanıza olanak tanıyan bir özelliğidir. Proje .zip dağıtım dosyası olarak paketlenir. Aynı API'ler paketinizi `d:\home\data\SitePackages` klasöre dağıtmak için kullanılabilir. Uygulama ayar değeri `1`ile, zip dağıtım API'leri `d:\home\data\SitePackages` dosyaları ayıklamak yerine `d:\home\site\wwwroot`paketinizi klasöre kopyalayın. `WEBSITE_RUN_FROM_PACKAGE` Ayrıca dosyayı `packagename.txt` oluşturur. Yeniden başlatmadan sonra paket salt `wwwroot` okunur dosya sistemi olarak monte edilir. Zip dağıtımı hakkında daha fazla bilgi için [Azure İşlevler için Zip dağıtımına](deployment-zip-push.md)bakın.

## <a name="adding-the-website_run_from_package-setting"></a>WEBSITE_RUN_FROM_PACKAGE ayarını ekleme

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]


## <a name="troubleshooting"></a>Sorun giderme

- Paketten Çalıştır `wwwroot` salt okunur hale getirir, böylece bu dizine dosya yazarken bir hata alırsınız.
- Katran ve gzip biçimleri desteklenmez.
- Bu özellik yerel önbellekle birlikte oluşturmaz.
- Daha iyi soğuk başlangıç performansı için yerel`WEBSITE_RUN_FROM_PACKAGE`Zip seçeneğini (=1) kullanın.
- Paketten Çalıştır dağıtım özelleştirme seçeneğiyle`SCM_DO_BUILD_DURING_DEPLOYMENT=true`uyumsuzdur ( ), yapı adımı dağıtım sırasında yoksayılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevler için sürekli dağıtım](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
