---
title: Dikey ölçekAzure sanal makine ölçek setleri
description: Azure Otomasyonu ile uyarıları izleme ye yanıt olarak Sanal Makine'yi dikey olarak ölçeklendirme
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: manayar
ms.openlocfilehash: fa1dda2907e8400491c8d18897bb41fb9cff49fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76274427"
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Sanal makine ölçek setleri ile dikey otomatik ölçek

Bu makalede, yeniden sağlama lı veya yeniden sağlamadan Azure [Sanal Makine Ölçek Kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/) dikey olarak nasıl ölçeklendirilen açıklanmaktadır. 

*Ölçeklendirme* ve *küçültme*olarak da bilinen dikey ölçekleme, iş yüküne yanıt olarak sanal makine (VM) boyutlarını artırmak veya azaltmak anlamına gelir. Bu davranışı, iş yüküne bağlı olarak VM sayısının değiştirildiği, *ölçeklendirme* ve *ölçekolarak*da adlandırılan [yatay ölçeklemeyle](virtual-machine-scale-sets-autoscale-overview.md)karşılaştırın.

Yeniden sağlama, varolan bir VM'yi kaldırmak ve yenisiyle değiştirmek anlamına gelir. Sanal makine ölçeği kümesindeki Sanal M'lerin boyutunu artırdığınızda veya azalttığınızda, bazı durumlarda varolan VM'leri yeniden boyutlandırmak ve verilerinizi korumak isterken, diğer durumlarda yeni boyutta yeni VM'ler dağıtmanız gerekir. Bu belge her iki durumda da kapsar.

Dikey ölçekleme aşağıdaki nde yararlı olabilir:

* Sanal makineler üzerine inşa edilmiş bir hizmet az kullanılır (örneğin hafta sonları). VM boyutunun azaltılması aylık maliyetleri azaltabilir.
* Ek VM'ler oluşturmadan daha büyük taleple başa çıkmak için VM boyutunu artırma.

Sanal makine ölçeği kümenizden gelen metrik tabanlı uyarılara göre tetiklenecek dikey ölçeklendirme ayarlayabilirsiniz. Uyarı etkinleştirildiğinde, ölçek kurulumunuzu veya küçültünüzi ölçeklendirebilen bir runbook'u tetikleyen bir webhook'u çalıştırıyor. Dikey ölçekleme aşağıdaki adımları izleyerek yapılandırılabilir:

1. Çalıştırabilme özelliğine sahip bir Azure Otomasyon hesabı oluşturun.
2. Sanal makine ölçeği kümeleri için Azure Otomasyon Dikey Ölçek li kitapları aboneliğinize aktarın.
3. Runbook'unuza bir webhook ekleyin.
4. Webhook bildirimi kullanarak sanal makine ölçeği setinize bir uyarı ekleyin.

> [!NOTE]
> İlk Sanal Makine boyutu nedeniyle, ölçeklenebilir boyutları, küme geçerli Sanal Makine diğer boyutların kullanılabilirliği nedeniyle sınırlı olabilir dağıtılır. Bu makalede kullanılan yayınlanmış otomasyon runbook'larında bu durumla ilgileniriz ve yalnızca aşağıdaki VM boyutu çiftleri içinde ölçeklendiriliriz. Bu, Standard_D1v2 bir Sanal Makine'nin aniden Standard_G5 veya Basic_A0 küçültülmeyec Ayrıca kısıtlı Sanal Makine boyutları ölçek yukarı / aşağı desteklenmez. Aşağıdaki boyut çiftleri arasında ölçeklendirmeyapmayı seçebilirsiniz:
> 
> | VM boyutları ölçekleme çifti |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_A1_v2 |Standard_A8_v2 |
> | Standard_A2m_v2 |Standard_A8m_v2  |
> | Standard_B1s |Standard_B2s |
> | Standard_B1ms |Standard_B8ms |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1_v2 |Standard_D5_v2 |
> | Standard_D11_v2 |Standard_D14_v2 |
> | Standard_DS1_v2 |Standard_DS5_v2 |
> | Standard_DS11_v2 |Standard_DS14_v2 |
> | Standard_D2_v3 |Standard_D64_v3 |
> | Standard_D2s_v3 |Standard_D64s_v3 |
> | Standard_DC2s |Standard_DC4s |
> | Standard_E2_v3 |Standard_E64_v3 |
> | Standard_E2s_v3 |Standard_E64s_v3 |
> | Standard_F1 |Standard_F16 |
> | Standard_F1s |Standard_F16s |
> | Standard_F2sv2 |Standard_F72sv2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> | Standard_H8 |Standard_H16 |
> | Standard_H8m |Standard_H16m |
> | Standart_L4s |Standart_L32s |
> | Standard_L8s_v2 |Standard_L80s_v2 |
> | Standard_M8ms  |Standard_M128ms |
> | Standard_M32ls  |Standard_M64ls |
> | Standard_M64s  |Standard_M128s |
> | Standard_M64  |Standard_M128 |
> | Standard_M64m  |Standard_M128m |
> | Standard_NC6 |Standard_NC24 |
> | Standard_NC6s_v2 |Standard_NC24s_v2 |
> | Standard_NC6s_v3 |Standard_NC24s_v3 |
> | Standard_ND6s |Standard_ND24s |
> | Standard_NV6 |Standard_NV24 |
> | Standard_NV6s_v2 |Standard_NV24s_v2 |
> | Standard_NV12s_v3 |Standard_NV48s_v3 |
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Çalıştırabilme özelliğine sahip bir Azure Otomasyon Hesabı oluşturma
Yapmanız gereken ilk şey, sanal makine ölçeği ayar örneklerini ölçeklendirmek için kullanılan runbook'ları barındıran bir Azure Otomasyon hesabı oluşturmaktır. Yakın zamanda [Azure Otomasyonu,](https://azure.microsoft.com/services/automation/) runbook'ları kullanıcı adına otomatik olarak çalıştırmak için Hizmet Sorumlusu'nu ayarlamayı sağlayan "Hesap Olarak Çalıştır" özelliğini kullanıma sundu. Daha fazla bilgi için bkz.

