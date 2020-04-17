---
title: Azure Monitör'de Azure Key Vault çözümü | Microsoft Dokümanlar
description: Azure Key Vault günlüklerini incelemek için Azure Monitor'daki Azure Key Vault çözümünü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/27/2019
ms.openlocfilehash: 7da2fa2ddfbd9c71563dd8bd2e17b14c6dee62b3
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81455487"
---
# <a name="azure-key-vault-analytics-solution-in-azure-monitor"></a>Azure Monitor'da Azure Key Vault Analytics çözümü

![Anahtar Vault sembolü](media/azure-key-vault/key-vault-analytics-symbol.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Key Vault Denetim Etkinliği günlüklerini incelemek için Azure Monitor'daki Azure Key Vault çözümünü kullanabilirsiniz.

Çözümü kullanmak için Azure Key Vault tanılama günlüğe kaydetmeyi etkinleştirmeniz ve tanılamayı bir Log Analytics çalışma alanına yönlendirmeniz gerekir. Günlükleri Azure Blob depolama alanına yazmak gerekli değildir.

> [!NOTE]
> Ocak 2017'de Key Vault'tan Log Analytics'e günlük göndermenin desteklenen yolu değişti. Başlıkta kullandığınız Key Vault çözümü *(amortismana alınmış)* varsa, izlemeniz gereken adımlar için [eski Key Vault çözümünden geçişe](#migrating-from-the-old-key-vault-solution) bakın.
>
>

## <a name="install-and-configure-the-solution"></a>Çözümü yükleme ve yapılandırma
Azure Anahtar Kasası çözümlerini yüklemek ve yapılandırmak için aşağıdaki yönergeleri kullanın:

1. Log Analytics çalışma alanınıza Azure Anahtar Kasası çözümünü eklemek için [Çözümler Galerisi'nden Azure Monitörü Ekle çözümlerinde](../../azure-monitor/insights/solutions.md) açıklanan işlemi kullanın.
2. [Portal](#enable-key-vault-diagnostics-in-the-portal) veya [PowerShell'i](#enable-key-vault-diagnostics-using-powershell) kullanarak, izlemek için Key Vault kaynakları için tanılama günlüğe kaydetmeyi etkinleştirin

### <a name="enable-key-vault-diagnostics-in-the-portal"></a>Portalda Anahtar Kasatanısını etkinleştirme

1. Azure portalında, izlemek için Key Vault kaynağına gidin
2. Aşağıdaki sayfayı açmak için *Tanılama ayarlarını* seçin

   ![Azure Key Vault döşemesi görüntüsü](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics01.png)
3. Aşağıdaki sayfayı açmak için *tanılamayı Aç'ı* tıklatın

   ![Azure Key Vault döşemesi görüntüsü](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics02.png)
4. Tanılama ayarına bir ad verin.
5. *Günlük Analitiğine Gönder* için onay kutusunu tıklatın
6. Varolan bir Log Analytics çalışma alanı seçin veya bir çalışma alanı oluşturun
7. *AuditEvent* günlüklerini etkinleştirmek için Günlük altındaki onay kutusunu tıklatın
8. Log Analytics çalışma alanına tanılamanın günlüğe kaydedilmesini etkinleştirmek için *Kaydet'i* tıklatın.

### <a name="enable-key-vault-diagnostics-using-powershell"></a>PowerShell'i kullanarak Anahtar Kasası tanılamasını etkinleştirin
Aşağıdaki PowerShell komut dosyası, Key `Set-AzDiagnosticSetting` Vault için kaynak günlüğe kaydetmeyi etkinleştirmek için nasıl kullanılacağına bir örnek sağlar:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'

Set-AzDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Azure Key Vault veri toplama ayrıntılarını gözden geçirin
Azure Key Vault çözümü tanılama günlüklerini doğrudan Key Vault'tan toplar.
Günlükleri Azure Blob depolama alanına yazmak gerekli değildir ve veri toplama için aracı gerekmez.

Aşağıdaki tabloda veri toplama yöntemleri ve Azure Anahtar Kasası için verilerin nasıl toplandığıyla ilgili diğer ayrıntılar gösterilmektedir.

| Platform | Doğrudan aracı | Sistem Merkezi Operasyon Yöneticisi | Azure | Operasyon Yöneticisi gerekli mi? | Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | varışta |

## <a name="use-azure-key-vault"></a>Azure Key Vault kullanma
Çözümü [yükledikten](https://azuremarketplace.microsoft.com/en-usrketplace/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview)sonra, Azure Monitörü **Genel Bakış** sayfasından **Key Vault Analytics** döşemesini tıklatarak Key Vault verilerini görüntüleyin. **Öngörüler** bölümünde **daha fazla** seçeneğini tıklayarak **Azure Monitor** menüsünden bu sayfayı açın. 

![Azure Key Vault döşemesi görüntüsü](media/azure-key-vault/log-analytics-keyvault-tile.png)

Key Vault **Analytics** döşemesini tıklattıktan sonra, günlüklerinizin özetlerini görüntüleyebilir ve ardından aşağıdaki kategorilerin ayrıntılarını inceleyebilirsiniz:

* Zaman içinde tüm önemli kasa işlemlerinin hacmi
* Zaman içinde başarısız işlem hacimleri
* Operasyona göre ortalama operasyonel gecikme
* 1000 ms'den fazla süren operasyon sayısı ve 1000 ms'den fazla işlem listesi ile işlemler için hizmet kalitesi

![Azure Key Vault panosu görüntüsü](media/azure-key-vault/log-analytics-keyvault01.png)

![Azure Key Vault panosu görüntüsü](media/azure-key-vault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Herhangi bir işlemin ayrıntılarını görüntülemek için
1. Genel **Bakış** **sayfasında, Key Vault Analytics** döşemesini tıklatın.
2. Azure **Anahtar Kasası** panosunda, bıçaklardan birinde özet bilgileri gözden geçirin ve ardından günlük arama sayfasında bu konudaayrıntılı bilgileri görüntülemek için bir tanesini tıklatın.

    Günlük arama sayfalarından herhangi birinde, sonuçları zamana, ayrıntılı sonuçlara ve günlük arama geçmişinize göre görüntüleyebilirsiniz. Sonuçları daraltmak için de yüzlere göre filtre uygulayabilirsiniz.

## <a name="azure-monitor-log-records"></a>Azure Monitör günlük kayıtları
Azure Key Vault çözümü, Azure Tanılama'daki [AuditEvent günlüklerinden](../../key-vault/general/logging.md) toplanan bir tür **KeyVault** türüne sahip kayıtları analiz eder.  Bu kayıtların özellikleri aşağıdaki tabloda dır:  

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*AzureDiagnostics* |
| `SourceSystem` |*Azure* |
| `CallerIpAddress` |Talebi yapan müşterinin IP adresi |
| `Category` | *Denetim Etkinliği* |
| `CorrelationId` |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| `DurationMs` |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. Bu süre ağ gecikmesini içermez, bu nedenle istemci tarafında ölçtüğüniz süre bu kez eşleşmeyebilir. |
| `httpStatusCode_d` |İstek tarafından döndürülen HTTP durum kodu (örneğin, *200*) |
| `id_s` |İsteğin benzersiz kimliği |
| `identity_claim_appid_g` | Başvuru Kimliği için GUID |
| `OperationName` |[Azure Anahtar Kasası Günlüğe Kaydetme'de](../../key-vault/general/logging.md) belgelenen işlemin adı |
| `OperationVersion` |İstemci tarafından istenen REST API sürümü (örneğin *2015-06-01)* |
| `requestUri_s` |İsteğin Uri |
| `Resource` |Anahtar kasasının adı |
| `ResourceGroup` |Anahtar kasasının kaynak grubu |
| `ResourceId` |Azure Resource Manager Kaynak Kimliği. Key Vault günlükleri için bu Anahtar Kasa kaynak kimliğidir. |
| `ResourceProvider` |*Microsoft. KEYVAULT* |
| `ResourceType` | *Tonoz* |
| `ResultSignature` |HTTP durumu (örneğin, *Tamam)* |
| `ResultType` |REST API isteğinin sonucu (örneğin, *Başarı)* |
| `SubscriptionId` |Anahtar Kasası içeren aboneliğin Azure abonelik kimliği |

## <a name="migrating-from-the-old-key-vault-solution"></a>Eski Key Vault çözümünden geçiş
Ocak 2017'de Key Vault'tan Log Analytics'e günlük göndermenin desteklenen yolu değişti. Bu değişiklikler aşağıdaki avantajları sağlar:
+ Günlükler, depolama hesabı kullanmaya gerek kalmadan doğrudan bir Log Analytics çalışma alanına yazılır
+ Günlüklerin oluşturulduğu andan itibaren günlüklerin Log Analytics'te kullanılabilir hale geldiği andan itibaren daha az gecikme süresi
+ Daha az yapılandırma adımı
+ Tüm Azure tanılama türleri için ortak bir biçim

Güncelleştirilmiş çözümü kullanmak için:

1. [Key Vault'tan doğrudan Log Analytics çalışma alanına gönderilecek tanılamayı yapılandırın](#enable-key-vault-diagnostics-in-the-portal)  
2. [Çözüm Galerisi'nden Azure Monitörü Ekle çözümlerinde](../../azure-monitor/insights/solutions.md) açıklanan işlemi kullanarak Azure Anahtar Kasası çözümünü etkinleştirin
3. Kaydedilen sorguları, panoları veya uyarıları yeni veri türünü kullanmak için güncelleştirme
   + Türü değiştirilir: KeyVaults to AzureDiagnostics. Anahtar Kasa Günlükleri'ne filtre yazmak için Kaynak Türü'ni kullanabilirsiniz.
   + Yerine: `KeyVaults`, kullanmak`AzureDiagnostics | where ResourceType'=="VAULTS"`
   + Alanlar: (Alan adları büyük/küçük harf duyarlıdır)
   + Addaki \_s, \_d veya \_g eki olan herhangi bir alan için, ilk karakteri küçük harfle değiştirin
   + Adı o soneki \_olan herhangi bir alan için, veriler iç içe geçen alan adlarını temel alan olarak tek tek alanlara ayrılır. Örneğin, arayanın UPN'si bir alanda depolanır`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   + Alan CallerIpAddress CallerIPAddress olarak değiştirildi
   + Alan RemoteIPCountry artık mevcut değil
4. Key *Vault Analytics (Deprecated)* çözümlerini kaldırın. PowerShell kullanıyorsanız,`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Değişiklikten önce toplanan veriler yeni çözümde görünmez. Eski Tür ve alan adlarını kullanarak bu verileri sorgulamaya devam edebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı Azure Anahtar Kasası verilerini görüntülemek için [Azure Monitor'da Günlük sorgularını](../../azure-monitor/log-query/log-query-overview.md) kullanın.
