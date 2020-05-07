---
title: VM'ler için Azure İzleyici (GA) sık sorulan sorular | Microsoft Docs
description: VM'ler için Azure İzleyici Azure 'da, Azure VM işletim sisteminin sistem durumunu ve performans izlemesini birleştiren ve uygulama bileşenlerini ve bağımlılıklarını diğer kaynaklarla otomatik olarak bulan ve aralarındaki iletişimi eşleyen bir çözümdür. Bu makalede, GA sürümü hakkında sık sorulan sorular yanıtlanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/31/2020
ms.openlocfilehash: df96ceb47bf33b734f2127bade50af18713a97a0
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82581370"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>VM'ler için Azure İzleyici genel kullanıma açık (GA) sık sorulan sorular
Bu genel kullanılabilirlik SSS, S4 2019 ' de yapılan değişiklikleri ve GA için hazırlandığımız şekilde 2020 ' i içerir.

## <a name="updates-for-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici için güncelleştirmeler
VM'ler için Azure İzleyici Ocak 2020 ' de, GA duyurduğumuz yeni bir sürümünü yayınladık. VM'ler için Azure İzleyici etkinleştiren müşteriler artık GA sürümünü alacak, ancak S4 2019 ve öncesiyle VM'ler için Azure İzleyici sürümünü kullanan mevcut müşterilerin yükseltilmesi istenecektir. Bu SSS, birden çok çalışma alanı genelinde büyük dağıtımlarınız varsa, bir yükseltmeyi ölçekte gerçekleştirmeye yönelik rehberlik sunar.


Bu yükseltme ile VM'ler için Azure İzleyici performans verileri, [kapsayıcılar Için Azure izleyici](container-insights-overview.md)olarak aynı *ınsightsölçümlerini* tablosunda depolanır ve bu da iki veri kümesini sorgulamanızı kolaylaştırır. Ayrıca, daha önce kullanılan tabloda depolayabildiğimiz daha fazla farklı veri kümesi depolayabileceksiniz. 

Performans görünümlerimiz artık *ınsightsölçümlerini* tablosunda depoladığınız verileri kullanıyor.  Çalışma alanınızda en son Vminsıghts çözümünü kullanmak için henüz yükseltmemiş olmanız durumunda, grafiklerinizde artık bilgi görüntülenmeyecektir.  Aşağıda açıklandığı gibi, **kullanmaya başlama** sayfamızı yükseltebilirsiniz.


## <a name="what-is-changing"></a>Ne değişiyor?
Veri toplamaya yönelik ek yetenekler ve bu verileri Log Analytics çalışma alanınızda depolamak için yeni bir konum içeren Vminsıghts adlı yeni bir çözüm yayımladık. 

Geçmişte, çalışma alanınızda ServiceMap çözümünü etkinleştirdik ve verileri *perf* tablosuna göndermek için Log Analytics çalışma alanınızdaki performans sayaçlarını ayarlayın. Bu yeni çözüm, verileri, kapsayıcılar için Azure Izleyici tarafından da kullanılan *ınsightsölçümlerini* adlı bir tabloya gönderir. Bu tablo şeması, *performans* Tablosu biçimiyle uyumlu olmayan ek ölçümleri ve hizmet veri kümelerini depolamamızı sağlar.

Performans grafiklerimizi, *ınsightsölçümlerini* tablosunda depoladığınız verileri kullanacak şekilde güncelleştirdik. Aşağıda açıklandığı gibi, **kullanmaya başlama** sayfamızda bulunan *ınsightsölçümlerini* tablosunu kullanmak için yükseltebilirsiniz.


## <a name="how-do-i-upgrade"></a>Nasıl yaparım? yükseltmesi yapılsın mı?
Bir Log Analytics çalışma alanı Azure Izleyici 'nin en son sürümüne sanal makinelere yükseltildiğinde, bu çalışma alanına eklenen her VM 'nin bağımlılık aracısını yükseltir. Yükseltme gerektiren her VM, Azure portal VM'ler için Azure İzleyici **başlama** sekmesinde belirlenir. Bir VM 'yi yükseltmeyi seçtiğinizde, bu sanal makine için çalışma alanını, bu çalışma alanına bağlı diğer VM 'lerle birlikte yükseltilecektir. Tek bir VM veya birden çok VM, kaynak grubu veya abonelik seçebilirsiniz. 

PowerShell kullanarak bir çalışma alanını yükseltmek için aşağıdaki komutu kullanın:

