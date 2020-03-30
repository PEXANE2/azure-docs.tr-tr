---
title: Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirin | Microsoft Dokümanlar
description: Bu makale, Azure Güvenlik Merkezi'ndeki kaynaklarınızı izleyerek güvenlik duruşunuzu güçlendirmenize yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 40352b7ae1f3fb6b1178f2dfe70cdca9871c76b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603777"
---
# <a name="strengthen-your-security-posture-with-azure-security-center"></a>Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirme
Bu makale, güvenlik duruşunuzu güçlendirmenize yardımcı olur. Kaynak güvenliğinizin mümkün olduğunca sıkı olduğundan emin olmak ve ilkelere uygunluğu izlemek için Azure Güvenlik Merkezi'ndeki izleme özelliklerini kullanın.

## <a name="how-do-you-strengthen-your-security-posture"></a>Güvenlik duruşunuzu nasıl güçlendiriyorsunuz?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenlik duruşunuzu güçlendirmek, kuruluş standartlarını veya en iyi uygulamaları karşılamayan sistemleri belirlemek için kaynaklarınızı denetleyen proaktif bir stratejiye sahip olmak anlamına gelir.

Bir aboneliğin kaynakları için [güvenlik ilkelerini](tutorial-security-policy.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur. Aracının yüklü olduğu VM'ler ve bilgisayarların sayısına bağlı olarak, güvenlik güncelleştirmesi durumu ve işletim sistemi yapılandırması gibi bilgisayar yapılandırma ayarları ve VM’ler hakkında bilgi toplamak bir saat veya daha fazla sürebilir. **Öneriler** döşemesinde ağınızı sertleştirmenin ve riski düzeltmenin yollarının tam listesini görüntüleyebilirsiniz.

Kaynaklarınızın güvenlik durumunu ve kaynak türü başına herhangi bir sorunu görüntüleyebilirsiniz:

- Bilgisayar kaynaklarınızın ve uygulamalarınızın sistem durumunu izlemek ve güvenliklerini artırmak için öneriler almak için Azure [Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md) bölümüne bakın
- Sanal makineler, ağ güvenlik grupları ve uç noktalar gibi ağ kaynaklarınızı izlemek ve güvenliklerini artırmak için öneriler almak için daha fazla bilgi için [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md) konusuna bakın. 
- SQL sunucuları ve depolama hesapları gibi veri ve depolama kaynaklarınızı izlemek ve güvenliklerini artırmak için öneriler almak için daha fazla bilgi için [Azure SQL hizmetini ve verilerini Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md) konusuna bakın. 
- MFA ve hesap izinleri de dahil olmak üzere kimliğinizi ve erişim kaynaklarınızı izlemek ve güvenliklerini artırmak için öneriler almak için daha fazla bilgi için [Azure Güvenlik Merkezi'nde Denetimli](security-center-identity-access.md) Görüş kimliği ve erişimi görün. 
- Kaynaklarınıza tam zamanında erişimi izlemek için daha fazla bilgi için [tam zamanında sanal makine erişimini yönet'e](security-center-just-in-time.md) bakın. 


Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.



![Kaynaklar güvenlik durumu kutucuğu](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.