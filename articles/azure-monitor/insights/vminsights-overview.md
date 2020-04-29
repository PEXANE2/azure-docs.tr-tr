---
title: VM'ler için Azure İzleyici nedir?
description: Uygulama bileşenlerini ve bağımlılıklarını otomatik olarak bulmaya ve eşleştirmeye ek olarak Azure VM 'lerinin sistem durumunu ve performansını izleyen VM'ler için Azure İzleyici genel bakış.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/11/2020
ms.openlocfilehash: 6f65ba96f768a4b88aa13c073cc66920b8c5e049
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79480496"
---
# <a name="what-is-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici nedir?

VM'ler için Azure İzleyici, Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerinizi ölçeklendirerek izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder. Şirket içinde veya başka bir bulut sağlayıcısında barındırılan VM 'Ler için performans ve uygulama bağımlılıklarını izleme desteği içerir. Aşağıdaki temel özellikler ayrıntılı öngörüler sunar:

- **Önceden tanımlanmış popüler performans grafikleri**: Konuk VM işletim sisteminden çekirdek performans ölçümlerini görüntüleyin.

- **Bağımlılık eşlemesi**: çeşitli kaynak gruplarından ve ABONELIKLERDEN VM ile bağlantılı bileşenleri görüntüler.  

>[!NOTE]
>Kısa süre önce, genel önizleme müşterilerimizden aldığımız geri bildirimlere göre sistem durumu özelliğine yaptığımız [değişiklikleri duyuruyoruz](https://azure.microsoft.com/updates/updates-to-azure-monitor-for-virtual-machines-preview-before-general-availability-release/
) . Yaptığımız değişikliklerin sayısı verildiğinde, yeni müşterilerin sistem durumu özelliğini sunmayı durduracağız. Mevcut müşteriler sistem durumu özelliğini kullanmaya devam edebilir. Daha fazla ayrıntı için lütfen [genel kullanım hakkında SSS bölümüne](vminsights-ga-release-faq.md)bakın.  

Azure Izleyici günlükleriyle tümleştirme, güçlü toplama ve filtreleme olanağı sunarak, zaman içinde veri eğilimlerini analiz VM'ler için Azure İzleyici sağlar. Bu verileri doğrudan sanal makineden tek bir VM 'de görüntüleyebilir veya Azure Izleyici 'yi, görünümün Azure Kaynak bağlamı veya çalışma alanı-bağlam modlarını desteklediği, VM 'nizin toplanmış bir görünümünü sunmak için kullanabilirsiniz. Daha fazla bilgi için bkz. [erişim modlarına genel bakış](../platform/design-logs-deployment.md#access-mode).

![Azure portal sanal makine öngörüleri perspektifi](media/vminsights-overview/vminsights-azmon-directvm.png)

VM'ler için Azure İzleyici, önemli uygulamaların tahmin edilebilir performansını ve kullanılabilirliğini sunabilir. Performans sorunlarını ve ağ sorunlarını belirler ve bir sorunun diğer bağımlılıklarla ilgili olup olmadığını anlamanıza da yardımcı olabilir.  

## <a name="data-usage"></a>Veri kullanımı

VM'ler için Azure İzleyici dağıttığınızda, VM 'leriniz tarafından toplanan veriler Azure Izleyici 'de alınır ve depolanır. Toplanan performans ve bağımlılık verileri bir Log Analytics çalışma alanında depolanır. [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)yayınlanan fiyatlandırmaya göre, VM'ler için Azure izleyici için faturalandırılır:

- Alınan ve depolanan veriler.
- Oluşturulan uyarı kuralları.
- Gönderilen bildirimler. 

Günlük boyutu performans sayaçlarının dize uzunluklarına göre farklılık gösterir ve VM 'ye ayrılan mantıksal disk ve ağ bağdaştırıcısı sayısıyla artabilir. Zaten bir çalışma alanınız varsa ve bu sayaçları topluyorsanız, hiçbir yinelenen ücret uygulanmaz. Zaten Hizmet Eşlemesi kullanıyorsanız, göreceğiniz tek değişiklik Azure Izleyici 'ye gönderilen ek bağlantı vereceğiz.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizi izlemenize yardımcı olan gereksinimleri ve yöntemleri anlamak için [VM'ler için Azure izleyici dağıt](vminsights-enable-overview.md)' ı inceleyin.
