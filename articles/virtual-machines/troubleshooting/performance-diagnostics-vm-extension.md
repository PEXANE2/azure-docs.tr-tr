---
title: Windows için Azure Performans Tanılama VM Uzantısı| Microsoft Dokümanlar
description: Windows için Azure Performans Tanılama VM Uzantısını tanılar.
services: virtual-machines-windows'
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 6f104fc6513874bfef5f4bf9fe7f536c3e3d69cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71057539"
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Windows için Azure Performans Tanılama VM Uzantısı

Azure Performans Tanılama VM Uzantısı, Windows VM'lerden performans tanılama verilerinin toplanmasına yardımcı olur. Uzantı, çözümleme yapar ve sanal makinedeki performans sorunlarını tanımlamak ve çözmek için bulgular ve önerilerin bir raporunu sağlar. Bu [uzantı, PerfInsights](https://aka.ms/perfinsights)adlı bir sorun giderme aracı yükler.

> [!NOTE]
> Klasik olmayan VM'ler için Azure portalından VM'nizde tanılama çalıştırmak istiyorsanız, yeni deneyimi kullanmanız önerilir. Daha fazla bilgi için Azure [sanal makineleri için Performans Tanılama](performance-diagnostics.md) 

## <a name="prerequisites"></a>Ön koşullar

Bu uzantı Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016'da yüklenebilir. Windows 8.1 ve Windows 10'a da yüklenebilir.

## <a name="extension-schema"></a>Uzantı şeması
Aşağıdaki JSON, Azure Performans Tanılama VM Uzantısı şeasını gösterir. Bu uzantı, tanılama çıktısını depolamak ve bildirmek için bir depolama hesabının adını ve anahtarını gerektirir. Bu değerler hassastır. Depolama hesabı anahtarı korumalı ayar yapılandırması içinde depolanmalıdır. Azure VM uzantıkorumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür. **StorageAccountName** ve **storageAccountKey'in** büyük/küçük harf duyarlı olduğunu unutmayın. Gerekli diğer parametreler aşağıdaki bölümde listelenmiştir.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"        
        }
      }
    }
```

### <a name="property-values"></a>Özellik değerleri

|   **Adı**   |**Değer / Örnek**|       **Açıklama**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|API sürümü.
|yayımcı|Microsoft.Azure.Performance.Diagnostics|Uzantın için yayımcı ad alanı.
|type|AzurePerformanceDiagnostics|VM uzantısı türü.
|typeHandlerVersion|1.0|Uzantı işleyicisinin sürümü.
|performansSenaryo|Temel|Veri yakalamak için performans senaryosu. Geçerli değerler şunlardır: **temel**, **vmslow**, **azurefiles**, ve **özel**.
|traceDurationInSeconds|300|İzleme seçeneklerinden herhangi biri seçilirse, izlemelerin süresi.
|perfCounterTrace|p|Performans Sayacı İzleme'yi etkinleştirme seçeneği. Geçerli değerler **p** veya boş değerdir. Bu izlemeyi yakalamak istemiyorsanız, değeri boş olarak bırakın.
|ağTrace|n|Ağ İzleme'yi etkinleştirme seçeneği. Geçerli değerler **n** veya boş değerdir. Bu izlemeyi yakalamak istemiyorsanız, değeri boş olarak bırakın.
|xperfTrace|x|XPerf İzleme'yi etkinleştirme seçeneği. Geçerli değerler **x** veya boş değerdir. Bu izlemeyi yakalamak istemiyorsanız, değeri boş olarak bırakın.
|storPortTrace|s|StorPort İzleme'yi etkinleştirme seçeneği. Geçerli değerler **s** veya boş değerdir. Bu izlemeyi yakalamak istemiyorsanız, değeri boş olarak bırakın.
|srNumber|123452016365929|Varsa destek bilet numarası. Yoksa değeri boş olarak bırakın.
|requestTimeUtc|2017-09-28T22:08:53.736Z|Utc'deki Geçerli Tarih Saati. Bu uzantıyı yüklemek için portalı kullanıyorsanız, bu değeri sağlamanız gerekmez.
|resourceId|/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}|VM'nin benzersiz tanımlayıcısı.
|storageAccountName|mystorageaccount|Tanılama günlüklerini ve sonuçlarını depolamak için depolama hesabının adı.
|depolamaAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc==|Depolama hesabının anahtarı.

## <a name="install-the-extension"></a>Uzantıyı yükleme

Uzantıyı Windows sanal makinelerine yüklemek için aşağıdaki yönergeleri izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Bu uzantıyı yüklemek istediğiniz sanal makineyi seçin.

    ![Sanal makineler vurgulanmış Azure portalının ekran görüntüsü](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. **Uzantılar** bıçağını seçin ve **Ekle'yi**seçin.

    ![Add vurgulanmış uzantılı uzantıların ekran görüntüsü](media/performance-diagnostics-vm-extension/select-extensions.png)
4. **Azure Performans Tanılama'yı**seçin, hüküm ve koşulları gözden geçirin ve **Oluştur'u**seçin.

    ![Azure Performans Tanılama vurgulandığı Yeni kaynak ekranının ekran görüntüsü](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Yükleme için parametre değerlerini sağlayın ve uzantıyı yüklemek için **Tamam'ı** seçin. Desteklenen senaryolar hakkında daha fazla bilgi için [PerfInsights'ı nasıl kullanacağız'a](how-to-use-perfinsights.md#supported-troubleshooting-scenarios)bakın. 

    ![Yükle uzantı iletişim kutusunun ekran görüntüsü](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Yükleme başarılı olduğunda, bu durumu belirten bir ileti görürsünüz.

    ![Başarılı İletiyi Sağlama ekran görüntüsü](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Uzantı, sağlama başarılı olduğunda çalışır. Temel senaryonun tamamlanması iki dakika veya daha az sürer. Diğer senaryolar için, yükleme sırasında belirtilen süre boyunca çalışır.

## <a name="remove-the-extension"></a>Uzantıyı kaldırma
Uzantıyı sanal bir makineden kaldırmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın, bu uzantıyı kaldırmak istediğiniz sanal makineyi seçin ve ardından **Uzantılar** bıçağını seçin. 
2. Listeden Performans Tanılama Uzantısı girişi için (**...**) seçeneğini belirleyin ve **Kaldır'ı**seçin.

    ![Kaldırma vurgulanmış Uzantılar bıçağının ekran görüntüsü](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Ayrıca uzantı girişini ve **Kaldır** seçeneğini de seçebilirsiniz.

## <a name="template-deployment"></a>Şablon dağıtımı
Azure sanal makine uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak kullanılan JSON şeması, Azure Kaynak Yöneticisi şablonunda kullanılabilir. Bu, Azure Kaynak Yöneticisi şablon dağıtımı sırasında Azure Performans Tanılama VM uzantısını çalıştırır. Burada bir örnek şablon:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "performanceScenario": "[parameters('performanceScenario')]",
            "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
            "perfCounterTrace": "[parameters('perfCounterTrace')]",
            "networkTrace": "[parameters('networkTrace')]",
            "xperfTrace": "[parameters('xperfTrace')]",
            "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]",
            "resourceId": "[resourceId('Microsoft.Compute/virtualMachines', parameters('vmName'))]"
        },
        "protectedSettings": {
            "storageAccountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
  ]
}
```

