---
title: ASE v1'deki bir uygulamayı ölçeklendirin
description: Uygulama Hizmeti Ortamında bir uygulamayı ölçekleme. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 43849ca7084f2237c37ad537c50f4e94ac4ea7c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688672"
---
# <a name="scaling-apps-in-an-app-service-environment-v1"></a>Uygulama Hizmet Ortamında ki uygulamaları ölçekleme v1
Azure Uygulama Hizmeti'nde normalde ölçeklendirebileceğiniz üç şey vardır:

* fiyatlandırma planı
* işçi boyutu 
* örnek sayısı.

ASE'de fiyatlandırma planını seçmenize veya değiştirmeye gerek yoktur.  Yetenekler açısından zaten Premium fiyatlandırma yeteneği düzeyindedir.  

Alt bölümlere ilişkin olarak, ASE yöneticisi her alt havuz için kullanılacak bilgi işlem kaynağının boyutunu atayabilir.  Bu, istenirse P4 hesaplama kaynaklarına sahip İşçi Havuzu 1 ve P1 hesaplama kaynaklarına sahip İşçi Havuzu 2'ye sahip olabileceğiniz anlamına gelir.  Onlar boyutu sırada olmak zorunda değildir.  Boyutları ve fiyatlandırmaları ile ilgili ayrıntılar için belgeye buradan bakın [Azure Uygulama Hizmeti Fiyatlandırması.][AppServicePricing]  Bu, bir Uygulama Hizmeti Ortamında web uygulamaları ve Uygulama Hizmeti Planları için ölçekleme seçeneklerini şu şekilde bırakır:

* işçi havuzu seçimi
* örnek sayısı

Her iki öğenin de değiştirilmesi, ASE barındırılan Uygulama Hizmet Planlarınız için gösterilen uygun UI üzerinden yapılır.  

![][1]

ASP'nizi, ASP'nizin içinde bulunduğu alt havuzdaki kullanılabilir işlem kaynağı sayısının ötesine ölçeklendiremezsiniz.  Bu alt havuzda işlem kaynaklarına ihtiyacınız varsa, ASE yöneticinizin bunları eklemesini almanız gerekir.  ASE'nizi yeniden yapılandırma hakkında bilgi için buradaki bilgileri okuyun: [Uygulama Hizmeti ortamını yapılandırma.][HowtoConfigureASE]  Zamanlamaya veya ölçümlere göre kapasite eklemek için ASE otomatik ölçeklendirme özelliklerinden de yararlanmak isteyebilirsiniz.  ASE ortamı nın kendisi için otomatik ölçek yapılandırma hakkında daha fazla bilgi almak [için, Bir Uygulama Hizmet Ortamı için otomatik ölçeklendirmeyi nasıl yapılandırılabildiğinize][ASEAutoscale]bakın.

Farklı alt havuzlardan gelen işlem kaynaklarını kullanarak birden çok uygulama hizmeti planı oluşturabilir veya aynı alt havuzunu kullanabilirsiniz.  Örneğin, İşçi Havuzu 1'de (10) kullanılabilir bilgi işlem kaynaklarınız varsa, (6) bilgi işlem kaynaklarını kullanarak bir uygulama hizmet planı ve (4) bilgi işlem kaynaklarını kullanan ikinci bir uygulama hizmet planı oluşturmayı seçebilirsiniz.

### <a name="scaling-the-number-of-instances"></a>Örnek sayısını ölçekleme
Web uygulamanızı bir Uygulama Hizmet Ortamında ilk oluşturduğunuzda, uygulama 1 örnekle başlar.  Daha sonra, uygulamanız için ek bilgi işlem kaynakları sağlamak için ek örneklere ölçeklendirebilirsiniz.   

ASE yeterli kapasiteye sahipse, bu oldukça basittir.  Ölçeklendirmek istediğiniz siteleri tutan Uygulama Hizmet Planı'nıza gidin ve Ölçek'i seçin.  Bu, ASP'niziçin ölçeği el ile ayarlayabildiğiniz veya ASP'niz için otomatik ölçeklendirme kurallarını yapılandırabileceğiniz u-u-larını açar.  Uygulamanızı el ile ölçeklendirmek ***için, Ölçeği*** ***el ile girdiğim bir örnek sayısına***göre ayarlamanız yeterlidir.  Buradan kaydırıcıyı istenilen miktara sürükleyin veya kaydırıcının yanındaki kutuya girin.  

![][2] 

ASE'deki bir ASP'nin otomatik ölçeklendirme kuralları, normalde olduğu gibi çalışır.  CPU Yüzdesi'ni kullanarak ***ÖLÇEK*** altında ***CPU Yüzdesi'ni*** seçebilir ve CPU Yüzde'nize göre ASP'niz için otomatik ölçeklendirme kuralları oluşturabilir veya zamanlama ve performans ***kurallarını***kullanarak daha karmaşık kurallar oluşturabilirsiniz.  Otomatik ölçek yapılandırma hakkında daha ayrıntılı bilgi görmek için burada kılavuzu kullanın [Azure App Service bir uygulama ölçeklendirin.][AppScale] 

### <a name="worker-pool-selection"></a>İşçi Havuzu seçimi
Daha önce de belirtildiği gibi, işçi havuzu seçimiNE ASP Web'den erişilir.  Ölçeklendirmek istediğiniz ASP için bıçağı açın ve işçi havuzunu seçin.  Uygulama Hizmet Ortamınızda yapılandırdığınız tüm çalışan havuzlarını görürsünüz.  Yalnızca bir çalışan havuzunuz varsa, yalnızca listelenen bir havuzu görürsünüz.  ASP'nizin içinde bulunan işçi havuzunu değiştirmek için, Uygulama Hizmet Planınızın taşınmasını istediğiniz işçi havuzunu seçmeniz yeterlidir.  

![][3]

ASP'nizi bir işçi havuzundan diğerine taşımadan önce, ASP'niz için yeterli kapasiteye sahip olacağınızdan emin olmak önemlidir.  İşçi havuzları listesinde, yalnızca işçi havuzu adı listelenir, aynı zamanda bu işçi havuzunda kaç işçi olduğunu da görebilirsiniz.  Uygulama Hizmet Planınızı içerecek kadar örnek olduğundan emin olun.  Taşınmak istediğiniz işçi havuzunda daha fazla işlem kaynağına ihtiyacınız varsa, ASE yöneticinizin bunları eklemesini alın.  

> [!NOTE]
> Bir ASP'yi bir çalışan havuzundan taşımak, o ASP'deki uygulamaların soğuk başlamasına neden olur.  Bu, uygulamanız yeni bilgi işlem kaynaklarında soğuyurken isteklerin yavaş çalışmasına neden olabilir.  Azure Uygulama Hizmeti'nde uygulama [ısınma özelliği][AppWarmup] kullanılarak soğuk başlangıç önlenebilir.  Makalede açıklanan Uygulama Başlatma modülü, yeni bilgi işlem kaynaklarında uygulamalar soğuduğunda başlatma işlemi de çağrıldığı için soğuk başlangıçlar için de çalışır. 
> 
> 

## <a name="getting-started"></a>Başlarken
Uygulama Hizmet Ortamlarına başlamak için Uygulama [Hizmet Ortamı Nasıl Oluşturulur][HowtoCreateASE]

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ScaleWebapp]: ../manage-scale-up.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[CreateWebappinASE]: app-service-web-how-to-create-a-web-app-in-an-ase.md
[Appserviceplans]: ../overview-hosting-plans.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[AppScale]: ../manage-scale-up.md
[AppWarmup]: https://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
