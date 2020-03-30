---
title: Azure Uygulama Hizmeti web uygulamalarınızı ve API'lerinizi koruyun
description: Bu makale, Azure Güvenlik Merkezi'ndeki Azure Uygulama Hizmeti web uygulamalarınızı ve API'lerinizi korumaya başlamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.topic: conceptual
ms.date: 01/27/2019
ms.author: memildin
ms.openlocfilehash: 2d81e1a1218add504e1e35015276b6924da0e3e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616478"
---
# <a name="protect-your-azure-app-service-web-apps-and-apis"></a>Azure Uygulama Hizmeti web uygulamalarınızı ve API'lerinizi koruyun

Azure Uygulama Hizmeti, altyapıyı yönetmek ten endişe etmeden web uygulamalarınızı ve API'lerinizi oluşturmak ve barındırmak için tamamen yönetilen bir platformdur. Kurumsal kalitede performans, güvenlik ve uyumluluk gereksinimlerini karşılamak için yönetim, izleme ve operasyonel öngörüler sağlar. Daha fazla bilgi için Azure [Uygulama Hizmeti'ne](https://azure.microsoft.com/services/app-service/)bakın.

Azure Uygulama Hizmeti planınız için gelişmiş tehdit koruması sağlamak için şunları

* Azure Güvenlik Merkezi'nin Standart fiyatlandırma katmanına abone olun
* Aşağıda gösterildiği gibi Uygulama Hizmeti planını etkinleştirin. Güvenlik Merkezi, dağıtım ve biniş ihtiyacını ortadan kaldırarak Uygulama Hizmeti ile yerel olarak entegre edilmiştir - entegrasyon saydamdır.
* Özel makinelerle ilişkili bir Uygulama Hizmeti planına sahip. Desteklenen planlar şunlardır: Temel, Standart, Premium, İzole veya Linux. Güvenlik Merkezi, Ücretsiz, Paylaşılan veya Tüketim planlarını desteklemez. Daha fazla bilgi için [Uygulama Hizmeti Planları'na](https://azure.microsoft.com/pricing/details/app-service/plans/)bakın.

Güvenlik Merkezi, Uygulama Hizmeti planının kapsadığı kaynakları değerlendirir ve bulgularına göre güvenlik önerileri oluşturur. Güvenlik Merkezi, Uygulama Hizmetinizin çalıştırıldığı VM örneğini ve yönetim arabirimini korur. Ayrıca, Uygulama Hizmeti'nde çalışan uygulamalarınıza ve uygulamalarınızdan gönderilen istek ve yanıtları da izler.

Güvenlik Merkezi, ortak web uygulaması saldırılarını izlemek için bulutun ölçeğinden ve Azure'un bulut sağlayıcısı olarak sahip olduğu görünürlükten yararlanır. Güvenlik Merkezi, uygulamalarınıza yönelik saldırıları keşfedebilir ve saldırganlar keşif aşamasında yken bile, Azure tarafından barındırılan birden çok uygulamadaki güvenlik açıklarını belirlemek için tarama yapabilir. Azure'un yerli hizmeti olarak Güvenlik Merkezi, bu PaaS'ın altında yatan bilgi işlem düğümlerini kapsayan ana bilgisayar tabanlı güvenlik analitiği sunarak Güvenlik Merkezi'nin zaten istismar edilmiş olan web uygulamalarına yönelik saldırıları algılamasını sağlayan benzersiz bir konumdadır. Daha fazla bilgi için Azure [Uygulama Hizmeti için Tehdit koruması'na](threat-protection.md#app-services)bakın.


## <a name="enabling-monitoring-and-protection-of-app-service"></a>App Hizmetinin izlenmesini ve korunmasını etkinleştirme

1. Azure portalında Güvenlik Merkezi'ni seçin.
2. Fiyatlandırma **& ayarlarına** gidin ve bir abonelik seçin.
3. **Fiyatlandırma katmanı**altında, **Uygulama hizmeti** satırında, planınızı **Etkin'e**geçiş.

    [![Standart katman aboneliğinizde uygulama hizmetlerini etkinleştirme](media/security-center-app-services/app-services-toggle.png)](media/security-center-app-services/app-services-toggle.png#lightbox)


>[!NOTE]
> **Kaynak Miktarınız** için listelenen örnek sayısı, fiyatlandırma katmanı bıçak larını açtığınız anda çalışan bu abonelikteki tüm App Service planlarında toplam bilgi işlem örneği sayısını temsil eder.
>
> Azure Uygulama Hizmeti çeşitli planlar sunar. Uygulama Hizmeti planınız, bir web uygulamasının çalışması için işlem kaynakları kümesini tanımlar. Bu geleneksel web barındırma sunucu çiftlikleri eşdeğerdir. Bir veya daha fazla uygulama aynı bilgi işlem kaynaklarında (veya aynı Uygulama Hizmeti planında) çalışacak şekilde yapılandırılabilir.
>
>Sayımı doğrulamak için, her plan tarafından kullanılan bilgi işlem örneklerinin sayısını görebileceğiniz Azure Portalı'ndaki 'Uygulama Hizmeti planları'na gidin. 






Uygulama Hizmetiniz için izleme ve önerileri devre dışı bırakıp, bu işlemi yineleyin ve **Uygulama Hizmeti** planınızı Devre **Dışı Bırakılmış'a**değiştirin.



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Uygulama hizmetleri](security-center-virtual-machine-protection.md#app-services): Uygulama hizmet ortamlarınızın listesini sağlık özetleriyle görüntüleyin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu yla ilgili blog gönderilerini bulun.
