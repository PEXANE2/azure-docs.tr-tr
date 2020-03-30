---
title: Hizmet Kumaş uygulama yükseltme
description: Bu makalede, yükseltme modları seçme ve sistem durumu denetimleri gerçekleştirme dahil olmak üzere bir Hizmet Kumaşı uygulamasını yükseltmek için bir giriş sağlar.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: 2dc484b49c5250510e5f018cbbc2da107573d452
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259051"
---
# <a name="service-fabric-application-upgrade"></a>Hizmet Kumaş uygulama yükseltme
Azure Hizmet Kumaşı uygulaması bir hizmet koleksiyonudur. Yükseltme sırasında, Service Fabric yeni [uygulama bildirimini](service-fabric-application-and-service-manifests.md) önceki sürümle karşılaştırır ve uygulamadaki hangi hizmetlerin güncelleştirmegerektirdiğini belirler. Service Fabric, hizmet bildirimlerinde bulunan sürüm numaralarını önceki sürümdeki sürüm numaralarıyla karşılaştırır. Bir hizmet değişmediyse, bu hizmet yükseltilmez.

## <a name="rolling-upgrades-overview"></a>Rolling yükseltmeleri genel bakış
Yuvarlanma uygulaması yükseltmesinde yükseltme aşamalı olarak gerçekleştirilir. Her aşamada, yükseltme kümedeki güncelleştirme etki alanı olarak adlandırılan düğümalt kümesine uygulanır. Sonuç olarak, uygulama yükseltme boyunca kullanılabilir kalır. Yükseltme sırasında küme, eski ve yeni sürümlerin bir karışımını içerebilir.

Bu nedenle, iki sürüm ileri ve geri uyumlu olmalıdır. Bunlar uyumlu değilse, uygulama yöneticisi kullanılabilirliği korumak için çok aşamalı bir yükseltme evreleme sorumludur. Çok aşamalı yükseltmede, ilk adım uygulamanın önceki sürümle uyumlu bir ara sürümüne yükseltiliyor. İkinci adım, ön güncelleştirme sürümüyle uyumluluğu bozan, ancak ara sürümle uyumlu olan son sürümü yükseltmektir.

Güncelleştirme etki alanları küme yi yapılandırdığınızda küme bildiriminde belirtilir. Güncelleştirme etki alanları belirli bir sırada güncelleştirme almaz. Güncelleştirme etki alanı, bir uygulama için mantıksal bir dağıtım birimidir. Etki alanlarını güncelleştirme, hizmetlerin yükseltme sırasında yüksek kullanılabilirlikte kalmasını sağlar.

Yükseltme kümedeki tüm düğümlere uygulanırsa, yuvarlanmayan yükseltmeler mümkündür, ki bu uygulamanın yalnızca bir güncelleştirme etki alanı olduğunda olduğu gibi. Hizmet indiğinden ve yükseltme sırasında kullanılamadığından, bu yaklaşım önerilmez. Ayrıca, Azure, bir küme yalnızca bir güncelleştirme etki alanıyla ayarlandığında herhangi bir garanti sağlamaz.

Yükseltme tamamlandıktan sonra, tüm hizmetler ve yinelemeler (örnekler) aynı sürümde kalır, yükseltme başarılı olursa, yeni sürüme güncelleştirilir; yükseltme başarısız olursa ve geri alınırsa, eski sürüme geri alınır.

## <a name="health-checks-during-upgrades"></a>Yükseltmeler sırasında sağlık kontrolleri
Yükseltme için sistem durumu ilkelerinin ayarlanmış olması (veya varsayılan değerlerin kullanılması gerekir). Tüm güncelleştirme etki alanları belirtilen zaman zaman ları içinde yükseltildiğinde ve tüm güncelleştirme etki alanları sağlıklı kabul edildiğinde yükseltme başarılı olarak adlanır.  Sağlıklı güncelleştirme etki alanı, güncelleştirme etki alanının sistem durumu ilkesinde belirtilen tüm sistem durumu denetimlerini geçtiği anlamına gelir. Örneğin, bir sağlık ilkesi, bir uygulama örneği içindeki tüm hizmetlerin *sağlıklı*olmasını zorunlu kılabilir , çünkü sağlık Hizmet Kumaşı tarafından tanımlanır.

