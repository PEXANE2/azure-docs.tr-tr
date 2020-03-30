---
title: VM'ler için Azure Monitörü (GA) sık sorulan sorular | Microsoft Dokümanlar
description: Sanal Ayarlar için Azure Monitörü, Azure VM işletim sisteminin sistem ve performans izleme sini biraraya getiren ve uygulama bileşenlerini ve diğer kaynaklarla olan bağımlılıkları otomatik olarak keşfeden ve aralarındaki iletişimi eşleyen bir çözümdür Onları. Bu makalede, GA sürümü hakkında sık sorulan soruları yanıtlar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/31/2020
ms.openlocfilehash: 3d250ef1aba979be04a44acaf31a3d685f162e37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283897"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>VM'ler için Azure Monitörü Genellikle Kullanılabilir (GA) Sık Sorulan Sorular
Bu Genel Kullanılabilirlik SSS' si, GA için hazırlanırken Q4 2019 ve Q1 2020' de yapılan değişiklikleri kapsar.

## <a name="updates-for-azure-monitor-for-vms"></a>VM'ler için Azure Monitör güncelleştirmeleri
Ocak 2020'de, GA duyurumuzdan önce VM'ler için Azure Monitor'un yeni bir sürümünü yayınladık. VM'ler için Azure Monitor'u etkinleştiren müşteriler artık GA sürümünü alacak, ancak Q4 2019 ve daha önceki VM'ler için Azure Monitor sürümünü kullanan mevcut müşterilerin yükseltmeleri istenecek. Bu SSS, birden çok çalışma alanı arasında büyük dağıtımlarınız varsa, ölçekte yükseltme yapmak için kılavuz sunar.


Bu yükseltmeyle, Sanal Taşıtlar için Azure Monitörü performans verileri [kapsayıcılar için Azure Monitor](container-insights-overview.md)ile aynı *InsightsMetrics* tablosunda depolanır ve bu da iki veri kümesini sorgulamanızı kolaylaştırır. Ayrıca, daha önce kullanılan tabloda depolayamaydığımız daha çeşitli veri kümelerini de depolayabilirsiniz. 

Performans görünümlerimiz artık *InsightsMetrics* tablosunda depoladığımız verileri kullanıyor.  Çalışma alanınızdaki en son VMInsights çözümlerini kullanmak için henüz yükseltme yapmadıysanız, grafikleriniz artık bilgileri görüntülemez.  Aşağıda açıklandığı gibi **Başlangıç** Sayfasımızdan yükseltebilirsiniz.


## <a name="what-is-changing"></a>Ne değişiyor?
VMInsights adlı yeni bir çözüm yayımladık ve bu verileri Log Analytics çalışma alanınızda depolamak için yeni bir konumla birlikte veri toplama için ek özellikler de dahil. 

Geçmişte, Günlük Analizi çalışma alanınızdaki çalışma alanınızdaki ServiceMap çözümünüzü ve kurulum performans sayaçlarını *perf* tablosuna veri göndermek için etkinleştirdik. Bu yeni çözüm, verileri, kapsayıcılar için Azure Monitor tarafından da kullanılan *InsightsMetrics* adlı bir tabloya gönderir. Bu tablo şeması, *Perf* tablo biçimiyle uyumlu olmayan ek ölçümler ve hizmet veri kümeleri depolamamıza olanak tanır.

*InsightsMetrics* tablosunda depoladığımız verileri kullanmak için Performans çizelgelerimizi güncelledik. Aşağıda açıklandığı gibi **Başlangıç** Sayfasımızdan *InsightsMetrics* tablosunu kullanmak için yükseltme yapabilirsiniz.


## <a name="how-do-i-upgrade"></a>Nasıl yükseltebilirim?
Bir Log Analytics çalışma alanı Azure Monitor'un en son sürümüne yükseltildiğinde, bu çalışma alanına eklenen Her VM'deki bağımlılık aracısını yükseltir. Yükseltme gerektiren her VM, Azure portalındaki VM'ler için Azure Monitor'daki **Başlat sekmesinde** tanımlanır. Bir VM yükseltmeyi seçtiğinizde, bu çalışma alanına eklenen diğer VM'lerle birlikte bu VM'nin çalışma alanını yükseltir. Tek bir VM veya birden çok VM, kaynak grubu veya abonelik seçebilirsiniz. 

PowerShell kullanarak bir çalışma alanını yükseltmek için aşağıdaki komutu kullanın:

