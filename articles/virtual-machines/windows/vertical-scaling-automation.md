---
title: Windows sanal makinelerini dikey olarak ölçeklendirmek için Azure Otomasyonu 'nu kullanma | Microsoft Docs
description: Azure Otomasyonu ile uyarıları izlemeye yanıt olarak bir Windows sanal makinesini dikey olarak ölçeklendirme
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5d255662f7db12537365f57eb71355ca2e11cc51
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947246"
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Azure Otomasyonu ile Windows VM 'lerini dikey olarak ölçeklendirme

Dikey ölçeklendirme, iş yüküne yanıt olarak bir makinenin kaynaklarını artırma veya azaltma işlemidir. Azure 'da bu, sanal makinenin boyutu değiştirilerek gerçekleştirilebilir. Bu, aşağıdaki senaryolarda yardımcı olabilir

* Sanal makine sıklıkla kullanılmıyorsa, aylık maliyetlerinizi azaltmak için onu daha küçük bir boyuta yeniden boyutlandırabilirsiniz
* Sanal makine yoğun bir yük görebilise kapasitesini artırmak için daha büyük bir boyuta yeniden boyutlandırılabilir

Bunu gerçekleştirmeye yönelik adımların ana hattı aşağıda verilmiştir

1. Sanal makinelerinize erişmek için Azure Otomasyonu kurma
2. Azure Otomasyonu dikey ölçek runbook 'larını aboneliğinize aktarın
3. Runbook 'na bir Web kancası ekleyin
4. Sanal makinenize bir uyarı ekleyin


## <a name="scale-limitations"></a>Ölçek sınırlamaları

İlk sanal makinenin boyutu nedeniyle, ölçeklendirilebilen Boyutlar, geçerli sanal makinedeki kümedeki diğer boyutların kullanılabilirliğine bağlı olarak, ' de sınırlı olabilir. Bu makalede kullanılan yayımlanan Otomasyon Runbook 'larında bu durumu ele aldık ve yalnızca aşağıdaki VM boyut çiftleri içinde ölçeklendirdik. Bu, Standard_D1v2 bir sanal makinenin aniden Standard_G5 'e kadar ölçeklendirilmesi veya Basic_A0 olarak ölçeklendirilmesi anlamına gelir. Ayrıca, kısıtlanmış sanal makine boyutları ölçeği artırma/azaltma desteklenmez. 

Aşağıdaki boyut çiftleri arasında ölçeklendirmeyi seçebilirsiniz:

