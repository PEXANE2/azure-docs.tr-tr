---
title: VM'ler için Azure Monitör nedir?
description: Uygulama bileşenlerini ve bağımlılıklarını otomatik olarak keşfetmenin ve eşlemenin yanı sıra Azure Sanal Taşıtlarının sistem durumunu ve performansını izleyen Sanal Taşıtlar için Azure Monitör'e genel bakış.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/11/2020
ms.openlocfilehash: 6f65ba96f768a4b88aa13c073cc66920b8c5e049
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480496"
---
# <a name="what-is-azure-monitor-for-vms"></a>VM'ler için Azure Monitör nedir?

VM'ler için Azure Monitor, Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerini ölçekte izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder. Şirket içinde veya başka bir bulut sağlayıcısında barındırılan VM'ler için performans ve uygulama bağımlılıklarını izleme desteği içerir. Aşağıdaki temel özellikler ayrıntılı bir bakış açısı sağlar:

- **Önceden tanımlanmış trend performans grafikleri**: Konuk VM işletim sisteminden temel performans ölçümlerini görüntüleyin.

- **Bağımlılık haritası**: Çeşitli kaynak gruplarından ve aboneliklerden VM ile birbirine bağlı bileşenleri görüntüler.  

>[!NOTE]
>Yakın zamanda, genel önizleme müşterilerimizden aldığımız geri bildirimlere dayanarak Sağlık özelliğinde yaptığımız [değişiklikleri duyurduk.](https://azure.microsoft.com/updates/updates-to-azure-monitor-for-virtual-machines-preview-before-general-availability-release/
) Yapacağımuz değişikliklerin sayısı göz önüne alındığında, yeni müşteriler için Sağlık özelliğini sunmayı bırakacağız. Varolan müşteriler sistem durumu özelliğini kullanmaya devam edebilir. Daha fazla bilgi için lütfen [Genel Kullanılabilirlik SSS](vminsights-ga-release-faq.md)bölümüne bakın.  

Azure Monitör Günlükleri ile tümleştirme, güçlü toplama ve filtreleme sağlayarak Azure Monitor'un VM'ler için zaman içinde veri eğilimlerini analiz etmesine olanak tanır. Bu verileri doğrudan sanal makineden tek bir VM'de görüntüleyebilir veya görünümün Azure kaynak bağlamını veya çalışma alanı bağlam modlarını desteklediği Sanal'larınızın toplu görünümünü sunmak için Azure Monitor'u kullanabilirsiniz. Daha fazla bilgi için [erişim modlarına genel bakış](../platform/design-logs-deployment.md#access-mode)alabakın.

![Azure portalında sanal makine öngörüleri perspektifi](media/vminsights-overview/vminsights-azmon-directvm.png)

Sanal Tom'lar için Azure Monitor, önemli uygulamaların öngörülebilir performansını ve kullanılabilirliğini sağlayabilir. Performans darboğazlarını ve ağ sorunlarını tanımlar ve bir sorunun diğer bağımlılıklarla ilişkili olup olmadığını anlamanıza da yardımcı olabilir.  

## <a name="data-usage"></a>Veri kullanımı

Sanal M'ler için Azure Monitörü dağıttığınızda, SANAL'larınız tarafından toplanan veriler yutularak Azure Monitor'da depolanır. Toplanan performans ve bağımlılık verileri Log Analytics çalışma alanında depolanır. Azure Monitor fiyatlandırma sayfasında yayınlanan [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/monitor/)bağlı olarak, VM'ler için Azure Monitör'ün faturası aşağıdakiler için faturalandırılır:

- Yutulan ve depolanan veriler.
- Oluşturulan uyarı kuralları.
- Gönderilen bildirimler. 

Günlük boyutu performans sayaçlarının dize uzunluklarına göre değişir ve VM'ye ayrılan mantıksal diskve ağ bağdaştırıcılarının sayısıyla artabilir. Zaten bir çalışma alanınız varsa ve bu sayaçları topluyorsanız, yinelenen ücretuygulanmaz. Hizmet Haritası'nı zaten kullanıyorsanız, göreceğiniz tek değişiklik Azure Monitor'a gönderilen ek bağlantı verileridir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizi izlemenize yardımcı olan gereksinimleri ve yöntemleri anlamak [için, SANAL'lar için Azure Monitörünü Dağıt'ı gözden geçirin.](vminsights-enable-overview.md)
