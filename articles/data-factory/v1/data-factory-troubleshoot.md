---
title: Azure Veri Fabrikası sorunlarını giderme
description: Azure Veri Fabrikası'nı kullanmayla ilgili sorunları nasıl gidereceklerini öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: anandsub
robots: noindex
ms.openlocfilehash: 81ae5c3c702108d854e4dfde93001d5c99875666
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931575"
---
# <a name="troubleshoot-data-factory-issues"></a>Data Factory'de sorun giderme
> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. 

Bu makalede, Azure Veri Fabrikası'nı kullanırken sorunlar için sorun giderme ipuçları verilmektedir. Bu makalede, hizmeti kullanırken tüm olası sorunları listelemiyor, ancak bazı sorunları ve genel sorun giderme ipuçlarını kapsar.   

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Hata: Abonelik ‘Microsoft.DataFactory’ ad alanını kullanacak şekilde kaydedilmemiş
Bu hatayı aldıysanız Azure Data Factory kaynak sağlayıcısı makinenizde kayıtlı değildir. Şunları yapın:

1. Azure PowerShell’i çalıştırın.
2. Aşağıdaki komutu kullanarak Azure hesabınızda oturum açın.

    ```powershell
    Connect-AzAccount
    ```
3. Azure Veri Fabrikası sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın.

    ```powershell        
    Register-AzResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Sorun: Veri Fabrikası cmdlet çalıştırırken yetkisiz hata
Azure PowerShell ile doğru Azure hesabını veya aboneliğini kullanmıyor olabilirsiniz. Azure PowerShell ile kullanılacak doğru Azure hesabını ve aboneliğini seçmek için aşağıdaki cmdlet’leri kullanın.

1. Connect-AzAccount - Doğru kullanıcı kimliğini ve parolayı kullanın
2. Get-AzSubscription - Hesabın tüm aboneliklerini görüntüleyin.
3. Select-AzSubscription &lt;abonelik&gt; adı - Doğru aboneliği seçin. Azure portalında bir veri fabrikası oluşturmak için kullandığınızın aynısını kullanın.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Sorun: Azure portalından Veri Yönetimi Ağ Geçidi Express Kurulum'u başlatamamak
Veri Yönetimi Ağ Geçidi Hızlı Kurulumu için Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı gerekir. Hızlı Kurulum başlatılamıyorsa, aşağıdakilerden birini yapın:

* Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı kullanın.

    Chrome kullanıyorsanız, [Chrome web mağazasına](https://chrome.google.com/webstore/) gidin, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.

    Firefox için de aynısını yapın (eklentiyi yükleyin). Araç çubuğundaki Menüyü Aç düğmesine tıklayın (sağ üst köşede yer alan üç yatay çizgi), Eklentiler’e tıklayın, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.
* Portaldaki aynı bıçakta gösterilen **Manuel Kurulum** bağlantısını kullanın. Yükleme dosyasını karşıdan yüklemek ve el ile çalıştırmak için bu yaklaşımı kullanırsınız. Yükleme başarılı olduktan sonra, Veri Yönetimi Ağ Geçidi Yapılandırma iletişim kutusunu görürsünüz. Portal ekranındaki **anahtarı** kopyalayın ve ağ geçidini hizmete elle kaydetmek için yapılandırma yöneticisi içinde kullanın.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Sorun: Şirket içi SQL Server'a bağlanamaması
Ağ geçidi makinesinde **Veri Yönetimi Ağ Geçidi Configuration Manager'ı** başlatın ve ağ geçidi makinesinden SQL Server bağlantısını test etmek için Sorun **Giderme** sekmesini kullanın. Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Sorun: Giriş dilimleri sonsuza kadar bekleme durumunda
Dilimler çeşitli nedenlerden dolayı **Bekleme** durumunda olabilir. Ortak nedenlerden biri, **dış** özelliğin **doğru**olarak ayarlanmamasıdır. Azure Veri Fabrikası kapsamı dışında üretilen tüm veri kümeleri **dış** özellik ile işaretlenmiş olmalıdır. Bu özellik, verilerin dışsal olduğunu ve veri fabrikası içindeki herhangi bir ardışık işlem tarafından yedeklenmediğini gösterir. İlgili depoda veriler kullanılabilir duruma geldiğinde veri dilimleri **Hazır** olarak işaretlenir.

**External** özelliğinin kullanımı ile ilgili olarak aşağıdaki örneğe bakın. Harici **verileri*** doğru olarak harici olarak ayarladığınızda isteğe bağlı olarak belirtebilirsiniz.

Bu özellikle ilgili daha ayrıntılı bilgiler için [Veri kümeleri](data-factory-create-datasets.md) makalesine bakın.

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

Hatayı gidermek için girdi tablosunun JSON tanımına **external** özelliğini ve isteğe bağlı **externalData** bölümünü ekleyin ve tabloyu yeniden oluşturun.

### <a name="problem-hybrid-copy-operation-fails"></a>Sorun: Karma kopyalama işlemi başarısız oldu
Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi veri deposuna kopyalama/kullanma yla ilgili sorunları gidermek için sorunlara yönelik sorunlar için Sorun Giderme ağ [geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Sorun: İsteğe bağlı HDInsight sağlama başarısız olur
HDInsightOnDemand türünde bağlantılı bir hizmet kullanırken bir Azure Blob Depolama’ya işaret eden linkedServiceName belirlemeniz gerekir. Data Factory hizmeti isteğe bağlı HDInsight kümeniz için günlükleri ve destekleyici dosyaları depolamak için bu depoyu kullanır.  Bazen aşağıdaki hatayla isteğe bağlı bir HDInsight kümesinin sağlanması başarısız olur:

```
Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Bu hata genellikle linkedServiceName içinde belirtilen depolama hesabı konumunun HDInsight sağlama işleminin gerçekleştiği veri merkezi konumu ile aynı olmadığını gösterir. Örnek: Veri fabrikanız Batı ABD'deyse ve Azure depolama alanı Doğu ABD'deyse, isteğe bağlı sağlama Batı ABD'de başarısız olur.

Buna ek olarak, isteğe bağlı HDInsight içinde ek depolama hesaplarının belirlenebileceği ikinci bir JSON özelliği (additionalLinkedServiceNames) bulunmaktadır. Bu ek bağlantılı depolama hesapları HDInsight kümesiyle aynı konumda olmalıdır veya aynı hatayla başarısız olur.

### <a name="problem-custom-net-activity-fails"></a>Sorun: Özel .NET etkinliği başarısız olur
Bkz. Ayrıntılı adımlar için [özel etkinlik içeren bir ardışık sözcük](data-factory-use-custom-activities.md#troubleshoot-failures) hata ayıklama.

## <a name="use-azure-portal-to-troubleshoot"></a>Sorun giderme için Azure portalını kullanma
### <a name="using-portal-blades"></a>Portal bıçakları kullanma
Adımlar için [Monitör ardışık hattına](data-factory-monitor-manage-pipelines.md) bakın.

### <a name="using-monitor-and-manage-app"></a>İzleme ve Yönetme Uygulamasını kullanma
Ayrıntılar [için Monitor ve Manage App'i kullanarak veri fabrikası boru hatlarını izleyin ve yönetin.](data-factory-monitor-manage-app.md)

## <a name="use-azure-powershell-to-troubleshoot"></a>Sorun giderme için Azure PowerShell'i kullanın
### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Bir hatayı gidermek için Azure PowerShell'i kullanma
Ayrıntılar için [Azure PowerShell'i kullanarak Veri Fabrikası'nı İzle](data-factory-monitor-manage-pipelines.md) neşrlerine bakın.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: https://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
