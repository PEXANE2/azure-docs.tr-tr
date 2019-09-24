---
title: Azure Güvenlik Merkezi 'nde uygulama hizmetlerini koruma | Microsoft Docs
description: Bu makale, Azure Güvenlik Merkezi 'nde uygulama hizmetlerinizi korumaya başlamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.topic: conceptual
ms.date: 01/27/2019
ms.author: memildin
ms.openlocfilehash: 68f7c47f0a0f56085d632f1c1741318f440b41ee
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202471"
---
# <a name="protect-app-service-with-azure-security-center"></a>Azure Güvenlik Merkezi ile App Service koruma
Bu makale, App Service üstünde çalışan uygulamalarınızı izlemek ve korumak için Azure Güvenlik Merkezi 'ni kullanmanıza yardımcı olur.

App Service, altyapıyı yönetmeksizin seçtiğiniz programlama dilinde Web uygulamaları oluşturmanıza ve barındırmanıza olanak sağlar. App Service otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux 'un yanı sıra GitHub, Azure DevOps veya herhangi bir git deposundan otomatik dağıtımları destekler. 

Web uygulamalarındaki güvenlik açıkları, saldırganların neredeyse her kuruluşun ortak ve dinamik bir arabirimine sahip oldukları için genellikle saldırganlar tarafından kullanılır. App Service üzerinde çalışan uygulamalara yönelik istekler, dünyanın dört bir yanındaki Azure veri merkezlerinde dağıtılan çeşitli ağ geçitlerinden geçerek her isteği ilgili uygulamaya yönlendirmekten sorumludur. 

Azure Güvenlik Merkezi, VM 'nizin veya istek üzerine örnekinizdeki korumalı alanlardaki App Service çalıştıran uygulamalarınızda değerlendirme ve öneriler çalıştırabilir. Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğden yararlanarak, genellikle birden çok hedef genelinde çalıştırılan ortak Web uygulaması saldırılarını izlemek üzere App Service iç günlüklerinizi analiz eder.

Güvenlik Merkezi, App Service uygulamalarınızda saldırıları tanımlamak ve gelişmekte olan saldırılara odaklanırken, saldırganlar keşif aşamasında olduğunda, saldırganlar, Azure 'da barındırılan birden çok Web sitesinde güvenlik açıklarını belirlemek için tarama yapar. Güvenlik Merkezi, müşterilerin, HTTP üzerinden veya bir yönetim yöntemi aracılığıyla uygulamalarıyla etkileşime geçmesini sağlayan tüm arabirimleri kapsayan analiz ve makine öğrenimi modellerini kullanır. Üstelik, Azure 'da birinci taraf bir hizmet olarak Güvenlik Merkezi, bu PaaS 'nin temel işlem düğümlerini kapsayan ana bilgisayar tabanlı güvenlik analizlerinin yanı sıra, güvenlik merkezi 'nin şu Web uygulamalarına karşı saldırıları algılamasını sağlar. zaten kullanılıyor.

## <a name="prerequisites"></a>Önkoşullar

App Service ortamınızı izlemek ve güvenliğini sağlamak için ayrılmış makinelerle ilişkilendirilen bir App Service planına sahip olmanız gerekir. Bu planlar şunlardır: Temel, Standart, Premium, Yalıtılmış veya Linux. Azure Güvenlik Merkezi Ücretsiz, Paylaşılan veya Tüketim planlarını desteklemez. Daha fazla bilgi için bkz. [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).

## <a name="security-center-protection"></a>Güvenlik Merkezi koruması

Azure Güvenlik Merkezi, App Service çalıştığı sanal makine örneğini ve yönetim arabirimini korur. Ayrıca, App Service ' de çalışan uygulamalarınıza gönderilen istekleri ve yanıtları izler.

Güvenlik Merkezi, App Service ile yerel olarak tümleşiktir ve dağıtım ve ekleme gereksinimini ortadan kaldırır; tümleştirme tamamen saydamdır.



## <a name="enabling-monitoring-and-protection-of-app-service"></a>App Service izlemeyi ve korumayı etkinleştirme

1. Azure 'da Güvenlik Merkezi ' ni seçin.
2. **Fiyatlandırma & ayarları** ' na gidin ve bir abonelik seçin.
3. **Fiyatlandırma katmanı**altında, **App Service** satırında planınızı **etkin**olarak değiştirin.

![App Service değiştirme](./media/security-center-app-services/app-services-toggle.png)

>[!NOTE]
> Kaynak miktarınız için listelenen örnek sayısı, fiyatlandırma katmanı dikey penceresini açtığınızda şu anda etkin olan ilgili App Service örneklerinin sayısını temsil eder. Bu sayı, seçtiğiniz ölçekleme seçeneklerine bağlı olarak değişeceğinden, ücretlendirilecek örneklerin sayısı buna göre değiştirilir.

App Service izlemeyi ve önerilerini devre dışı bırakmak için bu işlemi tekrarlayın ve **App Service** planınızı **devre dışı**olarak değiştirin.



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md): Azure Güvenlik Merkezi 'nde güvenlik ayarlarını yapılandırmayı öğrenin.
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Uygulama hizmetleri](security-center-virtual-machine-protection.md#app-services):  Durum özetleri olan App Service ortamlarınızın listesini görüntüleyin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik MERKEZI SSS](security-center-faq.md): Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