Service Fabric tarafından yükseltme sırasında sağlık politikaları ve kontrolleri hizmet ve uygulama agnostik vardır. Diğer bir diğer olarak, hizmete özel testler yapılmaz.  Örneğin, hizmetinizin bir iş verme gereksinimi olabilir, ancak Hizmet Kumaşı'nın iş çıkışını denetleyebilecek bilgileri yoktur. Yapılan denetimler için [sağlık makalelerine](service-fabric-health-introduction.md) bakın. Yükseltme sırasında meydana gelen denetimler, uygulama paketinin doğru kopyalanıp kopyalanmadığını, örneğin başlatılıp başlatılmadığını ve benzeri testleri içerir.

Uygulama durumu, uygulamanın alt varlıklarının bir toplamasidır. Kısacası, Hizmet Kumaş uygulama rapor edilen sağlık yoluyla uygulamanın sağlığını değerlendirir. Ayrıca uygulama için tüm hizmetlerin sağlığını bu şekilde değerlendirir. Service Fabric, hizmet çoğaltma gibi çocuklarının sağlığını toplayarak uygulama hizmetlerinin sağlığını daha da değerlendirir. Uygulama durumu ilkesi karşılandıktan sonra yükseltme devam edebilir. Sistem durumu ilkesi ihlal edilirse, uygulama yükseltmesi başarısız olur.

## <a name="upgrade-modes"></a>Yükseltme modları
Uygulama yükseltmesi için önerdiğimiz mod, yaygın olarak kullanılan mod olan izlenen moddur. İzlenen mod yükseltmeyi tek bir güncelleştirme etki alanında gerçekleştirir ve tüm sistem durumu denetimleri geçerse (belirtilen ilkeye göre), bir sonraki güncelleştirme etki alanına otomatik olarak geçer.  Sistem durumu denetimleri başarısız olursa ve/veya zaman çıkışlarına ulaşılırsa, yükseltme güncelleştirme etki alanı için geri alınır veya mod izlenmeyen kılavuza değiştirilir. Başarısız yükseltmeler için bu iki moddan birini seçmek üzere yükseltmeyi yapılandırabilirsiniz. 

İzlenmeyen manuel mod, bir sonraki güncelleştirme etki alanında yükseltmebaşlatmak için, bir güncelleştirme etki alanında her yükseltme sonra manuel müdahale gerekir. Servis Kumaşı sağlık kontrolü yapılmaz. Yönetici, bir sonraki güncelleştirme etki alanında yükseltmeyi başlatmadan önce sistem durumu veya durum denetimlerini gerçekleştirir.

## <a name="upgrade-default-services"></a>Varsayılan hizmetleri yükseltme
[Uygulama bildiriminde](service-fabric-application-and-service-manifests.md) tanımlanan bazı varsayılan hizmet parametreleri de uygulama yükseltmesinin bir parçası olarak yükseltilebilir. Yalnızca [Update-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricservice?view=azureservicefabricps) ile değiştirilmelerini destekleyen hizmet parametreleri yükseltmenin bir parçası olarak değiştirilebilir. Uygulama yükseltmesırasında varsayılan hizmetleri değiştirme davranışı aşağıdaki gibidir:

1. Kümede zaten bulunmayan yeni uygulama bildiriminde varsayılan hizmetler oluşturulur.
2. Önceki ve yeni uygulama bildirimlerinde bulunan varsayılan hizmetler güncelleştirilir. Yeni uygulama bildirimindeki varsayılan hizmetin parametreleri, varolan hizmetin parametrelerinin üzerine yazılır. Varsayılan bir hizmeti güncelleştirme başarısız olursa, uygulama yükseltmesi otomatik olarak geri alır.
3. Kümede varsa, yeni uygulama bildiriminde bulunmayan varsayılan hizmetler silinir. **Varsayılan bir hizmetin silmesinin, bu hizmetin durumunun tamamının silmesine neden olacağını ve geri alınamayacağını unutmayın.**

