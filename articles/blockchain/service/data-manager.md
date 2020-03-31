---
title: Azure Blockchain Hizmeti için Blockchain Veri Yöneticisi Nedir
description: Blockchain Data Manager, Blockchain verilerini yakalar, dönüştürür ve Olay Izgara Konuları'na sunar.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: chroyal
ms.openlocfilehash: 78c93880007259267b26cf53e93de722be1c7323
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77209452"
---
# <a name="what-is-blockchain-data-manager-for-azure-blockchain-service"></a>Azure Blok Zinciri Hizmeti için Blok Zinciri Veri Yöneticisi nedir?

Blockchain Data Manager, Azure hizmetleriyle güvenilir ve ölçeklenebilir blockchain genel muhasebe entegrasyonu sağlayan Azure Blockchain Hizmeti işlem verilerini Azure Event Grid Topics'e yakalar, dönüştürür ve sunar.

Çoğu kurumsal blockchain senaryosunda blockchain defteri çözümün bir parçasıdır. Örneğin, bir varlığı bir varlıktan diğerine aktarmak için hareketi göndermek için bir mekanizmagerekir. Daha sonra işlemin oluştuğundan, kabul edildiğinden ve ortaya çıkan durum değişikliklerinin uçtan uca çözümünizle tümleştirdiğinden emin olmak için genel muhasebe verilerini okumak için bir mekanizmaya ihtiyacınız vardır. Bu örnekte, varlıkları aktarmak için akıllı bir sözleşme yazarsanız, zincir dışı uygulamaları ve veri depolarını tümleştirmek için Blockchain Data Manager'ı kullanabilirsiniz. Kıymet transferi örneği için, bir varlık blockchain'e aktarıldığında, olaylar ve özellik durumu değişiklikleri Blockchain Data Manager tarafından Event Grid üzerinden teslim edilir. Daha sonra, blockchain verilerini zincir dışında depolamak veya durum değişikliklerine gerçek zamanlı olarak tepki vermek için Olay Izgarası için birden çok olası olay işleyicisi kullanabilirsiniz.

Blockchain Data Manager üç ana işlevi gerçekleştirir: yakalama, dönüştürme ve teslim.

![Blockchain Veri Yöneticisi fonksiyonları](./media/data-manager/functions.png)

## <a name="capture"></a>Capture

Her Blockchain Veri Yöneticisi örneği bir Azure Blockchain Hizmeti üye işlem düğümüne bağlanır. Yalnızca işlem düğümüne erişimi olan kullanıcılar, müşteri verilerine doğru erişim denetimini sağlayan bir bağlantı oluşturabilir. Blockchain Veri Yöneticisi örneği, işlem düğümündeki tüm ham blok ve ham işlem verilerini güvenilir bir şekilde yakalar ve kurumsal iş yüklerini destekleyecek şekilde ölçeklendirilebilir.

## <a name="transform"></a>Dönüşüm

Blockchain Data Manager içinde akıllı sözleşme uygulamaları yapılandırarak olay ve özellik durumunu çözmek için Blockchain Data Manager'ı kullanabilirsiniz. Akıllı bir sözleşme eklemek için, SÖZLEŞME ABI ve bytecode sağlar. Blockchain Data Manager, sözleşme adreslerini çözmek ve bulmak için akıllı sözleşme yapılarını kullanır. Blockchain uygulamasını örneğe ekledikten sonra Blockchain Data Manager, akıllı sözleşme konsorsiyuma dağıtıldığında akıllı sözleşme adresini dinamik olarak keşfeder ve kodlanmış olay ve özellik durumunu yapılandırılmış hedeflere gönderir.

## <a name="deliver"></a>Teslim etme

Blockchain Veri Yöneticisi, belirli bir Blockchain Veri Yöneticisi örneği için birden çok Olay Izgara Konu giden bağlantıları destekler. Blockchain verilerini tek bir hedefe gönderebilir veya blockchain verilerini birden çok hedefe gönderebilirsiniz. Blockchain Data Manager'ı kullanarak, herhangi bir blockchain dağıtımı için ölçeklenebilir olay tabanlı veri yayımlama çözümü oluşturabilirsiniz.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Blockchain Data Manager'ı çözümünüzün gereksinimlerini karşılayacak şekilde yapılandırabilirsiniz. Örneğin, şunları sağlayabilirsiniz:

* Azure Blockchain Hizmeti üyesi için tek bir Blockchain Veri Yöneticisi örneği.
* Azure Blockchain Hizmeti hareket düğümü başına bir Blockchain Veri Yöneticisi örneği. Örneğin, özel işlem düğümlerinin gizliliği korumak için kendi Blockchain Veri Yöneticisi örnekleri olabilir.
* Blockchain Veri Yöneticisi örneği birden çok çıktı bağlantısını destekleyebilir. Bir Blockchain Veri Yöneticisi örneği, bir Azure Blockchain Hizmeti üyesinin tüm veri yayımlama tümleştirme noktalarını yönetmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Blockchain Hizmeti üyesi için [bir Blockchain Veri Yöneticisi örneği oluşturmayı](data-manager-portal.md) deneyin.
