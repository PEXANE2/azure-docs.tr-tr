---
title: Web uygulaması sorunlarını bulmak için Azure Monitör'de Uygulama Değişikliği Analizi'ni kullanma | Microsoft Dokümanlar
description: Azure Uygulama Hizmeti'ndeki canlı sitelerdeki uygulama sorunlarını gidermek için Azure Monitor'da Uygulama Değişikliği Çözümlemesi'ni kullanın.
ms.topic: conceptual
author: cawams
ms.author: cawa
ms.date: 05/07/2019
ms.openlocfilehash: 036b8c084bdfdc11c02274758c550c76bdc7b1e7
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348743"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Azure Monitör'de Uygulama Değişikliği Çözümlemesi (önizleme) kullanma

Canlı bir site sorunu veya kesintisi oluştuğunda, kök nedenini hızla belirlemek önemlidir. Standart izleme çözümleri sizi bir soruna karşı uyarabilir. Hangi bileşenin başarısız olduğunu bile gösterebilirler. Ancak bu uyarı her zaman başarısızlığın nedenini hemen açıklamaz. Sitenizin beş dakika önce çalıştığını biliyorsunuz ve şimdi bozuldu. Son beş dakikada ne değişti? Uygulama Değişikliği Çözümlemesi'nin Azure Monitor'da yanıtlamak üzere tasarladığı soru budur.

