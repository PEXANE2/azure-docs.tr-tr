---
title: SAP çözümlerine genel bakış ve mimari için Azure Izleyici | Microsoft Docs
description: Bu makale, SAP Çözümleri için Azure izleyici hakkında sık sorulan soruların yanıtlarını sağlar
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/30/2020
ms.author: radeltch
ms.openlocfilehash: fc4e8766f77a41a3a53e3db3098a6d9cefd628d6
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964235"
---
# <a name="azure-monitor-for-sap-solutions-preview"></a>SAP Çözümleri için Azure izleyici (Önizleme)

## <a name="overview"></a>Genel Bakış  

SAP Çözümleri için Azure Izleyici, Azure 'da SAP 'lerini çalıştıran, müşteriler için Azure Native bir izleme ürünüdür. Ürün [Azure sanal makinelerinde hem SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-get-started) hem de [Azure büyük örneklerde SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)ile birlikte kullanılabilir.  
SAP Çözümleri için Azure Izleyici ile müşteriler, Azure altyapısından ve veritabanlarından tek bir merkezi konumda telemetri verileri toplayabilir ve daha hızlı sorun giderme için telemetri verilerini görsel olarak ilişkilendirebilir.  

SAP Çözümleri için Azure Izleyici, Azure Marketi aracılığıyla sunulur. Basit, sezgisel bir kurulum deneyimi sağlar ve yalnızca birkaç tıklamayla yararlanarak SAP için Azure Izleyici çözümleri için kaynak ( **SAP Monitor kaynağı**olarak bilinir) dağıtımı gerçekleştirir.  

Müşteriler, bu bileşene karşılık gelen **sağlayıcıyı** ekleyerek Azure sanal makineleri, yüksek kullanılabilirlik kümesi, SAP HANA veritabanı vb. gibi SAP yatay 'ın farklı bileşenlerini izleyebilir. 

Desteklenen altyapı:  

- Azure Sanal Makinesi  
- Azure büyük örnek  

Desteklenen veritabanları:  
- SAP HANA Veritabanı 
- Microsoft SQL Server  

SAP Çözümleri için Azure Izleyici, ek izleme özellikleri sağlamak üzere Log Analytics ve [çalışma kitapları](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview) gibi mevcut [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) yeteneklerinin gücünden yararlanır. Müşteriler, SAP Çözümleri için Azure Izleyici tarafından sunulan varsayılan çalışma kitaplarını düzenleyerek özel bir [görselleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview#getting-started) oluşturabilir, Azure Log Analytics çalışma alanı kullanarak [özel sorgular](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) yazabilir ve [özel uyarılar](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response) oluşturabilir, [Esnek saklama süresinden](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) yararlanır ve izleme verilerini bilet oluşturma sistemiyle birbirine bağlayabilirler.

## <a name="what-data-does-azure-monitor-for-sap-solutions-collect"></a>SAP Çözümleri için Azure Izleyici hangi verileri toplar?

SAP Çözümleri için Azure Izleyici 'de veri toplama, müşteriler tarafından yapılandırılan sağlayıcılara bağlıdır. Genel Önizleme sırasında aşağıdaki veriler toplanmaktadır.  

Yüksek kullanılabilirliğe sahip pacemaker kümesi telemetrisi:  
- Düğüm, kaynak ve SBD cihaz durumu  
- Pacemaker konum kısıtlamaları  
- Çekirdek oyları ve halka durumu  
- [Diğerleri](https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/doc/metrics.md)
  
SAP HANA telemetri:  
- CPU, bellek, disk ve ağ kullanımı  
- HANA sistem çoğaltması (HSR)  
- HANA yedekleme  
- HANA konak durumu  
- Dizin sunucusu ve ad sunucusu rolleri  

Microsoft SQL Server telemetri:  
- CPU, bellek, disk kullanımı  
- Ana bilgisayar adı, SQL örneği adı, SAP sistem KIMLIĞI  
- Zaman içinde toplu Istekler, derlemeler ve sayfa ömrü erkeklerin  
- Zaman içinde en çok pahalı 10 SQL deyimi  
- SAP sisteminde ilk 12 en büyük tablo  
- SQL Server hata günlüklerine kaydedilen sorunlar  
- Zaman içinde işlem ve SQL bekleme Istatistiklerini engelleme  

## <a name="data-sharing-with-microsoft"></a>Microsoft ile veri paylaşımı

SAP Çözümleri için Azure Izleyici, Azure müşterilerine yönelik SAP 'imiz için geliştirilmiş destek sağlamak üzere sistem meta verilerini toplar. PII/EUıı toplanmadı.  
Müşteriler, açılan listeden *Paylaşım* ' i seçerek SAP Çözümleri Için Azure izleyici kaynağı oluşturma sırasında Microsoft ile veri paylaşımını etkinleştirebilir.  
Müşterilerin, Microsoft destek ve mühendislik ekiplerine müşteri ortamı hakkında daha fazla bilgi verdiği ve Azure müşterilerine yönelik görev açısından kritik SAP 'imize yönelik gelişmiş destek sağladığından veri paylaşımını etkinleştirmeleri önemle önerilir.  

## <a name="architecture-overview"></a>Mimariye genel bakış

Yüksek düzeyde, aşağıdaki diyagramda SAP çözümlerinin Azure Izleyicisinin SAP HANA veritabanından telemetri nasıl topladığı açıklanmaktadır. Mimari, SAP HANA Azure sanal makinelerinde veya Azure büyük örneklerinde dağıtılmış olup olmadığı konusunda belirsiz değildir.

![SAP Çözümleri için Azure Izleyici mimarisi](./media/azure-monitor-sap/azure-monitor-architecture.png)

Mimarinin temel bileşenleri şunlardır:   
- Azure portal: müşteriler için başlangıç noktası. Müşteriler, Azure portal dahilinde Market 'e gidebilir ve SAP Çözümleri için Azure Izleyicisini bulabilir
- SAP Çözümleri için Azure Izleyici kaynak: müşterilerin izleme telemetrisini görüntülemesi için bir giriş yeri 
- Yönetilen kaynak grubu – SAP Çözümleri için Azure Izleyici Kaynak dağıtımının bir parçası olarak otomatik olarak dağıtılır. Yönetilen kaynak grubu içinde dağıtılan kaynaklar telemetri topluluğunda yardım eder. Dağıtılan anahtar kaynakları ve amaçları şunlardır:  
   - Azure sanal makinesi: *TOPLAYıCı VM*olarak da bilinir. Bu bir Standard_B2ms VM 'dir. Bu sanal makinenin ana amacı, *Izleme yükünü*barındırmak için kullanılır. İzleme yükü, kaynak sistemlerden telemetri toplama ve toplanan verileri izleme çerçevesine aktarma mantığını ifade eder. Yukarıdaki diyagramda, izleme yükü SQL bağlantı noktası üzerinden SAP HANA veritabanına bağlanma mantığını içerir.
   - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/basic-concepts): Bu kaynak, SAP HANA veritabanı kimlik bilgilerini güvenli bir şekilde tutmak ve [sağlayıcılar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/azure-monitor-providers)hakkındaki bilgileri depolamak için dağıtılır.  
   - Log Analytics çalışma alanı: Telemetri verilerinin bulunduğu hedef.  
      - Görselleştirme, [Azure çalışma kitaplarını](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview)kullanarak Log Analytics telemetri üzerine kurulmuştur. Müşteriler görselleştirmeyi özelleştirebilir. Müşteriler, çalışma kitaplarını veya çalışma kitapları içindeki belirli görselleştirmeleri, en düşük ayrıntı düzeyi olan 30 dakikalık bir şekilde Azure panosuna de sabitleyebilir.  
      - Müşteriler, dağıtım sırasında bu seçeneği belirleyerek, mevcut çalışma alanını SAP Monitor kaynağı ile aynı abonelikte kullanabilir. 
      - Müşteriler, Log Analytics çalışma alanındaki ham tablolara karşı [sorgu](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) çalıştırmak için kusto sorgu DILI (KQL) kullanabilir. *Özel günlüklere*bakın.  

