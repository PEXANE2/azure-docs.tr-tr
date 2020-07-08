---
title: SSS-SAP Çözümleri için Azure Izleyicisi | Microsoft Docs
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
ms.openlocfilehash: cf0366300c4fab18a0f6231a97ca050eddd50132
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852435"
---
# <a name="azure-monitor-for-sap-solutions-faq-preview"></a>SAP Çözümleri için Azure izleyici SSS (Önizleme)
## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu makalede, SAP Çözümleri için Azure izleyici hakkında sık sorulan soruların (SSS) yanıtları sağlanmaktadır.  

 - **SAP Çözümleri için Azure Izleyici için ödeme yapmam gerekir mi?**  
SAP Çözümleri için Azure Izleyici için lisans ücreti yoktur.  
Ancak, müşteriler yönetilen kaynak grubu bileşenlerinin maliyetini sağlamaktan sorumludur.  

 - **Bu hizmet genel önizleme için hangi bölgelerde kullanılabilir?**  
Bu hizmet, genel önizleme için Doğu ABD 2, Batı ABD 2, Doğu ABD ve Batı Avrupa ' de kullanıma sunulacaktır.  

 - **Aboneliğimde yönetilen kaynak grubunun dağıtımına izin vermek için izinler sağlamam gerekir mi?**  
Hayır, Açık izinler gerekli değildir.  

 - **Toplayıcı VM nerede bulunur?**  
SAP Çözümleri için Azure Izleyici 'yi dağıtma sırasında, müşterilerin kaynak izlemek için aynı VNET 'i SAP HANA sunucusu olarak seçmesini öneririz. Bu nedenle, toplayıcı VM 'nin SAP HANA sunucusuyla aynı VNET 'te bulunması önerilir. Müşteriler HANA olmayan veritabanını kullanıyorsa, toplayıcı VM, HANA olmayan veritabanı ile aynı VNET 'te yer alır.  

 - **HANA 'nın hangi sürümleri destekleniyor?**  
HANA 1,0 SPS 12 (Rev. 120 veya üzeri) ve HANA 2,0 SPS03 veya üzeri  

 - **Hangi HANA dağıtım yapılandırması desteklenir?**  
Aşağıdaki yapılandırmalar desteklenir:
   - Tek düğümlü (ölçek genişletme) ve çok düğümlü (genişleme)  
   - Tek veritabanı kapsayıcısı (HANA 1,0 SPS 12) ve birden çok veritabanı kapsayıcıları (HANA 1,0 SPS 12 veya HANA 2,0)  
   - Otomatik ana bilgisayar yük devretmesi (n + 1) ve HSR  

 - **Hangi SQL Server sürümleri desteklenir?**  
SQL Server 2012 SP4 veya üzeri.  

 - **Hangi SQL Server konfigürasyonları destekleniyor?**  
Aşağıdaki yapılandırmalar desteklenir:
   - Bir sanal makinedeki varsayılan veya adlandırılmış tek başına örnekleri  
   - Kümelenmiş kaynağın veya AlwaysOn dinleyicisi adının sanal adı kullanılırken bir AlwaysOn yapılandırmasındaki kümelenmiş örnekler veya örnekler. Şu anda hiçbir küme veya AlwaysOn özel ölçümü toplanmadı    
   - Azure SQL veritabanı (PAAS) Şu anda desteklenmiyor  

 - **Yönetilen kaynak grubunu yanlışlıkla silersem ne olur?**  
Yönetilen kaynak grubu varsayılan olarak kilitlidir. Bu nedenle, yönetilen kaynak grubunun müşteriler tarafından yanlışlıkla silinme olasılığı minuscule ' dir.  
Bir müşteri yönetilen kaynak grubunu silerse, SAP Çözümleri için Azure Izleyici çalışmayı durdurur. Müşterinin, SAP Çözümleri için yeni bir Azure Izleyici kaynağı dağıtması ve baştan başlaması gerekecektir.  

 - **Azure Aboneliğimde SAP Çözümleri kaynağı için Azure Izleyicisini dağıtmak üzere hangi rollere ihtiyacım var?**  
Katkıda bulunan rolü.  

 - **Bu ürünün SLA 'Sı nedir?**  
Önizlemeler hizmet düzeyi sözleşmelerinden çıkarılır. Lütfen SAP Solutions Market görüntüsü için Azure Izleyici aracılığıyla tam lisans terimini okuyun.  

 - **Bu çözüm aracılığıyla tüm Yatayı izleyebilir miyim?**  
Şu anda HANA veritabanını, temel altyapıyı, yüksek kullanılabilirlik kümesini ve Microsoft SQL Server 'ı genel önizlemede izleyebilirsiniz.  

 - **Bu hizmet SAP Solution Manager 'ın yerini mi?**  
Hayır. Müşteriler, Iş süreci izleme için SAP Solution Manager 'ı kullanmaya devam edebilir.  

 - **SAP HANA kokpiti/Studio gibi geleneksel çözümler üzerinden bu hizmetin değeri nedir?**  
SAP Çözümleri için Azure Izleyici, HANA veritabanına özgü değildir. SAP Çözümleri için Azure Izleyici de AnyDB 'yi destekler.  

## <a name="next-steps"></a>Sonraki adımlar

- SAP Çözümleri için ilk Azure Izleyici kaynağını oluşturun.