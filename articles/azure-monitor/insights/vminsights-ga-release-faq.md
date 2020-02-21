---
title: VM'ler için Azure İzleyici (GA) sık sorulan sorular | Microsoft Docs
description: VM'ler için Azure İzleyici Azure 'da, Azure VM işletim sisteminin sistem durumunu ve performans izlemesini birleştiren bir çözümdür ve uygulama bileşenlerini ve bağımlılıklarını diğer kaynaklarla otomatik olarak bulabilir ve arasındaki iletişimi eşler yapıştırabilirsiniz. Bu makalede, GA sürümü hakkında sık sorulan sorular yanıtlanmaktadır.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/31/2020
ms.openlocfilehash: 673cda4005d9c985d8d6ee5ef1d28a3d8c241ac0
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77482893"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>VM'ler için Azure İzleyici genel kullanıma açık (GA) sık sorulan sorular

Bu genel kullanılabilirlik SSS, GA sürümümüzü hazırlarken VM'ler için Azure İzleyici oluşan değişiklikleri içerir. 

## <a name="updates-for-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici için güncelleştirmeler

VM'ler için Azure İzleyici yeni bir sürümünü yayımladık. VM 'Ler için Azure Izleyicilerini etkinleştiren müşteriler artık yeni sürümü alacak, ancak zaten VM'ler için Azure İzleyici kullanan mevcut müşterilerin yükseltilmesi istenecektir. Bu SSS ve Belgelerimiz, birden çok çalışma alanı genelinde büyük dağıtımlarınız varsa, bir yükseltmeyi bir yükseltme gerçekleştirmeye yönelik rehberlik sunar.

Bu yükseltme ile VM'ler için Azure İzleyici performans verileri, [kapsayıcılar Için Azure izleyici](container-insights-overview.md)olarak aynı *ınsightsölçümlerini* tablosunda depolanır ve bu da iki veri kümesini sorgulamanızı kolaylaştırır. Ayrıca, daha önce kullanılan tabloda depolayabildiğimiz daha fazla farklı veri kümesi depolayabileceksiniz. 

Performans görünümlerimiz artık *ınsightsölçümlerini* tablosunda depoladığınız verileri kullanıyor.  Çalışma alanınızda en son Vminsıghts çözümünü kullanmak için henüz yükseltmemiş olmanız durumunda, grafiklerinizde artık bilgi görüntülenmeyecektir.  Aşağıda açıklandığı gibi, **kullanmaya başlama** sayfamızı yükseltebilirsiniz.

Bu yükseltme ile, VM'ler için Azure İzleyici performans verileri, iki veri kümesini sorgulamanızı kolaylaştıran [kapsayıcılar Için Azure izleyici](container-insights-overview.md) Ile aynı *ınsightsölçümlerini* tablosunda depolanır. Daha önce kullanılan tabloda depolayabildiğimiz daha farklı veri kümelerini de depolayabileceksiniz. 

Var olan müşterilerin yükseltmesini isteyen iş akışının kesintiye uğramasına neden olduğunu fark ettik. bu nedenle, bu işlemi daha sonra daha sonra genel önizleme aşamasında yapmayı tercih ediyoruz.


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

VM'ler için Azure İzleyici etkinleştirme geçerli yöntemi, çalışma alanınızdaki performans sayaçlarını kullanır. Yeni yöntem, bu verileri `InsightsMetrics`adlı yeni bir tabloda depolar.