## <a name="powershell-deployment"></a>PowerShell dağıtımı
Komut, `Set-AzVMExtension` Azure Performans Tanılama VM Uzantısı'nı varolan bir sanal makineye dağıtmak için kullanılabilir.

PowerShell

```
$PublicSettings = @{ "storageAccountName"="mystorageaccount";"performanceScenario"="basic";"traceDurationInSeconds"=300;"perfCounterTrace"="p";"networkTrace"="";"xperfTrace"="";"storPortTrace"="";"srNumber"="";"requestTimeUtc"="2017-09-28T22:08:53.736Z";"resourceId"="VMResourceId" }
$ProtectedSettings = @{"storageAccountKey"="mystoragekey" }

Set-AzVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS
```

## <a name="information-on-the-data-captured"></a>Yakalanan veriler le ilgili bilgiler
PerfInsights aracı, seçilen senaryoya bağlı olarak çeşitli günlükleri, yapılandırmave tanılama verilerini toplar. Daha fazla bilgi için [PerfInsights belgelerine](https://aka.ms/perfinsights)bakın.

## <a name="view-and-share-the-results"></a>Sonuçları görüntüleme ve paylaşma

Uzantıdan çıktı, yükleme sırasında belirtilen depolama hesabına yüklenen ve [Paylaşılan Erişim İmzaları (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md)kullanılarak 30 gün boyunca paylaşılan bir zip dosyasında bulunabilir. Bu zip dosyası tanılama günlükleri ve bulgular ve öneriler içeren bir rapor içerir. Çıktı zip dosyasına bir SAS bağlantısı klasörü altında *zipfilename*_saslink.txt adlı bir metin dosyası içinde bulunabilir **C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\\\<sürümü>**. Bu bağlantıya sahip herkes zip dosyasını indirebilir.

Destek biletiniz üzerinde çalışan destek mühendisine yardımcı olmak için Microsoft tanılama verilerini indirmek için bu SAS bağlantısını kullanabilir.

Raporu görüntülemek için zip dosyasını ayıklayın ve **PerfInsights Report.html** dosyasını açın.

Ayrıca uzantıyı seçerek zip dosyasını doğrudan portaldan indirebilmelisiniz.

![Performans Tanılama ayrıntılı durumu ekran görüntüsü](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Portalda görüntülenen SAS bağlantısı bazen çalışmayabilir. Bu, kodlama ve kod çözme işlemleri sırasında yanlış biçimlendirilmiş bir URL'den kaynaklanabilir. Bunun yerine bağlantıyı doğrudan VM'den *_saslink.txt dosyasından alabilirsiniz.

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

- Uzantı dağıtım durumu (bildirim alanında) uzantı başarıyla sağlanmış olsa bile "Dağıtım devam ediyor" gösterebilir.

    Uzantı durumu uzantınca başarıyla sağlanmış olduğunu gösterdiği sürece, bu sorun güvenle yoksayılabilir.
- Uzantı günlüklerini kullanarak yükleme sırasında bazı sorunları giderebilirsiniz. Uzantı yürütme çıktısı aşağıdaki dizinde bulunan dosyalara kaydedilir:

        C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics\<version>

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve destek **al'ı**seçin. Azure desteğini kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