* [Azure Farklı Çalıştır hesabıyla Runbook Kimlik Doğrulaması](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure Otomasyon Dikey Ölçek çalışma kitaplarını aboneliğinize aktarın

Sanal makine ölçek kümelerinizi dikey olarak ölçeklendirmek için gereken runbook'lar Azure Otomasyon Runbook Galerisi'nde zaten yayımlanmıştır. Aboneliğinize aktarmak için bu makaledeki adımları izleyin:

* [Azure Otomasyonu için runbook ve modül galerileri](../automation/automation-runbook-gallery.md)

RunBook'lar menüsünden Galeriye Gözat seçeneğini seçin:

![Alınacak runbook'lar][runbooks]

İçe aktarılması gereken runbook'lar gösterilir. Yeniden sağlama yla dikey ölçekleme isteyip istemediğiniz temel alınarak runbook'u seçin:

![Runbooks galerisi][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Runbook'unuza webhook ekleme

Runbook'ları aldıktan sonra, sanal makine ölçeği kümesinden gelen bir uyarı yla tetiklenebilmek için runbook'a bir webhook ekleyin. Runbook'unuz için bir webhook oluşturmanın ayrıntıları bu makalede açıklanmıştır:

* [Azure Otomasyon webhooks](../automation/automation-webhooks.md)

> [!NOTE]
> Bir sonraki bölümde bu adrese ihtiyacınız olacağından, webhook iletişim kutusunu kapatmadan önce webhook URI'yi kopyaladığınızdan emin olun.
> 
> 

## <a name="add-an-alert-to-your-virtual-machine-scale-set"></a>Sanal makine ölçek kümenize uyarı ekleme

Aşağıda sanal makine ölçeği kümesine nasıl uyarı ekleyeceğinigösteren bir PowerShell komut dosyası yer almaktadır. Uyarıyı ateşlemek için metrin adını almak için aşağıdaki makaleye bakın: [Azure Monitor ortak ölçümleri otomatikleştirin.](../azure-monitor/platform/autoscale-common-metrics.md)

```powershell
$actionEmail = New-AzAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> Dikey ölçeklemeve ilişkili hizmet kesintisini çok sık tetiklemeyi önlemek için uyarı için makul bir zaman penceresi yapılandırmanız önerilir. En az 20-30 dakika veya daha fazla bir pencere düşünün. Herhangi bir kesintiden kaçınmanız gerekiyorsa yatay ölçeklemeyi düşünün.
> 
> 

Uyarıların nasıl oluşturulacak hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Monitör PowerShell hızlı başlatma örnekleri](../azure-monitor/platform/powershell-quickstart-samples.md)
* [Azure Monitör Çapraz platform CLI hızlı başlatma örnekleri](../azure-monitor/platform/cli-samples.md)

## <a name="summary"></a>Özet

Bu makalede basit dikey ölçekleme örnekleri gösterilmektedir. Bu yapı taşları ile - Otomasyon hesabı, runbooks, webhooks, uyarılar - eylemlerin özelleştirilmiş bir dizi ile olayların zengin bir çeşitlilik bağlayabilirsiniz.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