[Azure Kaynak Grafiği'nin](https://docs.microsoft.com/azure/governance/resource-graph/overview)gücünden daha fazla bilgi alan Change Analysis, gözlemlenebilirliği artırmak ve MTTR'yi (ortalama onarım süresi) azaltmak için Azure uygulama değişikliklerinize ilişkin öngörüler sağlar.

> [!IMPORTANT]
> Değişiklik Çözümlemesi şu anda önizlemede. Bu önizleme sürümü, hizmet düzeyi sözleşmesi olmadan sağlanır. Bu sürüm, üretim iş yükleri için önerilmez. Bazı özellikler desteklenmeyebilir veya kısıtlı özelliklere sahip olabilir. Daha fazla bilgi için Microsoft [Azure önizlemeleri için ek kullanım koşullarına](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="overview"></a>Genel Bakış

Değişiklik Çözümlemesi, altyapı katmanından uygulama dağıtımına kadar çeşitli değişiklik türlerini algılar. Abonelikteki kaynak değişikliklerini denetleyen abonelik düzeyinde bir Azure kaynak sağlayıcısıdır. Değişiklik Çözümlemesi, kullanıcıların sorunlara neden olabilecek değişiklikleri anlamalarına yardımcı olmak için çeşitli tanılama araçları için veri sağlar.

Aşağıdaki diyagram, Değişim Çözümlemesi mimarisini göstermektedir:

![Değişim Çözümlemesi'nin değişim verilerini nasıl aldığını ve istemci araçlarına nasıl sağladığının mimari diyagramı](./media/change-analysis/overview.png)

Şu anda Değişiklik Analizi, Uygulama Hizmeti web uygulamasındaki **Sorunları Tanıla ve Çözme** deneyimine ve Azure portalında tek başına bir sekme olarak kullanılabilir.
Bu makalenin ilerleyen bölümlerinde Web Uygulaması portalında kullanmak için Değişiklik Analizi'ne ve *Web Uygulamaları için Değişiklik Analizi* bölümüne erişmek için Azure *bölümündeki tüm kaynakların Görüntüleme değişikliklerine* bakın.

### <a name="azure-resource-manager-tracked-properties-changes"></a>Azure Kaynak Yöneticisi izlenen özellikler değişiklikleri

[Azure Kaynak Grafiği'ni](https://docs.microsoft.com/azure/governance/resource-graph/overview)kullanarak Değişiklik Analizi, uygulamanızı barındıran Azure kaynaklarının zaman içinde nasıl değiştiğini gösteren geçmiş bir kayıt sağlar. Yönetilen kimlikler, Platform Işletim Sistemi yükseltmesi ve ana bilgisayar adları gibi izlenen ayarlar algılanabilir.

### <a name="azure-resource-manager-proxied-setting-changes"></a>Azure Kaynak Yöneticisi yakın ayar değişiklikleri
IP Yapılandırma kuralı, TLS ayarları ve uzantı sürümleri gibi ayarlar henüz ARG'de mevcut değildir, bu nedenle Çözümle'yi değiştir sorguları ve uygulamada ne değişti hakkında daha fazla bilgi sağlamak için bu değişiklikleri güvenli bir şekilde hesaplar. Bu bilgiler henüz Azure Kaynak Grafiği'nde kullanılamıyor, ancak yakında kullanıma sunulacaktır.

### <a name="changes-in-web-app-deployment-and-configuration-in-guest-changes"></a>Web uygulaması dağıtımı ve yapılandırmasındaki değişiklikler (konuk değişiklikleri)

Değişiklik Çözümlemesi, her 4 saatte bir bir uygulamanın dağıtım ve yapılandırma durumunu yakalar. Örneğin, uygulama ortamı değişkenlerinde değişiklikleri algılayabilir. Araç farklılıkları hesaplar ve değişenleri sunar. Kaynak Yöneticisi değişikliklerinin aksine, kod dağıtım değişikliği bilgileri araçta hemen kullanılamayabilir. Değişiklik Çözümlemesi'ndeki en son değişiklikleri görüntülemek **için, değişiklikleri şimdi tarayın'ı**seçin.

!["Şimdi tarayın" düğmesinin ekran görüntüsü](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Bağımlılık değişiklikleri

Kaynak bağımlılıklarında yapılan değişiklikler, bir web uygulamasında sorunlara da neden olabilir. Örneğin, bir web uygulaması Redis önbelleğine çağrı yaparsa, Redis önbelleği SKU web uygulamasının performansını etkileyebilir. Bağımlılıkdeğişiklikleri algılamak için, Değişiklik Analizi web uygulamasının DNS kaydını denetler. Bu şekilde, sorunlara neden olabilecek tüm uygulama bileşenlerindeki değişiklikleri tanımlar.
Şu anda aşağıdaki bağımlılıklar desteklenir:
- Web Apps
- Azure Storage
- Azure SQL

### <a name="enablement"></a>Geçerlilik
"Microsoft.ChangeAnalysis" kaynak sağlayıcısının Azure Kaynak Yöneticisi'nin izlenen özellikleri için bir aboneliğe kaydedilmesi ve yakın ayarların kullanılabilir olması için veri değiştirmesi gerekir. Web Uygulaması tanılama ve sorunları çözmek aracı girdiğinizde veya Tek başına Değişiklik Analizi sekmesini getirmek, bu kaynak sağlayıcısı otomatik olarak kaydedilir. Aboneliğiniz için herhangi bir performans ve maliyet uygulaması yoktur. Web uygulamaları için Değişiklik Analizi'ni etkinleştirdiğinizde (veya sorunları Tanıla ve Çöz aracında etkinleştirme), web uygulaması üzerinde ihmal edilebilir performans etkisi ne olur ve faturalandırma maliyeti yoktur.
Konuk içi web uygulaması değişiklikleri için, bir web uygulaması içindeki kod dosyalarını taramak için ayrı etkinleştirme gerekir. Daha fazla bilgi için [tanıtanı'daki Değişiklik Çözümlemesini Etkinleştir ve daha](https://docs.microsoft.com/azure/azure-monitor/app/change-analysis#enable-change-analysis-in-the-diagnose-and-solve-problems-tool) fazla ayrıntı için bu makalenin ilerleyen bölümlerinde sorunları çözme aracı bölümüne bakın.


## <a name="viewing-changes-for-all-resources-in-azure"></a>Azure'daki tüm kaynaklar için değişiklikleri görüntüleme
Azure Monitor'da, tüm değişiklikleri öngörüler ve uygulama bağımlılıkları kaynaklarıyla görüntülemek için Değişiklik Analizi için bağımsız bir bıçak vardır.

Bıçağı başlatmak için Azure portalındaki arama çubuğunda Değişiklik Analizi'ni arayın.

![Azure portalında Değişiklik Analizi aramaekran görüntüsü](./media/change-analysis/search-change-analysis.png)

Değişiklikleri görüntülemeye başlamak için Kaynak Grubu ve kaynakları seçin.

![Azure portalında Değişiklik Analizi ekranı](./media/change-analysis/change-analysis-standalone-blade.png)

Uygulamanızı barındıran Öngörüler ve ilgili bağımlılık kaynaklarını görebilirsiniz. Bu görünüm, geliştiricilerin sorunları gidermek için uygulama merkezli olarak tasarlanmıştır.

Şu anda desteklenen kaynaklar şunlardır:
- Virtual Machines
- Sanal Makine Ölçek Seti
- Azure Ağ kaynakları
- Misafir dosya izleme ve ortam değişkenleri değişiklikleri ile Web uygulaması

Herhangi bir geri bildirim için, bıçak veya changeanalysisteam@microsoft.come-posta geribildirim gönder düğmesini kullanın.

![Değişim Analizi bıçağında geri bildirim düğmesinin ekran görüntüsü](./media/change-analysis/change-analysis-feedback.png)

## <a name="change-analysis-for-the-web-apps-feature"></a>Web Uygulamaları özelliği için Analiz değiştir

Azure Monitor'da Değişiklik Çözümlemesi, self servis **tanılama ve sorunları çözme** deneyiminde de yerleşiktir. Bu deneyime Uygulama Hizmeti uygulamanızın **Genel Bakış** sayfasından erişin.

!["Genel Bakış" düğmesinin ekran görüntüsü ve "Sorunları tanıla ve çöz" düğmesi](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Tanılama da Değişiklik Çözümlemesini etkinleştirin ve sorunları çözme aracı

1. **Kullanılabilirlik ve Performans'ı**seçin.

    !["Kullanılabilirlik ve Performans" sorun giderme seçeneklerinin ekran görüntüsü](./media/change-analysis/availability-and-performance.png)

1. **Uygulama Değişikliklerini**Seçin. Bu özellik **Uygulama Kilitlenmeleri**de kullanılabilir değil.

   !["Uygulama Çöküyor" düğmesinin ekran görüntüsü](./media/change-analysis/application-changes.png)

1. Çözümle'yi Değiştir'i etkinleştirmek için **şimdi etkinleştir'i**seçin.

   !["Uygulama Çöküyor" seçeneklerinin ekran görüntüsü](./media/change-analysis/enable-changeanalysis.png)

1. **Çözümle çözümlemesi'ni** açın ve **Kaydet'i**seçin. Araç, tüm web uygulamalarını bir Uygulama Hizmeti planı altında görüntüler. Plan kapsamındaki tüm web uygulamaları için Değişiklik Analizi'ni açmak için plan düzeyi anahtarını kullanabilirsiniz.

    !["Değişim Analizini Etkinleştir" kullanıcı arabiriminin ekran görüntüsü](./media/change-analysis/change-analysis-on.png)


1. Değişiklik Çözümlemesi'ne erişmek için **Tanıla'yı seçin ve sorunları** > **ÇözÜn Kullanılabilirlik ve Performans** > **Uygulama Kilitlenmeleri.** Zaman içinde değişiklik türünü özetleyen bir grafik ve bu değişikliklerle ilgili ayrıntılar görürsünüz. Varsayılan olarak, son 24 saat içinde değişiklikler acil sorunlara yardımcı olmak için görüntülenir.

     ![Change diff görünümü ekran görüntüsü](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Ölçekte Değişim Analizini Etkinleştir

Aboneliğinizde çok sayıda web uygulaması varsa, web uygulaması düzeyinde hizmet etkinleştirmek verimsiz olacaktır. Aboneliğinizdeki tüm web uygulamalarını etkinleştirmek için aşağıdaki komut dosyasını çalıştırın.

Önkoşullar:
* PowerShell Az modülü. [Azure PowerShell modüllerini yükleyin](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.6.0) yönergeleri izleyin

Şu betiği çalıştırın:

```PowerShell
# Log in to your Azure subscription
Connect-AzAccount

# Get subscription Id
$SubscriptionId = Read-Host -Prompt 'Input your subscription Id'

# Make Feature Flag visible to the subscription
Set-AzContext -SubscriptionId $SubscriptionId

# Register resource provider
Register-AzResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis"


# Enable each web app
$webapp_list = Get-AzWebApp | Where-Object {$_.kind -eq 'app'}
foreach ($webapp in $webapp_list)
{
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId $webapp.Id -Tag $tags -Force
}

```



## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Hizmetleri uygulamaları](azure-web-apps.md)için Uygulama Öngörülerini etkinleştirin.
- Azure VM ve Azure sanal makine ölçeği için Uygulama Öngörüleri'ni [etkinleştirme IIS tarafından barındırılan uygulamaları ayarlayın.](azure-vm-vmss-apps.md)
- Güç Değişim Analizi'ne yardımcı olan [Azure Kaynak Grafiği](https://docs.microsoft.com/azure/governance/resource-graph/overview)hakkında daha fazla bilgi edinin.