```PowerShell
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <resource-group-name> -WorkspaceName <workspace-name> -IntelligencePackName "VMInsights" -Enabled $True
```

## <a name="what-should-i-do-about-the-performance-counters-in-my-workspace-if-i-install-the-vminsights-solution"></a>VMInsights çözümlerini yüklersem çalışma alanımdaki Performans sayaçları hakkında ne yapmalıyım?

VM'ler için Azure Monitor'u etkinleştirme yöntemi, çalışma alanınızda performans sayaçlarını kullanmış. Geçerli sürüm bu verileri adlı `InsightsMetrics`bir tabloda depolar. Artık kullanmanız gerekmiyorsa, bu performans sayaçlarını çalışma alanınızda devre dışı bırakmayı seçebilirsiniz. 

>[!NOTE]
>`Perf` Tabloda bu sayaçlara başvuru yapan Uyarı Kuralları varsa, tabloda depolanan yeni `InsightsMetrics` verilere başvurmak için bunları güncelleştirmeniz gerekir. Bu tabloya bakın, örneğin kullanabileceğiniz günlük sorguları için belgelerimize bakın.
>

Performans sayaçlarını etkin tutmaya karar verirseniz, `Perf` [Log Analytics fiyatlandırması[(https://azure.microsoft.com/pricing/details/monitor/).

## <a name="how-will-this-change-affect-my-alert-rules"></a>Bu değişiklik uyarı kurallarımı nasıl etkileyecek?

Çalışma alanında etkinleştirilen `Perf` tablo hedefleme performans [sayaçlarını](../platform/alerts-unified-log.md) sorgulayan Günlük uyarıları oluşturduysanız, bunun yerine `InsightsMetrics` tabloya başvurmak için bu kuralları güncelleştirmeniz gerekir. Bu kılavuz, tüm günlük arama `ServiceMapComputer_CL` kurallarını `ServiceMapProcess_CL`kullanarak ve bu veri `VMComputer` `VMProcess` kümeleri ve tablolar ait olduğundan da geçerlidir.

Bu SSS'yi ve belgelerimizi, topladığımız veri kümeleri için örnek günlük arama uyarı kurallarını içerecek şekilde güncelleştireceğiz.

## <a name="how-will-this-affect-my-bill"></a>Bu faturamı nasıl etkileyecek?

Faturalandırma, Log Analytics çalışma alanınızda alınan ve tutulan verilere dayanmaktadır.

Topladığımız makine düzeyindeperformans verileri aynıdır, `Perf` tabloda depoladığımız verilerle benzer boyuttadır ve yaklaşık olarak aynı tutara mal olur.

## <a name="what-if-i-only-want-to-use-service-map"></a>Yalnızca Hizmet Haritası'nı kullanmak istersem ne olur?

Sorun değil. Yaklaşan güncelleştirme yle ilgili Olarak VM'ler için Azure Monitörünü görüntülerken Azure portalında istemler görürsünüz. Yayımlandıktan sonra, yeni sürüme güncelleştirmenizi isteyen bir istem alırsınız. Yalnızca [Haritalar](vminsights-maps.md) özelliğini kullanmayı tercih ederseniz, Azure Monitor'daki Haritalar özelliğini VM'ler için yükseltmemeyi seçebilir ve Çalışma alanınızdan veya pano döşemenizden erişilen Hizmet Haritası çözümünü Azure Monitörü'nde kullanmaya devam edebilirsiniz.

Çalışma alanınızdaki performans sayaçlarını el ile etkinleştirmeyi seçtiyseniz, Azure Monitor tarafından görüntülenen bazı performans grafiklerimizdeki verileri görebilirsiniz. Yeni çözüm yayımlandıktan sonra `InsightsMetrics` tabloda depolanan verileri sorgulamak için performans çizelgelerimizi güncelleştireceğiz. Bu grafiklerde bu tablodaki verileri görmek istiyorsanız, Sanal Taşıtlar için Azure Monitörünün yeni sürümüne yükseltmeniz gerekir.

Verileri taşıma değişiklikleri `ServiceMapComputer_CL` hem `ServiceMapProcess_CL` Hizmet Haritası'nı hem de Sanal Taşıtlar için Azure Monitörünü etkiler, bu nedenle bu güncelleştirmeyi planlamanız gerekir.

**VMInsights** çözümüne yükseltmemeyi seçtiyseniz, tablodaki verilere atıfta bulunan performans çalışma kitaplarımızın eski `Perf` sürümlerini sunmaya devam edeceğiz.  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>Hizmet Haritası veri setleri insightsMetrics'te de depolanacak mı?

Her iki çözüm de kullanırsanız veri kümeleri çoğaltılamaz. Her iki teklif `VMComputer` de depolanacak veri kümelerini (eski `VMProcess` ServiceMapComputer_CL), (eski ServiceMapProcess_CL) `VMConnection`ve `VMBoundPort` topladığımız harita veri kümelerini depolamak için tabloları paylaşır.  

Tablo, `InsightsMetrics` topladığımız VM, süreç ve hizmet veri kümelerini depolar ve yalnızca VM'ler ve VM Öngörüleri çözümü için Azure Monitor kullanıyorsanız doldurulur. Hizmet Haritası çözümü tabloda veri toplamaz `InsightsMetrics` veya depolamaz.

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>Çalışma alanımda Hizmet Haritası ve VMInsights çözümleri varsa çift ücretlendirilir miyim?

Hayır, `VMComputer` bu iki çözüm, depoladığımız (eski ServiceMapComputer_CL), `VMProcess` (eski ServiceMapProcess_CL), `VMConnection`ve `VMBoundPort`. Çalışma alanınızda her iki çözüm de varsa, bu veriler için çift ücret alınmaz.

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>Hizmet Haritası veya VMInsights çözümlerini kaldırırsam verilerimi kaldırır mı?

Hayır, `VMComputer` bu iki çözüm, depoladığımız (eski ServiceMapComputer_CL), `VMProcess` (eski ServiceMapProcess_CL), `VMConnection`ve `VMBoundPort`. Çözümlerden birini kaldırırsanız, bu veri kümeleri verileri kullanan bir çözümün hala yerinde olduğunu ve bunun Log Analytics çalışma alanında kaldığını fark eder. Verilerin bu çalışma alanından kaldırılabilmesi için her iki çözümü de çalışma alanınızdan kaldırmanız gerekir.

## <a name="health-feature-is-in-limited-public-preview"></a>Sistem durumu özelliği sınırlı genel önizlemede

VM Health özellik setimiz hakkında müşterilerden çok sayıda büyük geri bildirim aldık. Bu özellik ve izleme iş akışlarını destekleme potansiyeli üzerinde heyecan etrafında ilgi bir yeri vardır. İşlevsellik eklemek ve aldığımız geri bildirimleri gidermek için bir dizi değişiklik yapmayı planlıyoruz. 

Bu değişikliklerin yeni müşterilerüzerindeki etkisini en aza indirmek için, bu özelliği sınırlı bir **genel önizlemeye**taşıdık. Bu güncelleştirme Ekim 2019'da gerçekleşti.

Bu Sağlık özelliğini, VM'ler için Azure Monitör'ün GA'da olması sonrasında 2020 yılında yeniden başlatmayı planlıyoruz.

## <a name="how-do-existing-customers-access-the-health-feature"></a>Varolan müşteriler Sistem Durumu özelliğine nasıl erişir?

Sistem Durumu özelliğini kullanan varolan müşteriler bu özellikiçin erişime sahip olmaya devam edecektir, ancak yeni müşterilere sunulmayacaktır.  

Özelliğe erişmek için Azure portal URL'sine `feature.vmhealth=true` [https://portal.azure.com](https://portal.azure.com)aşağıdaki özellik bayrağını ekleyebilirsiniz. Örnek `https://portal.azure.com/?feature.vmhealth=true`.

Özellik bayrağını otomatik olarak ayarlayan bu kısa url'yi de kullanabilirsiniz: [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview).

Varolan bir müşteri olarak, sistem durumu işlevselliği yle varolan bir çalışma alanı kurulumuna bağlı Olan VM'lerde Sistem Durumu özelliğini kullanmaya devam edebilirsiniz.  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>VM Health'i artık tek bir ortamla kullanıyorum ve yeni bir ortama dağıtmak istiyorum

Sağlık özelliğini kullanan ve yeni bir kullanıma sunulması için kullanmak isteyen mevcut bir müşteriyseniz, talimat istemek vminsights@microsoft.com için lütfen bizimle iletişime geçin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizi izlemenize yardımcı olan gereksinimleri ve yöntemleri anlamak [için, SANAL'lar için Azure Monitörünü Dağıt'ı gözden geçirin.](vminsights-enable-overview.md)
