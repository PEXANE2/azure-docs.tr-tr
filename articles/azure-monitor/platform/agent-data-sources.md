---
title: Azure Monitör'de aracı veri kaynaklarını yapılandırma | Microsoft Dokümanlar
description: Veri kaynakları, Azure Monitor'un aracılardan ve diğer bağlı kaynaklardan topladığı günlük verilerini tanımlar.  Bu makalede, Azure Monitor'un veri kaynaklarını nasıl kullandığı, bunları nasıl yapılandırılacaklarına ilişkin ayrıntıları açıkladığı ve mevcut farklı veri kaynaklarının bir özetini sağladığı kavramı açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: aec3fe2386ce916c556f6da295a8554fff140259
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249106"
---
# <a name="agent-data-sources-in-azure-monitor"></a>Azure Monitor'da veri kaynağı aracısı
Azure Monitor'un aracılardan topladığı veriler, yapılandırdığınız veri kaynakları tarafından tanımlanır.  Aracılardan gelen veriler, bir kayıt kümesiyle [günlük verileri](data-platform-logs.md) olarak depolanır.  Her veri kaynağı, her türkendi özellikleri kümesi ne sahip belirli bir tür kayıtları oluşturur.

![Günlük veri toplama](media/agent-data-sources/overview.png)

## <a name="summary-of-data-sources"></a>Veri kaynaklarının özeti
Aşağıdaki tabloda, şu anda Azure Monitor'da bulunan aracı veri kaynakları listelenmektedir.  Her biri, bu veri kaynağı için ayrıntı sağlayan ayrı bir makaleye bağlantı vardır.   Ayrıca, metodları ve toplama sıklığı hakkında da bilgi sağlar. 


| Veri kaynağı | Platform | Günlük analiz aracısı | Operations Manager aracısı | Azure depolama alanı | Operasyon Yöneticisi gerekli mi? | Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Özel günlükler](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | varışta |
| [Özel günlükler](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | varışta |
| [IIS günlükleri](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |Log File Rollover ayarına bağlıdır |
| [Performans sayaçları](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |planlandığı gibi, en az 10 saniye |
| [Performans sayaçları](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |planlandığı gibi, en az 10 saniye |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |Azure depolamadan: 10 dakika; ajandan: varışta |
| [Windows Olay günlükleri](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | varışta |


## <a name="configuring-data-sources"></a>Veri kaynaklarını yapılandırma
Çalışma alanı için **Gelişmiş Ayarlar'daki** **Veri** menüsünden veri kaynaklarını yapılandırırsınız.  Herhangi bir yapılandırma çalışma alanınızdaki tüm bağlı kaynaklara teslim edilir.  Şu anda bu yapılandırmadan herhangi bir aracıyı hariç tutamazsınız.

![Windows olaylarını yapılandırma](media/agent-data-sources/configure-events.png)

1. Azure portalında, Çalışma alanınız > **Gelişmiş Ayarlar'>** **Günlük Analizi çalışma alanlarını** seçin.
2. **Veri'yi**seçin.
3. Yapılandırmak istediğiniz veri kaynağını tıklatın.
4. Yapılandırmaları hakkında ayrıntılar için yukarıdaki tablodaki her veri kaynağıiçin belgelerin bağlantısını izleyin.


## <a name="data-collection"></a>Veri toplama
Veri kaynağı yapılandırmaları, birkaç dakika içinde Azure Monitor'a doğrudan bağlanan aracılara teslim edilir.  Belirtilen veriler aracıdan toplanır ve her veri kaynağına özgü aralıklarla doğrudan Azure Monitor'a teslim edilir.  Bu ayrıntılar için her veri kaynağıiçin belgelere bakın.

Bağlı bir yönetim grubundaki Sistem Merkezi Operasyon Yöneticisi aracıları için, veri kaynağı yapılandırmaları yönetim paketlerine dönüştürülr ve varsayılan olarak her 5 dakikada bir yönetim grubuna teslim edilir.  Aracı, yönetim paketini diğerleri gibi indirir ve belirtilen verileri toplar. Veri kaynağına bağlı olarak, veriler verileri Azure Monitör'e ileten bir yönetim sunucusuna gönderilir veya aracı, yönetim sunucusundan geçmeden verileri Azure Monitor'a gönderir. Ayrıntılar [için Azure'daki çözümleri izlemek için Veri toplama ayrıntılarına](../insights/solutions-inventory.md) bakın.  Operations Manager ve Azure Monitor'u bağlama ve yapılandırmanın teslim edilen sıklığını sistem [merkezi operasyon yöneticisiyle Yapılandırma Yapılandırma'da](om-agents.md)değiştirme ayrıntılarını okuyabilirsiniz.

Aracı Azure Monitor veya Operations Manager'a bağlanamıyorsa, bağlantı kurduğunda sunacağı verileri toplamaya devam eder.  Veri miktarı istemci için maksimum önbellek boyutuna ulaşırsa veya aracı 24 saat içinde bağlantı kuramazsa veriler kaybedilebilir.

## <a name="log-records"></a>Günlük kayıtları
Azure Monitor tarafından toplanan tüm günlük verileri çalışma alanında kayıt olarak depolanır.  Farklı veri kaynakları tarafından toplanan kayıtların kendi özellikleri kümesi olur ve **Type** özelliklerine göre tanımlanır.  Her kayıt türüyle ilgili ayrıntılar için her veri kaynağı ve çözüm için belgelere bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Azure [Monitor'a](../insights/solutions.md) işlevsellik katan ve aynı zamanda çalışma alanına veri toplayan izleme çözümleri hakkında bilgi edinin.
* Veri kaynaklarından ve izleme çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  
* Veri kaynaklarından ve izleme çözümlerinden toplanan kritik verileri proaktif olarak bildirmek için [uyarıları](alerts-overview.md) yapılandırın.