Bir uygulama yükseltmesi geri alındığunda, varsayılan hizmet parametreleri yükseltme başlamadan önce eski değerlerine geri döndürülür, ancak silinen hizmetler eski durumları ile yeniden oluşturulamaz.

> [!TIP]
> [EnableDefaultServicesYükseltme](service-fabric-cluster-fabric-settings.md) küme config ayarı (varsayılan hizmet güncelleştirmeve silme) yukarıdaki kurallar 2) ve 3 etkinleştirmek için *doğru* olmalıdır. Bu özellik Service Fabric sürüm 5.5'ten başlayarak desteklenir.

## <a name="upgrading-multiple-applications-with-https-endpoints"></a>HTTPS uç noktalarıyla birden çok uygulamayı yükseltme
HTTP**S**kullanırken aynı uygulamanın farklı örnekleri için **aynı bağlantı noktasını** kullanmamaya dikkat etmalısınız. Bunun nedeni, Hizmet Kumaşı'nın uygulama örneklerinden biri için sertifikayı yükseltememesidir. Örneğin, uygulama 1 veya uygulama 2 her ikisi de cert 1'i cert 2'ye yükseltmek istiyorlarsa. Yükseltme gerçekleştiğinde, Diğer uygulama hala kullanıyor olsa bile Service Fabric http.sys ile cert 1 kaydını temizlenmiş olabilir. Bunu önlemek için Service Fabric, bağlantı noktasına sertifika (http.sys nedeniyle) kayıtlı başka bir uygulama örneği olduğunu algılar ve işlemi başarısız eder.

Bu nedenle Service Fabric, farklı uygulama örneklerinde aynı bağlantı noktasını kullanarak iki farklı **hizmetin** yükseltilmesine destek vermez. Başka bir deyişle, aynı bağlantı noktasında farklı hizmetlerde aynı sertifikayı kullanamazsınız. Aynı bağlantı noktasında paylaşılan bir sertifikaya sahip olmanız gerekiyorsa, hizmetlerin yerleşim kısıtlamaları olan farklı makinelere yerleştirildiğinden emin olmanız gerekir. Veya her bir uygulama örneğinde her hizmet için mümkünse Service Fabric dinamik bağlantı noktalarını kullanmayı düşünün. 

https ile bir yükseltmenin başarısız olduğunu görürseniz, "Windows HTTP Server API, bağlantı noktasını paylaşan uygulamalar için birden çok sertifikayı desteklemez" diyen bir hata uyarısı.

## <a name="application-upgrade-flowchart"></a>Uygulama yükseltme akış şeması
Bu paragrafı izleyen akış şeması, Bir Hizmet Kumaşı uygulamasının yükseltme işlemini anlamanıza yardımcı olabilir. Özellikle, akış nasıl zaman-outs açıklar, *HealthCheckStableDuration*dahil, *HealthCheckRetryTimeout*, ve *UpgradeHealthCheckInterval*, bir güncelleştirme etki alanında yükseltme bir başarı veya başarısızlık olarak kabul edildiğinde kontrol yardımcı olur.

![Hizmet Kumaşı Uygulaması için yükseltme işlemi][image]

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak Uygulama yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme ile size yol.

[PowerShell kullanarak Uygulamanızı yükseltme](service-fabric-application-upgrade-tutorial-powershell.md) powershell kullanarak bir uygulama yükseltme ile size yol.

[Yükseltme](service-fabric-application-upgrade-parameters.md)Parametreleri'ni kullanarak uygulamanızın nasıl yükselttikini kontrol edin.

[Veri Serileştirme'nin](service-fabric-application-upgrade-data-serialization.md)nasıl kullanılacağını öğrenerek uygulama yükseltmelerinizi uyumlu hale getirin.

[Gelişmiş Konular'a](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.

[Sorun Giderme Uygulama Yükseltmeleri'ndeki](service-fabric-application-upgrade-troubleshooting.md)adımlara atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.

[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png
