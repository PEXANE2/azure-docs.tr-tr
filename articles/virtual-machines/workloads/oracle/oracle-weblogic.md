---
title: Azure 'da Oracle WebLogic Server nedir?
description: Microsoft Azure 'da Oracle WebLogic Server çalıştırmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: edburns
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2020
ms.author: edburns
ms.openlocfilehash: fe247e75040f658beb94a66176f802993268a7d8
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87851876"
---
# <a name="what-is-oracle-weblogic-server-on-azure"></a>Azure 'da Oracle WebLogic Server nedir?

Bu sayfada, Azure sanal makinelerinde WebLogic Server (WLS) çalıştırmaya yönelik çözümler açıklanmaktadır.  Bu çözümler, Oracle ve Microsoft tarafından ortaklaşa geliştirilmiştir.

Oracle WebLogic Server, çok katmanlı dağıtılmış kurumsal uygulamalar geliştirmeye ve dağıtmaya yönelik dünyanın ilk bulut Yerel, Enterprise Java platformu uygulama sunucusudur. Azure WebLogic Server teklifleri, bulut bilgi ayracını etkin hale etkinleştirmenizi sağlar.  Java EE uygulamalarınızın Azure bulutuna taşınması ve kaydırılması dahil olmak üzere WebLogic Migration için daha fazla seçenek ve esneklik elde edersiniz.   Azure 'da WLS, küçük bir çabayla büyük bir etkiye sahiptir. Teklifler, iş kolu (LOB) uygulamalarınızı hızlı bir şekilde başlatmayı güçlendirin.  Her teklif, sanal ağ, depolama ve Linux kaynaklarını otomatik olarak sağlar.  Sıfır çabayla WebLogic Server yüklenir.  WLS on Azure, bir ağ güvenlik grubuyla güvenlik, Azure uygulama ağ geçidi ile yük dengeleme, Azure Active Directory kimlik doğrulaması ve otomatik olarak mevcut veritabanınıza bağlanır.

:::image type="content" source="media/oracle-weblogic/wls-on-azure.gif" alt-text="Azure Portal 'ı kullanarak Azure 'da WebLogic Server dağıtımı yapabilirsiniz":::

Farklı senaryoları karşılamak için kullanabileceğiniz dört teklif vardır: yönetici sunucusu olmayan tek düğüm, yönetim sunucusu, küme ve dinamik küme içeren tek düğüm.  Teklifleri deneyin, ücretsiz olarak kullanılabilir.

_Bu teklifler, kendi lisansını getir_. Bunlar, Oracle ile ilgili lisanslarınızın zaten bulunduğunu varsayar ve Microsoft Azure tekliflerini çalıştırmak için doğru lisanslanır.

_Bu teklifleri geliştirmede mühendislik ekibinin geçiş senaryolarınız üzerinde yakından çalışmak istiyorsanız, Azure Marketi 'nde [benimle Iletişim kurun](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview) düğmesini seçin_ . [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview) Program yöneticileri, mimarlar ve mühendisler, kısa bir süre sonra yeniden sunulacaktır ve işbirliği başlatabilir!

## <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server tek düğüm

Bu teklif, tek bir sanal makine sağlar ve üzerine WLS 'yi kurar. Bir etki alanı oluşturmaz veya yönetim sunucusunu başlatır. Tek düğüm, yüksek düzeyde özelleştirilmiş etki alanı yapılandırmasına sahip senaryolar için faydalıdır.

## <a name="oracle-weblogic-server-with-admin-server"></a>Yönetim sunucusu ile Oracle WebLogic Server

Bu teklif, tek bir sanal makine sağlar ve üzerine WLS 'yi kurar. Bir etki alanı oluşturur ve etki alanını yönetmenizi sağlayan yönetim sunucusunu başlatır.

## <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server kümesi

Bu teklif, WLS sanal makinelerinin yüksek düzeyde kullanılabilir bir kümesini oluşturur. Yönetim sunucusu ve tüm yönetilen sunucular varsayılan olarak başlatılır, bu da etki alanını yönetmenizi sağlar.

## <a name="oracle-weblogic-server-dynamic-cluster"></a>Oracle WebLogic Server dinamik kümesi

Bu teklif, WLS sanal makinelerinin yüksek düzeyde kullanılabilir ve ölçeklenebilir dinamik bir kümesini oluşturur. Yönetim sunucusu ve tüm yönetilen sunucular varsayılan olarak başlatılır, bu da etki alanını yönetmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Azure Marketi 'nde teklifleri bulun.

> [!div class="nextstepaction"]
> [Oracle WebLogic Server tek düğüm](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls)

> [!div class="nextstepaction"]
> [Yönetim sunucusu ile Oracle WebLogic Server](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server kümesi](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server dinamik kümesi](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster)
