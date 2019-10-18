---
title: VM'ler için Azure İzleyici (Önizleme) nedir? | Microsoft Docs
description: VM'ler için Azure İzleyici Azure Izleyici 'nin, Azure VM işletim sisteminin sistem durumunu ve performans izlemesini birleştiren ve uygulama bileşenlerini ve bağımlılıklarını diğer kaynaklarla otomatik olarak keşfederek ve iletişimi eşleyen bir özelliktir. aralarında. Bu makalede genel bakış sunulmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2019
ms.author: magoedte
ms.openlocfilehash: 3569247ad560f985c041eeb11a7d274e15bfeb4c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515378"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>VM'ler için Azure İzleyici (Önizleme) nedir?

VM'ler için Azure İzleyici, Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerinizi ölçeklendirerek izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder. 

Şirket içinde veya başka bir bulut sağlayıcısında barındırılan VM 'Ler için performans ve uygulama bağımlılıklarını izleme desteği içerir. Üç temel özellik ayrıntılı öngörüler sunar:

- **Azure VM 'Lerinin mantıksal bileşenleri ve Windows ve Linux çalıştıran sanal makine ölçek kümeleri**: önceden yapılandırılmış durum ölçütlerine göre ölçülür ve değerlendirilen koşul karşılandığında sizi uyarır.  

- **Önceden tanımlanmış popüler performans grafikleri**: Konuk VM işletim sisteminden çekirdek performans ölçümlerini görüntüleyin.

- **Bağımlılık eşlemesi**: çeşitli kaynak gruplarından ve ABONELIKLERDEN VM ile bağlantılı bileşenleri görüntüler.  

Özellikler üç perspektifle düzenlenmiştir:

- Sağlık
- Performans
- Eşleme

>[!NOTE]
>Kısa süre önce, genel önizleme müşterilerimizden aldığımız geri bildirimlere göre sistem durumu özelliğinde değişiklikler [yaptığımız için](https://azure.microsoft.com/updates/upcoming-changes-for-azure-monitor-for-vms-as-we-prepare-for-ga) değişiklikler yapıyoruz. Yaptığımız değişikliklerin sayısı verildiğinde, yeni müşterilerin sistem durumu özelliğini sunmayı durduracağız. Mevcut müşteriler sistem durumu özelliğini kullanmaya devam edebilir. Daha fazla ayrıntı için lütfen [genel kullanım hakkında SSS bölümüne](vminsights-ga-release-faq.md)bakın.  

Azure Izleyici günlükleri ile tümleştirme, güçlü toplama ve filtreleme sağlar ve zaman içinde veri eğilimlerini analiz edebilir. Bu kapsamlı iş yükü izleme, Azure Izleyici veya tek başına Hizmet Eşlemesi ile elde alınamaz.  

Bu verileri doğrudan sanal makineden tek bir VM 'de görüntüleyebilir veya Azure Izleyici 'yi kullanarak VM 'nizin toplanmış bir görünümünü sağlayabilirsiniz. Bu görünüm her bir özelliğin perspektifine dayalıdır:

- **Sistem durumu**: VM 'ler bir kaynak grubuyla ilgilidir.
- **Eşleme** ve **performans**: vm 'ler belirli bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırılır.

![Azure portal sanal makine öngörüleri perspektifi](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

VM'ler için Azure İzleyici, önemli uygulamaların tahmin edilebilir performansını ve kullanılabilirliğini sunabilir. Kritik işletim sistemi olaylarını, performans sorunlarını ve ağ sorunlarını belirler. VM'ler için Azure İzleyici, bir sorunun diğer bağımlılıklarla ilgili olup olmadığını anlamanıza da yardımcı olabilir.  

## <a name="data-usage"></a>Veri kullanımı

VM'ler için Azure İzleyici dağıttığınızda, VM 'leriniz tarafından toplanan veriler Azure Izleyici 'de alınır ve depolanır. Durum ölçütleri ölçümleri Azure Izleyici 'de bir zaman serisi veritabanında depolanır, performans ve toplanan bağımlılık verileri Log Analytics çalışma alanında depolanır. [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)yayınlanan fiyatlandırmaya göre, VM'ler için Azure izleyici için faturalandırılır:

- Alınan ve depolanan veriler.
- İzlenen durum ölçütü ölçüm zaman serisi sayısı.
- Oluşturulan uyarı kuralları.
- Gönderilen bildirimler. 

Günlük boyutu performans sayaçlarının dize uzunluklarına göre farklılık gösterir ve VM 'ye ayrılan mantıksal disk ve ağ bağdaştırıcısı sayısıyla artabilir. Zaten bir çalışma alanınız varsa ve bu sayaçları topluyorsanız, hiçbir yinelenen ücret uygulanmaz. Zaten Hizmet Eşlemesi kullanıyorsanız, göreceğiniz tek değişiklik Azure Izleyici 'ye gönderilen ek bağlantı vereceğiz.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinelerinizi izlemenize yardımcı olan gereksinimleri ve yöntemleri anlamak için [VM'ler için Azure izleyici dağıt](vminsights-enable-overview.md)' ı inceleyin.
