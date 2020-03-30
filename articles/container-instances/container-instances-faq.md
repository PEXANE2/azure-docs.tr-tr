---
title: Sık sorulan sorular
description: Azure Kapsayıcı Örnekleri hizmetiyle ilgili sık sorulan soruların yanıtları
author: dkkapur
ms.topic: article
ms.date: 01/07/2020
ms.openlocfilehash: 4a3fb4c1818d86f7fe2913790fd9e573c630cbfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247182"
---
# <a name="frequently-asked-questions-about-azure-container-instances"></a>Azure Kapsayıcı Örnekleri hakkında sık sorulan sorular

Bu makalede, Azure Kapsayıcı Örnekleri hakkında sık sorulan sorular giderilen.

## <a name="deployment"></a>Dağıtım

### <a name="how-large-can-my-container-image-be"></a>Konteyner resmim ne kadar büyük olabilir?

Azure Kapsayıcı Örnekleri'nde dağıtılabilir kapsayıcı görüntüsü için maksimum boyut 15 GB'dır. Dağıttığınız anda tam kullanılabilirlik durumuna bağlı olarak daha büyük görüntüler dağıtabilirsiniz, ancak bu garanti edilmez.

Kapsayıcı görüntünüzün boyutu, dağıtımının ne kadar sürdüğünü etkiler, bu nedenle genellikle kapsayıcı görüntülerinizi mümkün olduğunca küçük tutmak istersiniz.

### <a name="how-can-i-speed-up-the-deployment-of-my-container"></a>Konteynerimin dağıtımını nasıl hızlandırabilirim?

Dağıtım sürelerinin ana belirleyicilerinden biri görüntü boyutu olduğundan, boyutu küçültmenin yollarını arayın. İhtiyacınız olmayan katmanları kaldırın veya görüntüdeki katmanların boyutunu küçültün (daha hafif bir temel işletim sistemi görüntüsü seçerek). Örneğin, Linux kapsayıcıları çalıştırıyorsanız, Alp'i tam bir Ubuntu Sunucusu yerine temel resminiz olarak kullanmayı düşünün. Benzer şekilde, Windows kapsayıcıları için mümkünse nano server temel görüntüsü kullanın. 

[Önbelleğe Alınmış Görüntüler Listesi](/rest/api/container-instances/listcachedimages) API'si aracılığıyla kullanılabilen Azure Kapsayıcı Resimleri'nde önceden önbelleğe alınmış görüntülerin listesini de kontrol etmelisiniz. Önceden önbelleğe alınmış görüntülerden biri için görüntü katmanını değiştirebilirsiniz. 