* [A serisi](#a-series)
* [B serisi](#b-series)
* [D serisi](#d-series)
* [E Serisi](#e-series)
* [F serisi](#f-series)
* [G serisi](#g-series)
* [H serisi](#h-series)
* [L serisi](#l-series)
* [A serisi](#m-series)
* [N serisi](#n-series)

### <a name="a-series"></a>A Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Basic_A0 | Basic_A1 |
| Basic_A1 | Basic_A2 |
| Basic_A2 | Basic_A3 |
| Basic_A3 | Basic_A4 |
| Standard_A0 | Standard_A1 |
| Standard_A1 | Standard_A2 |
| Standard_A2 | Standard_A3 |
| Standard_A3 | Standard_A4 |
| Standard_A5 | Standard_A6 |
| Standard_A6 | Standard_A7 |
| Standard_A8 | Standard_A9 |
| Standard_A10 | Standard_A11 |
| Standard_A1_v2 | Standard_A2_v2 |
| Standard_A2_v2 | Standard_A4_v2 |
| Standard_A4_v2 | Standard_A8_v2 |
| Standard_A2m_v2 | Standard_A4m_v2 |
| Standard_A4m_v2 | Standard_A8m_v2 |

### <a name="b-series"></a>B Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_B1s | Standard_B2s |
| Standard_B1ms | Standard_B2ms |
| Standard_B2ms | Standard_B4ms |
| Standard_B4ms | Standard_B8ms |

### <a name="d-series"></a>D Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_D1 | Standard_D2 |
| Standard_D2 | Standard_D3 |
| Standard_D3 | Standard_D4 |
| Standard_D11 | Standard_D12 |
| Standard_D12 | Standard_D13 |
| Standard_D13 | Standard_D14 |
| Standard_DS1 | Standard_DS2 |
| Standard_DS2 | Standard_DS3 |
| Standard_DS3 | Standard_DS4 |
| Standard_DS11 | Standard_DS12 |
| Standard_DS12 | Standard_DS13 |
| Standard_DS13 | Standard_DS14 |
| Standard_D1_v2 | Standard_D2_v2 |
| Standard_D2_v2 | Standard_D3_v2 |
| Standard_D3_v2 | Standard_D4_v2 |
| Standard_D4_v2 | Standard_D5_v2 |
| Standard_D11_v2 | Standard_D12_v2 |
| Standard_D12_v2 | Standard_D13_v2 |
| Standard_D13_v2 | Standard_D14_v2 |
| Standard_DS1_v2 | Standard_DS2_v2 |
| Standard_DS2_v2 | Standard_DS3_v2 |
| Standard_DS3_v2 | Standard_DS4_v2 |
| Standard_DS4_v2 | Standard_DS5_v2 |
| Standard_DS11_v2 | Standard_DS12_v2 |
| Standard_DS12_v2 | Standard_DS13_v2 |
| Standard_DS13_v2 | Standard_DS14_v2 |
| Standard_D2_v3 | Standard_D4_v3 |
| Standard_D4_v3 | Standard_D8_v3 |
| Standard_D8_v3 | Standard_D16_v3 |
| Standard_D16_v3 | Standard_D32_v3 |
| Standard_D32_v3 | Standard_D64_v3 |
| Standard_D2s_v3 | Standard_D4s_v3 |
| Standard_D4s_v3 | Standard_D8s_v3 |
| Standard_D8s_v3 | Standard_D16s_v3 |
| Standard_D16s_v3 | Standard_D32s_v3 |
| Standard_D32s_v3 | Standard_D64s_v3 |
| Standard_DC2s | Standard_DC4s |

### <a name="e-series"></a>E Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_E2_v3 | Standard_E4_v3 |
| Standard_E4_v3 | Standard_E8_v3 |
| Standard_E8_v3 | Standard_E16_v3 |
| Standard_E16_v3 | Standard_E20_v3 |
| Standard_E20_v3 | Standard_E32_v3 |
| Standard_E32_v3 | Standard_E64_v3 |
| Standard_E2s_v3 | Standard_E4s_v3 |
| Standard_E4s_v3 | Standard_E8s_v3 |
| Standard_E8s_v3 | Standard_E16s_v3 |
| Standard_E16s_v3 | Standard_E20s_v3 |
| Standard_E20s_v3 | Standard_E32s_v3 |
| Standard_E32s_v3 | Standard_E64s_v3 |

### <a name="f-series"></a>F Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_F1 | Standard_F2 |
| Standard_F2 | Standard_F4 |
| Standard_F4 | Standard_F8 |
| Standard_F8 | Standard_F16 |
| Standard_F1s | Standard_F2s |
| Standard_F2s | Standard_F4s |
| Standard_F4s | Standard_F8s |
| Standard_F8s | Standard_F16s |
| Standard_F2s_v2 | Standard_F4s_v2 |
| Standard_F4s_v2 | Standard_F8s_v2 |
| Standard_F8s_v2 | Standard_F16s_v2 |
| Standard_F16s_v2 | Standard_F32s_v2 |
| Standard_F32s_v2 | Standard_F64s_v2 |
| Standard_F64s_v2 | Standard_F7s_v2 |

### <a name="g-series"></a>G Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_G1 | Standard_G2 |
| Standard_G2 | Standard_G3 |
| Standard_G3 | Standard_G4 |
| Standard_G4 | Standard_G5 |
| Standard_GS1 | Standard_GS2 |
| Standard_GS2 | Standard_GS3 |
| Standard_GS3 | Standard_GS4 |
| Standard_GS4 | Standard_GS5 |

### <a name="h-series"></a>H serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_H8 | Standard_H16 |
| Standard_H8m | Standard_H16m |

### <a name="l-series"></a>L Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standart_L4s | Standart_L8s |
| Standart_L8s | Standart_L16s |
| Standart_L16s | Standart_L32s |
| Standard_L8s_v2 | Standard_L16s_v2 |
| Standard_L16s_v2 | Standard_L32s_v2 |
| Standard_L32s_v2 | Standard_L64s_v2 |
| Standard_L64s_v2 | Standard_L80s_v2 |

### <a name="m-series"></a>M Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_M8ms | Standard_M16ms |
| Standard_M16ms | Standard_M32ms |
| Standard_M32ms | Standard_M64ms |
| Standard_M64ms | Standard_M128ms |
| Standard_M32ls | Standard_M64ls |
| Standard_M64s | Standard_M128s |
| Standard_M64 | Standard_M128 |
| Standard_M64m | Standard_M128m |

### <a name="n-series"></a>N Serisi

| Başlangıç boyutu | Ölçeği büyütme boyutu | 
| --- | --- |
| Standard_NC6 | Standard_NC12 |
| Standard_NC12 | Standard_NC24 |
| Standard_NC6s_v2 | Standard_NC12s_v2 |
| Standard_NC12s_v2 | Standard_NC24s_v2 |
| Standard_NC6s_v3 | Standard_NC12s_v3 |
| Standard_NC12s_v3 | Standard_NC24s_v3 |
| Standard_ND6 | Standard_ND12 |
| Standard_ND12 | Standard_ND24 |
| Standard_NV6 | Standard_NV12 |
| Standard_NV12 | Standard_NV24 |
| Standard_NV6s_v2 | Standard_NV12s_v2 |
| Standard_NV12s_v2 | Standard_NV24s_v2 |
| Standard_NV12s_v3 |Standard_NV48s_v3 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Sanal makinelerinize erişmek için Azure Otomasyonu kurma
Yapmanız gereken ilk şey, bir sanal makineyi ölçeklendirmek için kullanılan runbook 'ları barındıracak bir Azure Otomasyonu hesabı oluşturmaktır. Son olarak Otomasyon Hizmeti, Kullanıcı adına runbook 'ları otomatik olarak çalıştırmaya yönelik hizmet sorumlusu ayarlamayı sağlayan "farklı çalıştır hesabı" özelliğini kullanıma sunmuştur. Aşağıdaki makalede bu konuda daha fazla bilgi edinebilirsiniz:

* [Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure Otomasyonu dikey ölçek runbook 'larını aboneliğinize aktarın
Sanal makinenizi dikey ölçekleme için gereken runbook 'lar Azure Otomasyonu runbook galerisinde zaten yayımlandı. Bunları aboneliğinize aktarmanız gerekecektir. Aşağıdaki makaleyi okuyarak runbook 'ların nasıl içeri aktarılacağını öğrenebilirsiniz.

* [Azure Otomasyonu için runbook ve modül galerileri](../../automation/automation-runbook-gallery.md)

İçeri aktarılması gereken runbook 'lar aşağıdaki görüntüde gösteriliyor

![Runbook 'ları içeri aktarma](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Runbook 'na bir Web kancası ekleyin
Runbook 'ları içeri aktardıktan sonra, bir sanal makineden bir uyarı tarafından tetiklenmesi için Runbook 'a bir Web kancası eklemeniz gerekir. Runbook 'larınız için Web kancası oluşturma ayrıntıları burada okunabilir

* [Azure Otomasyonu web kancaları](../../automation/automation-webhooks.md)

Sonraki bölümde gerekli olacağı için Web kancası iletişim kutusunu kapatmadan önce Web kancasını kopyalamadığınızdan emin olun.

## <a name="add-an-alert-to-your-virtual-machine"></a>Sanal makinenize bir uyarı ekleyin
1. Sanal makine ayarlarını seçin
2. "Uyarı kuralları" nı seçin
3. "Uyarı Ekle" yi seçin
4. Uyarı tetiklenmesi için bir ölçüm seçin
5. Yerine getirilme Uyarının tetiklenmesine neden olacağını belirten bir koşul seçin
6. 5\. adımdaki koşul için bir eşik seçin. yerine getirilmesi
7. Adım 5 & 6 ' da izleme hizmetinin koşul ve eşiği denetlebileceği bir dönem seçin
8. Önceki bölümden kopyaladığınız Web kancasını yapıştırın.

![Sanal makineye uyarı Ekle 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Sanal makineye uyarı Ekle 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