Kullanıcı arabirimimizi `InsightsMetrics` tablosundaki verileri kullanmak üzere güncelleştirdikten sonra belgelerimizi güncelleştiririz ve bu duyuruyu, Azure portal bir başlık görüntüleme dahil olmak üzere birden çok kanal aracılığıyla iletişim kuracak. Bu noktada, artık bunları kullanmanıza gerek kalmadığında, çalışma alanınızda bu [performans sayaçlarını](vminsights-enable-overview.md#performance-counters-enabled) devre dışı bırakmayı tercih edebilirsiniz. 

>[!NOTE]
>`Perf` tablosundaki bu sayaçlara başvuran uyarı kurallarınız varsa, bunları `InsightsMetrics` tablosunda depolanan yeni verilere başvuracak şekilde güncelleştirmeniz gerekir. Bu tabloya başvuran, kullanabileceğiniz günlük sorguları için belgelerimize bakın.
>

Performans sayaçlarını etkin tutmaya karar verirseniz, [Log Analytics fiyatlandırması [(https://azure.microsoft.com/pricing/details/monitor/)temel alınarak `Perf` tabloda saklanan ve depolanan veriler için faturalandırılırsınız.

## <a name="how-will-this-change-affect-my-alert-rules"></a>Bu değişiklik, uyarı kurallarımı nasıl etkiler?

Çalışma alanında etkinleştirilmiş olan `Perf` tablo hedefleme performans sayaçlarını sorgulayan [günlük uyarıları](../platform/alerts-unified-log.md) oluşturduysanız, bu kuralları, bunun yerine `InsightsMetrics` tablosuna başvuracak şekilde güncelleştirmeniz gerekir. Bu kılavuz Ayrıca, `ServiceMapComputer_CL` ve `ServiceMapProcess_CL`kullanan tüm günlük arama kuralları için de geçerlidir çünkü bu veri kümeleri `VMComputer` ve `VMProcess` tablolarına taşınır.

Bu SSS 'yi ve belgelerimizi topladığımız veri kümelerine yönelik örnek günlük arama uyarısı kuralları içerecek şekilde güncelleştireceğiz.

## <a name="how-will-this-affect-my-bill"></a>Bu işlem faturanızı nasıl etkiler?

Faturalandırma, Log Analytics çalışma alanınızda alınan ve saklanan verilere göre hala devam etmektedir.

Topladığımız makine düzeyi performans verileri aynı, `Perf` tablosunda depoladığınız verilere benzer bir boyutludır ve yaklaşık olarak aynı miktarda maliyetlidir.

## <a name="what-if-i-only-want-to-use-service-map"></a>Yalnızca Hizmet Eşlemesi kullanmak istersem ne yapmalıyım?

Bu çok uygundur. Yaklaşan güncelleştirme hakkında VM'ler için Azure İzleyici görüntülerken Azure portal istemler görürsünüz. Yayımlandıktan sonra, yeni sürüme güncelleştirmenizi isteyen bir istem alırsınız. Yalnızca [haritalar](vminsights-maps.md) özelliğini kullanmayı tercih ediyorsanız, yükseltmemeyi seçebilirsiniz ve VM'ler için Azure izleyici ve çalışma alanınız veya Pano kutucuğundan erişilen hizmet eşlemesi çözümünde Haritalar özelliğini kullanmaya devam edebilirsiniz.

Çalışma alanınızdaki performans sayaçlarını el ile etkinleştirmeyi seçerseniz, bazı performans grafiklerimizden Azure Izleyici 'den görüntülenen verileri görebilirsiniz. Yeni çözüm yayımlandıktan sonra, `InsightsMetrics` tablosunda depolanan verileri sorgulamak için performans grafiklerimizi güncelleştireceğiz. Bu grafiklerde bu tablodaki verileri görmek isterseniz, VM'ler için Azure İzleyici yeni sürümüne yükseltmeniz gerekir.

`ServiceMapComputer_CL` ve `ServiceMapProcess_CL` verileri taşıma değişiklikleri hem Hizmet Eşlemesi hem de VM'ler için Azure İzleyici etkiler, bu nedenle yine de bu güncelleştirme için plan yapmanız gerekir.

**Vminsıghts** çözümüne yükseltmemeyi seçerseniz, `Perf` tablosundaki verilere başvuran performans çalışma kitaplarımızın eski sürümlerini sağlamaya devam edeceğiz.  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>Hizmet Eşlemesi veri kümeleri de ınsightsölçümler ' de depolanacak mi?

Her iki çözüm de kullanırsanız veri kümeleri yinelenmez. Her iki teklif de `VMComputer` (eski adıyla ServiceMapComputer_CL), `VMProcess` (eski adıyla ServiceMapProcess_CL), `VMConnection`ve `VMBoundPort` tablolarında depolanacak veri kümelerini paylaşır ve bu da topladığımız harita veri kümelerini depolar.  

`InsightsMetrics` tablosu, topladığımız VM, işlem ve hizmet veri kümelerini depolar ve yalnızca VM'ler için Azure İzleyici ve VM öngörüleri çözümünü kullanıyorsanız doldurulur. Hizmet Eşlemesi çözümü `InsightsMetrics` tablosunda veri toplamaz veya depolamaz.

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>Çalışma alanım 'da Hizmet Eşlemesi ve Vminsıghts çözümlerini varsa, Double olarak ücretlendirilecektir mıyım?

Hayır, iki çözüm `VMComputer` (eski adıyla ServiceMapComputer_CL), `VMProcess` (eski adıyla ServiceMapProcess_CL), `VMConnection`ve `VMBoundPort`depoladığımız harita veri kümelerini paylaşır. Çalışma alanınızda her iki çözüm de varsa bu veriler için iki ücret ödemeniz gerekmez.

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>Hizmet Eşlemesi veya Vminsıghts çözümünü kaldırdığımda verilerimi kaldıracağım mi?

Hayır, iki çözüm `VMComputer` (eski adıyla ServiceMapComputer_CL), `VMProcess` (eski adıyla ServiceMapProcess_CL), `VMConnection`ve `VMBoundPort`depoladığımız harita veri kümelerini paylaşır. Çözümlerden birini kaldırırsanız, bu veri kümeleri, verileri kullanan bir çözüm olduğunu ve Log Analytics çalışma alanında kaldığını fark eder. Verilerin kaldırılmadan kaldırılabilmesi için, her iki çözümü de çalışma alanınızdan kaldırmanız gerekir.

## <a name="when-will-this-update-be-released"></a>Bu güncelleştirme ne zaman yayımlanacak?

Güncelleştirme 2020 ' den erken Ocak ' de VM'ler için Azure İzleyici için yayımlanması bekleniyor. Ocak ayında yayın tarihine yaklaşdığımızda, güncelleştirmeleri buraya yayımlayacaktır ve Azure Izleyici 'yi açtığınızda Azure portal bildirimler sunacağız.

## <a name="health-feature-is-in-limited-public-preview"></a>Sistem durumu özelliği sınırlı genel önizlemede

VM sağlık özelliği kümesiyle ilgili olarak müşterilerden çok sayıda geri bildirim aldık. Bu özelliğin bir çok ilgisi vardır ve izleme iş akışlarını destekleme potansiyelini aşmakatın. İşlevselliği eklemek ve aldığımız geri bildirimi gidermek için bir dizi değişiklik yapmayı planlıyoruz. 

Bu değişikliklerin yeni müşterilere etkilerini en aza indirmek için bu özelliği **sınırlı bir genel önizlemeye**taşıdık. Bu güncelleştirme 2019 Ekim 'de gerçekleşti.

VM'ler için Azure İzleyici GA ' de olduktan sonra bu sistem durumu özelliğini 2020 ' de yeniden başlatmayı planlıyoruz.

## <a name="how-do-existing-customers-access-the-health-feature"></a>Mevcut müşteriler sistem durumu özelliğine nasıl erişir?

Sistem durumu özelliğini kullanan mevcut müşteriler buna erişime sahip olmaya devam eder, ancak yeni müşterilere sunulmayacaktır.  

Bu özelliğe erişmek için, Azure portal URL 'SI [https://portal.azure.com](https://portal.azure.com)`feature.vmhealth=true` aşağıdaki özellik bayrağını ekleyebilirsiniz. Örnek `https://portal.azure.com/?feature.vmhealth=true`.

Ayrıca, özellik bayrağını otomatik olarak ayarlayan bu kısa URL 'yi de kullanabilirsiniz: [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview).

Mevcut bir müşteri olarak, sistem sağlığı özelliğini mevcut bir çalışma alanı kurulumuna bağlı olan VM 'lerde sistem durumu işleviyle kullanmaya devam edebilirsiniz.  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>VM sistem durumunu şimdi bir ortamla kullanıyorum ve yeni bir ortama dağıtmak istiyorum

Sistem durumu özelliğini kullanan mevcut bir müşterisiyseniz ve bunu yeni bir dağıtım için kullanmak istiyorsanız, lütfen yönergeler istemek için vminsights@microsoft.com adresinden bizimle iletişime geçin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizi izlemenize yardımcı olan gereksinimleri ve yöntemleri anlamak için [VM'ler için Azure izleyici dağıt](vminsights-enable-overview.md)' ı inceleyin.