> [!Note]
> Müşteriler, yönetilen kaynak grubunda dağıtılan VM 'nin düzeltme eki uygulama ve bakım yapmaktan sorumludur.  

> [!Tip]
> Müşteriler, SAP Çözümleri için Azure Izleyici için kaynak olarak aynı Azure aboneliğine dağıtılmışsa, telemetri toplama için mevcut bir Log Analytics çalışma alanını kullanmayı tercih edebilir.

### <a name="architecture-highlights"></a>Mimari vurgulamalar

Mimarinin başlıca vurgulamaları aşağıda verilmiştir:
 - **Çok örnekli** -MÜŞTERILER, SAP Çözümleri için tek bir Azure izleyici kaynağına sahıp bir sanal ağ içindeki bırden fazla sap SID arasında belirli bir bileşen türünün (ÖRNEĞIN, Hana DB, ha kümesi, Microsoft SQL Server) birden çok örneği için izleyici oluşturabilir. 
 - **Multi-Provider** -yukarıdaki mimari diyagramda, örnek olarak SAP HANA sağlayıcısı gösterilmektedir. Benzer şekilde, müşteriler ilgili bileşenler için ek sağlayıcılar (örneğin, HANA DB, HA kümesi, Microsoft SQL Server), bu bileşenlerden veri toplamak için yapılandırabilir.
 - **Açık kaynak** -SAP Için Azure izleyici çözümlerinin kaynak kodu [GitHub](https://github.com/Azure/AzureMonitorForSAPSolutions)' da bulunabilir. Müşteriler sağlayıcı koduna başvurabilir ve ürün, katkıda bulunma veya paylaşma hakkında daha fazla bilgi edinebilirsiniz. 
 - **Genişletilebilir sorgu çerçevesi** -telemetri verilerini toplamak için SQL sorguları [JSON](https://github.com/Azure/AzureMonitorForSAPSolutions/blob/master/sapmon/content/SapHana.json)dilinde yazılır. Daha fazla telemetri verisi toplamak için ek SQL sorguları kolayca eklenebilir. Müşteriler, bu belgenin sonundaki bağlantı aracılığıyla geri bildirim bırakarak veya hesap ekibine başvurarak, SAP Çözümleri için Azure Izleyici 'ye belirli telemetri verileri eklemek isteyebilir.

## <a name="pricing"></a>Fiyatlandırma
SAP Çözümleri için Azure Izleyici, ücretsiz bir üründür (lisans ücreti yoktur). Müşteriler, yönetilen kaynak grubundaki temel bileşenler için maliyeti ödemekten sorumludur.

## <a name="next-steps"></a>Sonraki adımlar

Sağlayıcılar hakkında bilgi edinin ve SAP Çözümleri için ilk Azure Izleyici kaynağını oluşturun.
 - [Sağlayıcılar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/azure-monitor-providers) hakkında daha fazla bilgi edinin
 - SAP Çözümleri için Azure Izleyici hakkında sorularınız mı var? [SSS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/azure-monitor-faq) bölümüne bakın
