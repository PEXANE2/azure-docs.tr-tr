---
title: Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirin | Microsoft Docs
description: Bu makale, Azure Güvenlik Merkezi 'nde kaynaklarınızı izleyerek güvenlik duruşunuzu güçlendirmenize yardımcı olur.
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
ms.openlocfilehash: d18258d62267c931a39947611dda038ff6e65205
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84718708"
---
# <a name="strengthen-your-security-posture-with-azure-security-center"></a>Azure Güvenlik Merkezi ile güvenlik duruşunuzu güçlendirme
Bu makale, güvenlik duruşunuzu güçlendirmenize yardımcı olur. Azure Güvenlik Merkezi 'ndeki izleme yeteneklerini kullanarak kaynak güvenliği mümkün olduğunca sıkı olduğundan ve ilkelerle uyumluluğu izlediğinizden emin olun.

## <a name="how-do-you-strengthen-your-security-posture"></a>Güvenlik duruşunuzu nasıl güçlendirin?
Genellikle izlemeyi, izleme ve bir olayın gerçekleşmesini bekleyip duruma tepki verme olarak ele alırız. Güvenlik duruşunuzu güçleşerek, kurumsal standartları veya en iyi yöntemleri karşılamayan sistemleri belirlemek için kaynaklarınızı denetleyen öngörülü bir stratejiye sahip olunmaktadır.

Bir aboneliğin kaynakları için [güvenlik ilkelerini](tutorial-security-policy.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder. Ağ yapılandırmanız ile ilgili bilgiler hemen kullanımınıza sunulur. Aracının yüklü olduğu VM'ler ve bilgisayarların sayısına bağlı olarak, güvenlik güncelleştirmesi durumu ve işletim sistemi yapılandırması gibi bilgisayar yapılandırma ayarları ve VM’ler hakkında bilgi toplamak bir saat veya daha fazla sürebilir. Sorunların tam listesini ve ağınızı bir bütün olarak görmek ve **öneriler** kutucuğunda riski düzeltmek için kullanabileceğiniz yolları görüntüleyebilirsiniz.

Kaynaklarınızın güvenlik durumunu ve kaynak türü başına tüm sorunları görüntüleyebilirsiniz:

- Bilgisayar kaynaklarınızın ve uygulamalarınızın durumunu izlemek ve güvenliğini iyileştirmeye yönelik öneriler almak için bkz. [Azure Güvenlik Merkezi 'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
- Sanal makineler, ağ güvenlik grupları ve uç noktaları gibi ağ kaynaklarınızı izlemek ve güvenliğini iyileştirmeye yönelik öneriler almak için bkz. daha fazla bilgi için [Azure Güvenlik Merkezi 'nde ağınızı koruma](security-center-network-recommendations.md) . 
- Verilerinizi ve depolama kaynaklarınızı SQL Server 'lar ve depolama hesapları gibi izlemek ve bunların güvenliğini iyileştirmeye yönelik öneriler almak için bkz. daha fazla bilgi için bkz. Azure [Güvenlik Merkezi 'Nde Azure SQL hizmetini ve verilerini koruma](security-center-sql-service-recommendations.md) . 
- MFA ve hesap izinleri dahil olmak üzere kimliğinizi ve erişim kaynaklarını izlemek ve güvenliğini iyileştirmeye yönelik öneriler almak için bkz. daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](security-center-identity-access.md) . 
- Kaynaklarınıza tam zamanında erişimi izlemek için bkz. daha fazla bilgi için [tam zamanında sanal makine erişimini yönetme](security-center-just-in-time.md) .


Önerileri uygulama hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde güvenlik önerilerini uygulama](security-center-recommendations.md)'yı okuyun.



![Kaynaklar güvenlik durumu kutucuğu](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)



## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, Azure Güvenlik Merkezi'nde izleme işlevlerini nasıl kullanacağınız hakkında bilgi edindiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md): Azure Güvenlik Merkezi'nde güvenlik ayarlarını yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.