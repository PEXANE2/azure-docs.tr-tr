---
title: Azure çözümlerinde Oracle WebLogic Server
description: Microsoft Azure 'da Oracle WebLogic Server çalıştırmayı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: rezar
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2020
ms.author: rezar
ms.openlocfilehash: e408f9e245fb78b475a194bc0db6f1edfdf85b41
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069739"
---
# <a name="solutions-for-running-oracle-weblogic-server-on-azure"></a>Azure 'da Oracle WebLogic Server çalıştırmaya yönelik çözümler

Bu sayfada, Azure sanal makinelerinde Oracle WebLogic Server (WLS) çalıştırmaya yönelik çözümler açıklanmaktadır. Bu çözümler, Oracle ve Microsoft tarafından ortaklaşa geliştirilmiştir.

WLS, dünyanın dört bir yanında en çok iş açısından kritik kurumsal Java uygulamalarından bazılarını çalıştıran önde gelen bir Java uygulama sunucusudur. WLS, Oracle yazılım paketi için ara yazılım temelini oluşturur. Oracle ve Microsoft, Azure 'da iş yüklerini önde gelen bir bulut platformu olarak çalıştırmak için, WLS müşterilerinin seçim ve esneklik ile güçlendirin.

Azure WLS çözümleri, en çok ortak işlemleri otomatikleştirerek Java EE uygulamalarınızı Azure sanal makinelerine kaldırıp kaydırmak için mümkün olduğunca kolay hale getirmeye yönelik olarak tasarlanmıştır. Çözümler, sanal ağ, depolama, Java ve Linux kaynaklarını otomatik olarak sağlayın. En az çaba ile WebLogic Server yüklenir. Çözümler bir ağ güvenlik grubu ile güvenliği, Azure uygulama ağ geçidi ile yük dengelemeyi ve Azure Active Directory kimlik doğrulamasını ayarlayabilir. Ayrıca, Azure PostgreSQL, Azure SQL ve Oracle DB Oracle bulut veya Azure 'da da dahil olmak üzere mevcut veritabanınıza otomatik olarak bağlanabilirsiniz. Çözümler için yol haritası, Oracle uyumluluğu aracılığıyla dağıtılmış günlüğe kaydetmeyi ve dağıtılmış önbelleğe almayı etkinleştirme olanağını içerir. Microsoft ve Oracle, WebLogic ve Azure Kubernetes hizmeti (AKS) için benzer işlevleri etkinleştirmek üzere ortaklığı sağlar.

:::image type="content" source="media/oracle-weblogic/wls-on-azure.gif" alt-text="Azure 'da WebLogic Server dağıtmak için Azure portal kullanabilirsiniz":::

Farklı senaryoları karşılamak için kullanabileceğiniz dört teklif vardır: yönetici sunucusu olmayan tek düğüm, yönetim sunucusu, küme ve dinamik küme içeren tek düğüm. Teklifler ücretsiz olarak kullanılabilir. Bu teklifler aşağıda açıklanmıştır ve aşağıda bağlantılıdır.

_Bu teklifler, kendi lisansını getir_. Bunlar, Oracle ile ilgili lisanslarınızın zaten bulunduğunu varsayar ve Azure 'da teklifleri çalıştırmak için doğru lisanslanır.

Teklifler, temel görüntüler (Oracle Linux 7,6 ' de WebLogic Server 14 ve JDK 11 gibi) aracılığıyla bir dizi işletim sistemi, Java ve WLS sürümü destekler. Bu temel görüntüler, Azure 'da kendi kendine de kullanılabilir. Temel görüntüler, karmaşık ve özelleştirilmiş Azure dağıtımları gerektiren müşteriler için uygundur. Geçerli temel görüntü kümesine [buradan](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=WebLogic%20Server%20Base%20Image&page=1)ulaşabilirsiniz.

_Bu teklifleri geliştirirken mühendislik ekibinin geçiş senaryolarınız üzerinde yakından çalışmaya ilgileniyorsanız [CONTACT ME](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview) _ [Market TEKLIFININ Genel Bakış sayfasında](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)benimle iletişim kurun düğmesini seçin. Program yöneticileri, mimarlar ve mühendisler, kısa bir süre sonra tekrar çalışmaya başlar ve işbirliğini kapatın. Bir geçiş senaryosunda işbirliği yapma fırsatı, teklifler etkin geliştirme altındayken ücretsizdir.

## <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server tek düğüm

Bu teklif, tek bir sanal makine sağlar ve üzerine WLS 'yi kurar. Bir etki alanı oluşturmaz veya yönetim sunucusunu başlatır. Tek düğümlü teklif, yüksek düzeyde özelleştirilmiş etki alanı yapılandırmasına sahip senaryolar için faydalıdır.

## <a name="oracle-weblogic-server-with-admin-server"></a>Yönetim sunucusu ile Oracle WebLogic Server

Bu teklif, tek bir sanal makine sağlar ve üzerine WLS 'yi kurar. Bir etki alanı oluşturur ve yönetim sunucusunu başlatır. Etki alanını yönetebilir ve uygulama dağıtımlarını hemen kullanmaya başlayabilirsiniz.

## <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server kümesi

Bu teklif, WLS sanal makinelerinin yüksek düzeyde kullanılabilir bir kümesini oluşturur. Yönetim sunucusu ve tüm yönetilen sunucular varsayılan olarak başlatılır. Kümeyi yönetebilir ve yüksek oranda kullanılabilir uygulamaları hemen hemen kullanmaya başlayabilirsiniz.

## <a name="oracle-weblogic-server-dynamic-cluster"></a>Oracle WebLogic Server dinamik kümesi

Bu teklif, WLS sanal makinelerinin yüksek düzeyde kullanılabilir ve ölçeklenebilir dinamik bir kümesini oluşturur. Yönetim sunucusu ve tüm yönetilen sunucular varsayılan olarak başlatılır.

Çözümler, kapsamlı bir üretime yönelik dağıtım mimarilerinin göreli kolaylıklarla etkinleştirecektir. İş uygulaması geliştirmeye odaklanmaya izin vererek, çoğu geçiş durumunu mümkün olduğunca en verimli şekilde karşılayabilirsiniz.

:::image type="content" source="media/oracle-weblogic/weblogic-architecture-vms.png" alt-text="Azure 'da karmaşık WebLogic Server dağıtımları etkin":::

Çözümler tarafından otomatik olarak sağlananların ötesinde, müşteriler dağıtımlarını daha da özelleştirmek için tamamen esneklik sunar. Büyük olasılıkla uygulama dağıtımı müşterileri, diğer Azure kaynaklarını dağıtımlarıyla tümleştirecektir. Müşterilerin çözümleri daha da geliştirmek için geri bildirim sağlaması önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure 'daki teklifleri keşfedebilirsiniz.

> [!div class="nextstepaction"]
> [Oracle WebLogic Server tek düğüm](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls)

> [!div class="nextstepaction"]
> [Yönetim sunucusu ile Oracle WebLogic Server](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server kümesi](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server dinamik kümesi](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster)