```PowerShell
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <resource-group-name> -WorkspaceName <workspace-name> -IntelligencePackName "VMInsights" -Enabled $True
```

## <a name="what-should-i-do-about-the-performance-counters-in-my-workspace-if-i-install-the-vminsights-solution"></a>Vminsıghts çözümünü yüklediğimde çalışma alanım 'daki performans sayaçlarından ne yapmam gerekir?

VM'ler için Azure İzleyici çalışma alanınızda kullanılan performans sayaçlarını etkinleştirme için önceki yöntem. Geçerli sürüm, bu verileri adlı `InsightsMetrics`bir tabloda depolar. Artık kullanmanız gerekmiyorsa, çalışma alanınızda bu performans sayaçlarını devre dışı bırakmayı tercih edebilirsiniz. 

>[!NOTE]
>`Perf` Tablodaki bu sayaçlara başvuran uyarı kurallarınız varsa, `InsightsMetrics` tabloda depolanan yeni verilere başvurmak için onları güncelleştirmeniz gerekir. Bu tabloya başvuran, kullanabileceğiniz günlük sorguları için belgelerimize bakın.
>

Performans sayaçlarını etkin tutmaya karar verirseniz, toplanan veriler için faturalandırılır ve bu `Perf` tabloda [Log Analytics fiyatlandırması [(.https://azure.microsoft.com/pricing/details/monitor/)

## <a name="how-will-this-change-affect-my-alert-rules"></a>Bu değişiklik, uyarı kurallarımı nasıl etkiler?

Çalışma alanında etkinleştirilmiş performans sayaçlarını hedefleyen `Perf` tablo hedefleme [günlük uyarıları](../platform/alerts-unified-log.md) oluşturduysanız, bunun yerine `InsightsMetrics` tabloya başvurmak için bu kuralları güncelleştirmeniz gerekir. Bu `ServiceMapComputer_CL` kılavuz, ve `ServiceMapProcess_CL`kullanan tüm günlük arama kuralları için de geçerlidir çünkü bu veri kümeleri `VMComputer` ve `VMProcess` tablolarına taşınır.

Bu SSS 'yi ve belgelerimizi topladığımız veri kümelerine yönelik örnek günlük arama uyarısı kuralları içerecek şekilde güncelleştireceğiz.

## <a name="how-will-this-affect-my-bill"></a>Bu işlem faturanızı nasıl etkiler?

Faturalandırma, Log Analytics çalışma alanınızda alınan ve saklanan verilere göre hala devam etmektedir.

Topladığımız makine düzeyindeki performans verileri aynı, `Perf` tabloda depoladığımız verilere benzer bir boyutludır ve yaklaşık olarak aynı tutara göre ücretlendirilir.

## <a name="what-if-i-only-want-to-use-service-map"></a>Yalnızca Hizmet Eşlemesi kullanmak istersem ne yapmalıyım?

Bu çok uygundur. Yaklaşan güncelleştirme hakkında VM'ler için Azure İzleyici görüntülerken Azure portal istemler görürsünüz. Yayımlandıktan sonra, yeni sürüme güncelleştirmenizi isteyen bir istem alırsınız. Yalnızca [haritalar](vminsights-maps.md) özelliğini kullanmayı tercih ediyorsanız, yükseltmemeyi seçebilirsiniz ve VM'ler için Azure izleyici ve çalışma alanınız veya Pano kutucuğundan erişilen hizmet eşlemesi çözümünde Haritalar özelliğini kullanmaya devam edebilirsiniz.

Çalışma alanınızdaki performans sayaçlarını el ile etkinleştirmeyi seçerseniz, bazı performans grafiklerimizden Azure Izleyici 'den görüntülenen verileri görebilirsiniz. Yeni çözüm yayımlandıktan sonra, `InsightsMetrics` tabloda depolanan verileri sorgulamak için performans grafiklerimizi güncelleştireceğiz. Bu grafiklerde bu tablodaki verileri görmek isterseniz, VM'ler için Azure İzleyici yeni sürümüne yükseltmeniz gerekir.

Ve `ServiceMapComputer_CL` `ServiceMapProcess_CL` ' dan veri taşıma değişiklikleri, hizmet eşlemesi ve VM'ler için Azure izleyici etkileyecektir, bu nedenle yine de bu güncelleştirme için plan yapmanız gerekir.

**Vminsıghts** çözümüne yükseltmemeyi seçerseniz, `Perf` tablodaki verilere başvuran performans çalışma kitaplarımızın eski sürümlerini sağlamaya devam edeceğiz.  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>Hizmet Eşlemesi veri kümeleri de ınsightsölçümler ' de depolanacak mi?

Her iki çözüm de kullanırsanız veri kümeleri yinelenmez. Her iki `VMComputer` teklif de içinde depolanacak veri kümelerini (eski adıyla ServiceMapComputer_CL ServiceMapProcess_CL `VMProcess` `VMConnection`), ve `VMBoundPort` topladığımız harita veri kümelerini depolamak için tabloları paylaşır.  

`InsightsMetrics` Tablo, topladığımız VM, işlem ve hizmet veri kümelerini depolar ve yalnızca VM'LER IÇIN Azure İZLEYICI ve VM öngörüleri çözümünü kullanıyorsanız doldurulur. Hizmet Eşlemesi çözümü `InsightsMetrics` tabloda veri toplamaz veya depolamaz.

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>Çalışma alanım 'da Hizmet Eşlemesi ve Vminsıghts çözümlerini varsa, Double olarak ücretlendirilecektir mıyım?

Hayır, iki çözüm `VMComputer` içinde depoladığımız harita veri kümelerini (eski adıyla ServiceMapComputer_CL) paylaşır `VMProcess` (eski adıyla ServiceMapProcess_CL) `VMConnection`, ve. `VMBoundPort` Çalışma alanınızda her iki çözüm de varsa bu veriler için iki ücret ödemeniz gerekmez.

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>Hizmet Eşlemesi veya Vminsıghts çözümünü kaldırdığımda verilerimi kaldıracağım mi?

Hayır, iki çözüm `VMComputer` içinde depoladığımız harita veri kümelerini (eski adıyla ServiceMapComputer_CL) paylaşır `VMProcess` (eski adıyla ServiceMapProcess_CL) `VMConnection`, ve. `VMBoundPort` Çözümlerden birini kaldırırsanız, bu veri kümeleri, verileri kullanan bir çözüm olduğunu ve Log Analytics çalışma alanında kaldığını fark eder. Verilerin kaldırılmadan kaldırılabilmesi için, her iki çözümü de çalışma alanınızdan kaldırmanız gerekir.

## <a name="health-feature-is-in-limited-public-preview"></a>Sistem durumu özelliği sınırlı genel önizlemede

VM sağlık özelliği kümesiyle ilgili olarak müşterilerden çok sayıda geri bildirim aldık. Bu özelliğin bir çok ilgisi vardır ve izleme iş akışlarını destekleme potansiyelini aşmakatın. İşlevselliği eklemek ve aldığımız geri bildirimi gidermek için bir dizi değişiklik yapmayı planlıyoruz. 

Bu değişikliklerin yeni müşterilere etkilerini en aza indirmek için bu özelliği **sınırlı bir genel önizlemeye**taşıdık. Bu güncelleştirme 2019 Ekim 'de gerçekleşti.

VM'ler için Azure İzleyici GA ' de olduktan sonra bu sistem durumu özelliğini 2020 ' de yeniden başlatmayı planlıyoruz.

## <a name="how-do-existing-customers-access-the-health-feature"></a>Mevcut müşteriler sistem durumu özelliğine nasıl erişir?

Sistem durumu özelliğini kullanan mevcut müşteriler buna erişime sahip olmaya devam eder, ancak yeni müşterilere sunulmayacaktır.  

Bu özelliğe erişmek için Azure portal URL `feature.vmhealth=true` [https://portal.azure.com](https://portal.azure.com)'sine aşağıdaki özellik bayrağını ekleyebilirsiniz. Örnek `https://portal.azure.com/?feature.vmhealth=true`.

Ayrıca, özellik bayrağını otomatik olarak ayarlayan bu kısa URL 'yi de kullanabilirsiniz: [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview).

Mevcut bir müşteri olarak, sistem sağlığı özelliğini mevcut bir çalışma alanı kurulumuna bağlı olan VM 'lerde sistem durumu işleviyle kullanmaya devam edebilirsiniz.  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>VM sistem durumunu şimdi bir ortamla kullanıyorum ve yeni bir ortama dağıtmak istiyorum

Sistem durumu özelliğini kullanan mevcut bir müşterisiyseniz ve yeni bir dağıtım için kullanmak istiyorsanız, yönergeleri istemek için lütfen adresinden vminsights@microsoft.com bizimle iletişim kurun.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizi izlemenize yardımcı olan gereksinimleri ve yöntemleri anlamak için [VM'ler için Azure izleyici dağıt](vminsights-enable-overview.md)' ı inceleyin.
