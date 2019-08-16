---
title: Azure Güvenlik Merkezi tarafından desteklenen özellikler ve platformlar | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi tarafından desteklenen özelliklerin ve platformların bir listesini sağlar.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/03/2019
ms.author: v-mohabe
ms.openlocfilehash: 9a11af7e2875c9af5cf4b08d459bc67b55dbdcf3
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515542"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Azure Güvenlik Merkezi tarafından desteklenen platformlar ve Özellikler

Hem klasik hem de Kaynak Yöneticisi dağıtım modelleri ve Bilgisayarları kullanılarak oluşturulan sanal makineler (VM) için güvenlik durumu izleme ve öneriler mevcuttur.

> [!NOTE]
> Azure kaynakları için [Klasik ve Kaynak Yöneticisi dağıtım modelleri](../azure-classic-rm.md) hakkında daha fazla bilgi edinin.
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Veri toplama aracısını destekleyen platformlar 

Bu bölümde, Azure Güvenlik Merkezi aracısının çalışacağı ve veri toplayabileceği platformlar listelenmektedir.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Windows bilgisayarları ve VM 'Ler için desteklenen platformlar
Aşağıdaki Windows işletim sistemleri desteklenir:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

> [!NOTE]
> Windows Defender ATP ile tümleştirme yalnızca Windows Server 2012 R2 ve Windows Server 2016 ' i destekler.

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Linux bilgisayarlar ve VM 'Ler için desteklenen platformlar

Aşağıdaki Linux işletim sistemleri desteklenir:

> [!NOTE]
> Desteklenen Linux işletim sistemlerinin listesi sürekli olarak değiştiğinden, tercih ediyorsanız, Desteklenen sürümlerin en güncel listesini görüntülemek için [buraya](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems) tıklayın ve bu konunun son yayımlandığı tarihten sonra değişiklikler olması gerekir.

64 bit
* CentOS 6 ve 7
* Amazon Linux 2017,09
* Oracle Linux 6 ve 7
* Red Hat Enterprise Linux Server 6 ve 7
* Borçlu GNU/Linux 8 ve 9
* Ubuntu Linux 14,04 LTS, 16,04 LTS ve 18,04 LTS
* SUSE Linux Enterprise Server 12

32 bit
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Borçlu GNU/Linux 8 ve 9
* Ubuntu Linux 14,04 LTS ve 16,04 LTS

