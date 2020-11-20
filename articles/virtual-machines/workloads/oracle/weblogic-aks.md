---
title: Azure Kubernetes hizmetinde Oracle WebLogic Server çalıştırmaya yönelik çözümler nelerdir?
description: Oracle WebLogic Server 'ı Azure Kubernetes hizmetinde çalıştırmayı öğrenin.
author: rezar
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.date: 10/28/2020
ms.author: rezar
ms.reviewer: cynthn
ms.openlocfilehash: a57d5a00fad9a17e01b96ebdf395fb7a4b857935
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94968698"
---
# <a name="what-are-solutions-for-running-oracle-weblogic-server-on-the-azure-kubernetes-service"></a>Azure Kubernetes hizmetinde Oracle WebLogic Server çalıştırmaya yönelik çözümler nelerdir?

Bu sayfada, Azure Kubernetes Service (AKS) üzerinde Oracle WebLogic Server (WLS) çalıştırmaya yönelik çözümler açıklanmaktadır. Bu çözümler, Oracle ve Microsoft tarafından ortaklaşa geliştirilmiştir ve desteklenir.

Web Logic Server 'ı Azure sanal makinelerinde çalıştırmak da mümkündür. Bunu yapmak için çözümler [Bu Microsoft makalesinde](/azure/virtual-machines/workloads/oracle/oracle-weblogic)açıklanmaktadır.

WebLogic Server, dünyanın dört bir yanında en çok görev açısından kritik kurumsal Java uygulamalarından bazılarını çalıştıran önde gelen bir Java uygulama sunucusudur. Web Logic Server, Oracle yazılım paketi için ara yazılım temelini oluşturur. Oracle ve Microsoft, Azure 'da iş yüklerini önde gelen bulut platformu olarak çalıştırmak için Web Logic Server müşterilerinin tercih ve esnekliğe sahip olmasını sağlar.

## <a name="wls-on-aks-certified-and-supported"></a>AKS sertifikalı ve desteklenen WLS 'ler
Web Logic Server, Oracle ve Microsoft tarafından AKS 'de iyi çalışacak şekilde sertifikalandırilmiştir. AKS çözümlerinde WebLogic Server, kapsayıcınızı çalıştırmak ve Docker ve Kubernetes altyapısında bulunan Java EE uygulamalarını yönetmek için mümkün olduğunca kolay hale getirmeye yönelik olarak tasarlanmıştır. Çözümler güvenilirlik, ölçeklenebilirlik, yönetilebilirlik ve kurumsal desteğe odaklanılmıştır.

WebLogic Server kümeleri, WebLogic Kubernetes Işleci aracılığıyla Kubernetes üzerinde çalışmak üzere tamamen etkinleştirilmiştir (burada ' operator ', burada ' operator ' olarak adlandırılır). Işleci standart Kubernetes işleç düzenlerini izler. Alternatif olarak el ile görevleri otomatikleştirerek ve ek işlemsel güvenilirlik özellikleri ekleyerek Kubernetes üzerindeki WebLogic etki alanlarının ve dağıtımlarının yönetimini ve çalışmasını basitleştirir. Işleci, Oracle WebLogic Server 12c, Oracle Fusion ara yazılım altyapısını 12c ve sonrasını destekler. WebLogic Server 12.2.1.3 ve 12.2.1.4 için resmi Docker görüntülerini Işleçle test ettik. Işleci hakkında daha fazla bilgi için [Oracle 'daki resmi belgelere](https://oracle.github.io/weblogic-kubernetes-operator/)bakın.

## <a name="guidance-scripts-and-samples-for-wls-on-aks"></a>AKS üzerinde WLS için rehberlik, betikler ve örnekler
Web Logic Server 'ı AKS üzerinde sertifikalandırın ötesinde Oracle ve Microsoft, AKS üzerinde WebLogic Server çalıştırmaya yönelik ayrıntılı yönergeler, betikler ve örnekler sağlar. Bu kılavuz, [operatör belgelerinin](https://oracle.github.io/weblogic-kubernetes-operator/samples/simple/azure-kubernetes-service/)Azure Kubernetes hizmeti örnek bölümüne eklenmiştir. Bu kılavuz, AKS dağıtımlarında üretim Web Logic Server 'ı mümkün olduğunca kolay hale getirmeyi hedefler. Rehberlik, Oracle tarafından sunulan resmi WebLogic Server Docker görüntülerini kullanır. Yük devretme işlemi, Kubernetes kalıcı birim talepleri aracılığıyla erişilen Azure dosyaları aracılığıyla gerçekleştirilir. Azure yük dengeleyiciler, ' LoadBalancer ' türünde bir Kubernetes hizmeti kullanılarak sağlandığında desteklenir. Rehberlik, yüksek ölçüde yapılandırma ve özelleştirmeye izin verir.

:::image type="content" source="media/oracle-weblogic/wls-on-aks.gif" alt-text="AKS üzerinde WebLogic Server dağıtmak için örnek betikleri kullanabilirsiniz":::

Kılavuz Şu anda, etki alanını Docker görüntüsünün dışında ve Oracle 'daki standart Docker görüntülerini kullanarak dağıtmaktadır. Bir Docker görüntüsü içinde etki alanı ile özel görüntüleri etkinleştirmeye yönelik rehberlik ekleyeceğiz. Daha iyi kullanım kolaylığı ve Azure hizmet tümleştirmeleri, Azure sanal makineler çözümlerinde Oracle WebLogic Server 'ın Market sunumlarını yansıtarak daha da olasıdır.

_Bu çözümler kendi lisansını getir_. Bunlar, Oracle ile ilgili lisanslarınızın zaten bulunduğunu varsayar ve Azure 'da teklifleri çalıştırmak için doğru lisanslanır.

_Bu çözümleri geliştiren mühendislik ekibi ile geçiş senaryolarınız üzerinde yakından çalışmaya ilgileniyorsanız, [Bu kısa anketi](https://aka.ms/wls-on-azure-survey) doldurun ve iletişim bilgilerinizi ekleyin_. Program yöneticileri, mimarlar ve mühendisler, kısa bir süre sonra tekrar çalışmaya başlar ve işbirliğini kapatın. Bir geçiş senaryosunda işbirliği yapma fırsatı, çözümler etkin ilk geliştirme altındayken ücretsizdir.

## <a name="deployment-architectures"></a>Dağıtım mimarileri

Azure Kubernetes hizmetinde Oracle WebLogic Server çalıştırmaya yönelik çözümler, çok çeşitli üretime hazır dağıtım mimarilerini göreli kolaylıklar sağlar.

:::image type="content" source="media/oracle-weblogic/weblogic-architecture-aks.png" alt-text="AKS üzerinde karmaşık WebLogic Server dağıtımları etkinleştirilmiştir":::

Çözümlerin ötesinde müşterilerin dağıtımlarını daha da özelleştirmek için tamamen esnekliği vardır. Büyük olasılıkla uygulama dağıtımı müşterileri, diğer Azure kaynaklarını dağıtımlarıyla tümleştirecektir. Müşterilerin çözümleri daha da geliştirmek için ankette geri bildirimde bulunmak önerilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kubernetes hizmetinde Oracle WebLogic Server çalıştırmayı inceleyin.

> [!div class="nextstepaction"]
> [AKS üzerinde WLS çalıştırmaya yönelik yönergeler, betikler ve örnekler](https://oracle.github.io/weblogic-kubernetes-operator/samples/simple/azure-kubernetes-service/)

> [!div class="nextstepaction"]
> [WebLogic Kubernetes Işleci](https://oracle.github.io/weblogic-kubernetes-operator/)