Kapsayıcı başlatma süresini azaltma konusunda daha [ayrıntılı kılavuza](container-instances-troubleshooting.md#container-takes-a-long-time-to-start) bakın.

### <a name="what-windows-base-os-images-are-supported"></a>Hangi Windows temel işletim sistemi görüntüleri desteklenir?

#### <a name="windows-server-2016-base-images"></a>Windows Server 2016 temel resimleri

* [Nano](https://hub.docker.com/_/microsoft-windows-nanoserver)Sunucu `10.0.14393.x`: ,`sac2016`
* [Windows Server](https://hub.docker.com/_/microsoft-windows-servercore) `ltsc2016`Core : ,`10.0.14393.x`

> [!NOTE]
> Yarı Yıllık Kanal sürümü 1709 veya 1803'e dayalı Windows görüntüleri desteklenmez.

#### <a name="windows-server-2019-and-client-base-images-preview"></a>Windows Server 2019 ve istemci temel görüntüleri (önizleme)

* [Nano](https://hub.docker.com/_/microsoft-windows-nanoserver)Sunucu `1809`: ,`10.0.17763.x`
* [Windows Server](https://hub.docker.com/_/microsoft-windows-servercore) `ltsc2019`Core `1809`: , ,`10.0.17763.x`
* [Windows](https://hub.docker.com/_/microsoft-windows) `1809`: ,`10.0.17763.x` 

### <a name="what-net-or-net-core-image-layer-should-i-use-in-my-container"></a>Kapsayıcımda hangi .NET veya .NET Core resim katmanını kullanmalıyım? 

Gereksinimlerinizi karşılayan en küçük görüntüyü kullanın. Linux için,.NET Core 2.1'in yayınlanmasından bu yana desteklenen bir *runtime-alpine* .NET Core görüntüsü kullanabilirsiniz. Windows için, tam .NET Framework kullanıyorsanız, windows server core görüntüsünü *(4.7.2-windowsservercore-ltsc2016*gibi yalnızca çalışma zamanı görüntüsü) kullanmanız gerekir. Yalnızca çalışma zamanı görüntüler daha küçüktür, ancak .NET SDK gerektiren iş yüklerini desteklemez.

## <a name="availability-and-quotas"></a>Kullanılabilirlik ve kotalar

### <a name="how-many-cores-and-memory-should-i-allocate-for-my-containers-or-the-container-group"></a>Konteynerlerime veya kapsayıcı grubuma kaç çekirdek ve bellek ayırmalıyım?

Bu gerçekten iş yükünü bağlıdır. Kapsayıcılarınızın nasıl olduğunu görmek için küçük ve test performansını başlatın. [CPU ve bellek kaynağı kullanımını izleyin](container-instances-monitor.md)ve ardından kapsayıcıda dağıttığınız işlemlerin türüne göre çekirdek veya bellek ekleyin. 

Cpu çekirdekleri ve kapsayıcı grubu başına kullanılabilir bellek üst sınırları için dağıtmakta olduğunuz bölge için [kaynak kullanılabilirliğini](container-instances-region-availability.md#availability---general) de kontrol ettiğinizden emin olun. 

### <a name="what-underlying-infrastructure-does-aci-run-on"></a>ACI hangi altyapıyla çalışır?

Azure Kapsayıcı Örnekleri isteğe bağlı olarak sunucusuz kapsayıcılar hizmeti olmayı hedeflediğiiçin, kapsayıcılarınızı geliştirmeye odaklanmanızı ve altyapı konusunda endişelenmenizi istiyoruz! ACI, başta F ve D serisinden olmak üzere çeşitli SK'lerin Azure VM'leri kümelerinde çalışır. Hizmeti geliştirmeye ve optimize etmeye devam ettikçe bunun gelecekte de değişmesini bekliyoruz. 

### <a name="i-want-to-deploy-thousand-of-cores-on-aci---can-i-get-my-quota-increased"></a>ACI'ye binlerce çekirdek dağıtmak istiyorum - kotamı yükseltebilir miyim?
 
Evet (bazen). Geçerli [kotalar için kota lar ve limitler](container-instances-quotas.md) maddesine ve istek le hangi sınırların artırılabildiğine bakın.

### <a name="can-i-deploy-with-more-than-4-cores-and-16-gb-of-ram"></a>4 çekirdek ve 16 GB'dan fazla RAM ile dağıtabilir miyim?

Henüz bağlanamazsınız. Şu anda, bunlar bir kapsayıcı grubu için maksimum. Azure Destek'e belirli gereksinimler veya isteklerle başvurun. 

### <a name="when-will-aci-be-in-a-specific-region"></a>ACI belirli bir bölgede ne zaman olacak?

Mevcut bölge durumu [burada](container-instances-region-availability.md#availability---general)yayınlanır. Belirli bir bölge için gereksiniminiz varsa Azure Desteği'ne başvurun.

## <a name="features-and-scenarios"></a>Özellikler ve senaryolar

### <a name="how-do-i-scale-a-container-group"></a>Bir konteyner grubunu nasıl ölçeklendirebilirim?

Şu anda, ölçekleme kapsayıcılar veya kapsayıcı grupları için kullanılamaz. Daha fazla örnek çalıştırmanız gerekiyorsa, hizmete kapsayıcı grubu oluşturma için daha fazla istek oluşturmak için API'mizi kullanın. 

### <a name="what-features-are-available-to-instances-running-in-a-custom-vnet"></a>Özel bir VNet'te çalışan örnekler için hangi özellikler kullanılabilir?

Kapsayıcı [gruplarını](container-instances-vnet.md) seçtiğiniz bir Azure sanal ağında dağıtabilir ve Azure kaynaklarınız üzerinden VNet içindeki trafiği yönlendirmek için özel IP'leri kapsayıcı gruplarına devredebilirsiniz. Bir kapsayıcı grubunun sanal ağa dağıtılması şu anda Azure bölgelerinin bir alt kümesindeki üretim iş yükleri için kullanılabilir.

## <a name="pricing"></a>Fiyatlandırma

### <a name="when-does-the-meter-start-running"></a>Sayaç ne zaman çalışmaya başlar?

Kapsayıcı grubu süresi, ilk kapsayıcınızın görüntüsünü çekmeye başladığımız andan (yeni bir dağıtım için) veya kapsayıcı grubunuzun yeniden başlatıldığı andan (zaten dağıtılmışsa), kapsayıcı grubu durdurulana kadar hesaplanır. [Konteyner Örnekleri fiyatlandırması](https://azure.microsoft.com/pricing/details/container-instances/)ile ilgili ayrıntılara bakın.

### <a name="do-i-stop-being-charged-when-my-containers-are-stopped"></a>Konteynerlerim durdurulduğunda ücretlendirilmekten vazgeçer miyim?

Tüm konteyner grubunuz durdurulduktan sonra sayaçlar çalışmayı durdurur. Kapsayıcı grubunuzdaki bir kapsayıcı çalışıyor sayılsa da, kapsayıcıları yeniden başlatmak istemeniz durumunda kaynakları saklarız. 

## <a name="next-steps"></a>Sonraki adımlar

* Azure Kapsayıcı Örnekleri hakkında [daha fazla bilgi edinin.](container-instances-overview.md)
* Azure Kapsayıcı Örnekleri'nde [sık karşılaşılan sorunları giderin.](container-instances-troubleshooting.md)