## <a name="vms-and-cloud-services"></a>VM 'Ler ve Cloud Services
Bulut hizmetinde çalışan VM 'Ler de desteklenir. Yalnızca üretim yuvalarında çalışan Cloud Services Web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi edinmek için bkz. [Azure Cloud Services 'ye genel bakış](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Desteklenen IaaS özellikleri

> [!div class="mx-tableFixed"]
> 

|Sunucusu|Windows|||Linux|||Fiyatlandırma|
|----|----|----|----|----|----|----|----|
|**Ortam**|**Azure**||**Azure dışı**|**Azure**||**Azure dışı**||
||**Sanal Makine**|**Sanal makine ölçek kümesi**||**Sanal Makine**|**Sanal makine ölçek kümesi**|
|VMBA tehdit algılama uyarıları|✔|✔|✔|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|Öneriler (ücretsiz) tehdit algılama (Standart)|
|Ağ tabanlı tehdit algılama uyarıları|✔|✔|X|✔|✔|X|Standart|
|Windows Defender ATP tümleştirme|✔ (desteklenen sürümlerde)|✔ (desteklenen sürümlerde)|✔|X|X|X|Standart|
|Eksik düzeltme ekleri|✔|✔|✔|✔|✔|✔|Boş|
|Güvenlik yapılandırmaları|✔|✔|✔|✔|✔|✔|Boş|
|Endpoint Protection değerlendirmesi|✔|✔|✔|X|X|X|Boş|
|JIT VM erişimi|✔|X|X|✔|X|X|Standart|
|Uyarlamalı uygulama denetimleri|✔|X|✔|✔|X|✔|Standart|
|FIM|✔|✔|✔|✔|✔|✔|Standart|
|Disk şifrelemesi değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Üçüncü taraf dağıtımı|✔|X|X|✔|X|X|Boş|
|NSG değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Dosya daha az tehdit algılama|✔|✔|✔|X|X|X|Standart|
|Ağ haritası|✔|✔|X|✔|✔|X|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|X|✔|✔|X|Standart|
|Mevzuat uyumluluk panosu & raporları|✔|✔|✔|✔|✔|✔|Standart|
|Docker tarafından barındırılan IaaS kapsayıcılarında öneriler ve tehdit algılama|X|X|X|✔|✔|✔|Standart|

### <a name="supported-endpoint-protection-solutions"></a>Desteklenen Endpoint Protection çözümleri

Aşağıdaki tabloda bir matrisi verilmiştir:
 - Her çözümü sizin için yüklemek üzere Azure Güvenlik Merkezi 'ni kullanıp kullanmayacağınızı belirtir.
 - Güvenlik Merkezi 'nin hangi Endpoint Protection çözümlerini keşfedebileceği. Bu uç nokta koruma çözümlerinden biri bulunursa, güvenlik merkezi bir tane yüklemeyi önermez.

Bu korumaların her biri için önerilerin ne zaman oluşturulduğu hakkında bilgi için bkz. [Endpoint Protection değerlendirme ve öneriler](security-center-endpoint-protection.md).

| Endpoint Protection| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)| Windows Server 2016| Hayır, işletim sisteminde yerleşik| Evet |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Evet |
| Trend Micro – Tüm sürümler | Windows Server Ailesi  | Hayır | Evet |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Linux sunucu ailesi  | Hayır | Yes **\*** |
| Sophos v9 +| Linux sunucu ailesi  | Hayır | Yes **\***  |

 **\*** Kapsam durumu ve destekleyici veriler şu anda yalnızca korunan aboneliklerinizle ilişkili Log Analytics çalışma alanında kullanılabilir ve Azure Güvenlik Merkezi portalında yansıtılmamaktadır.

> [!NOTE]
> - Windows Server 2008 R2 sanal makinesi üzerinde System Center Endpoint Protection (SCEP) algılama, PowerShell 3,0 (veya bir üst sürüm) sonrasında SCEP 'in yüklenmesini gerektirir.

## <a name="supported-paas-features"></a>Desteklenen PaaS özellikleri


|Hizmet|Öneriler (ücretsiz)|Tehdit algılama (Standart)|
|----|----|----|
|SQL|✔| ✔|
|PostgreSQL|✔| ✔|
|MySQL|✔| ✔|
|Azure Blob depolama hesapları|✔| ✔|
|Uygulama hizmetleri|✔| ✔|
|Cloud Services|✔| X|
|Sanal Ağlar|✔| NA|
|Alt ağlar|✔| NA|
|NIC’ler|✔| NA|
|NSG|✔| NA|
|Subscription|✔ * *| ✔|
|Batch|✔| NA|
|Service Fabric|✔| NA|
|Otomasyon hesabı|✔| NA|
|Yük dengeleyici|✔| NA|
|Ara|✔| NA|
|Service bus|✔| NA|
|Stream Analytics|✔| NA|
|Olay hub'ı|✔| NA|
|Logic Apps|✔| NA|
|Depolama hesabı|✔| NA|
|Redis|✔| NA|
|Data Lake Analytics|✔| NA|
|Anahtar kasası|✔| NA|




\*Bu özellikler Şu anda genel önizlemede desteklenmektedir.

\*\*AAD önerileri yalnızca standart abonelikler için kullanılabilir



## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi 'ni benimsemek için tasarım konularını nasıl planlayacağınızı ve anlayacağınızı](security-center-planning-and-operations-guide.md)öğrenin.
- [Azure Güvenlik Merkezi 'Nde VM 'ler & Sunucular Için tehdit algılama](security-center-alerts-iaas.md)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Merkezi 'ni kullanma hakkında sık sorulan soruları](security-center-faq.md)bulun.
- [Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini](https://blogs.msdn.com/b/azuresecurity/)bulun.
