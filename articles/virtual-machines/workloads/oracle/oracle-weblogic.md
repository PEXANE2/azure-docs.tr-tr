---
title: Oracle WebLogic Server Azure uygulamaları | Microsoft Docs
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
ms.openlocfilehash: 9ba4e3a66ae8ef71b39aa26fd54048381237c2fa
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665218"
---
# <a name="oracle-weblogic-server-azure-applications"></a>Oracle WebLogic Server Azure uygulamaları

## <a name="oracle-weblogic-server-is-a-scalable-enterprise-ready-java-ee-application-server"></a>Oracle WebLogic Server, ölçeklenebilir, kurumsal özellikli bir Java EE uygulama sunucusudur.

Oracle WebLogic Server, çok katmanlı dağıtılmış kurumsal uygulamalar geliştirmeye ve dağıtmaya yönelik dünyanın ilk bulut Yerel, Enterprise Java platformu uygulama sunucusudur. Azure WebLogic Server teklifleri, Java EE uygulamalarınızın en düşük çaba ve en büyük etkiyle Azure bulutuna taşınması ve kaydırılması dahil olmak üzere Web Logic Migration için daha fazla seçenek ve esneklik sağlayarak bulut bilgi ayracını çok daha geniş bir şekilde seçmenizi sağlar. Teklifler, sanal ağ, depolama ve Linux kaynaklarını otomatik olarak sağlayarak, WebLogic Server 'ı yükleyerek, bir ağ güvenlik grubu ile güvenlik dengelemesi, Azure uygulama ağ geçidi ile yük dengeleme, Azure Active Directory kimlik doğrulama ve kolaylaştırıcı veritabanı bağlantısı aracılığıyla iş uygulamalarınızı hızlı bir şekilde başlatmayı güçlendirin.

Farklı senaryoları karşılamak için kullanabileceğiniz dört teklif vardır: yönetici sunucusu olmayan tek düğüm, yönetici sunucusu, küme ve dinamik küme ile tek düğüm.  Bunlara bir deneme hakkı vermeniz gerekir. teklifler ücretsiz olarak kullanılabilir.

_Bu teklifler, kendi lisansını getir_. Bunlar, Oracle ile ilgili lisansları zaten temin ve Microsoft Azure teklifleri çalıştırmak için doğru lisanslanmış olduğunu varsaymaktadır.

_Bu teklifleri geliştiren mühendislik ekibinin geçiş senaryolarınız üzerinde yakından çalışmak istiyorsanız, Azure Marketi 'ndeki [benimle Iletişim kurun](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview) düğmesine basın_ . [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview) Program yöneticileri, mimarlar ve mühendisler kısa bir süre sonra yeniden sunulacaktır ve işbirliği başlatabilir!

### <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server tek düğüm

Bu teklif, tek bir sanal makine sağlar ve buna Oracle WebLogic Server 'ı kurar. Bir etki alanı oluşturmaz veya yönetim sunucusunu başlatır. Bu, yüksek düzeyde özelleştirilmiş etki alanı yapılandırmasına sahip senaryolar için yararlıdır.

### <a name="oracle-weblogic-server-with-admin-server"></a>Yönetim sunucusu ile Oracle WebLogic Server

Bu teklif, tek bir sanal makine sağlar ve buna Oracle WebLogic Server 'ı kurar. Bir etki alanı oluşturur ve etki alanını yönetmenizi sağlayan yönetim sunucusunu başlatır.

### <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server kümesi

Bu teklif, Oracle WebLogic Server sanal makinelerinin yüksek düzeyde kullanılabilir bir kümesini oluşturur. Yönetim sunucusu ve tüm yönetilen sunucular varsayılan olarak başlatılır, bu da etki alanını yönetmenizi sağlar.

### <a name="oracle-weblogic-server-dynamic-cluster"></a>Oracle WebLogic Server dinamik kümesi

Bu teklif, Oracle WebLogic Server sanal makinelerinin yüksek düzeyde kullanılabilir ve ölçeklenebilir dinamik bir kümesini oluşturur. Yönetim sunucusu ve tüm yönetilen sunucular varsayılan olarak başlatılır, bu da etki alanını yönetmenizi sağlar.

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
