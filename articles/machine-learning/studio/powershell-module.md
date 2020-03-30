---
title: PowerShell modülleri
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) çalışma alanları, denemeler, web hizmetleri ve daha fazlasını oluşturmak ve yönetmek için PowerShell'i kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 04/25/2019
ms.openlocfilehash: 6afd222730a9864e0b8edd681b1ce919b03c3be8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204299"
---
# <a name="powershell-modules-for-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio için PowerShell modülleri (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

PowerShell modüllerini kullanarak Studio (klasik) kaynaklarınızı ve çalışma alanları, veri kümeleri ve web hizmetleri gibi varlıkları nızı programlı olarak yönetebilirsiniz.

Üç Powershell modülünü kullanarak Studio (klasik) kaynaklarla etkileşim kurabilirsiniz:

* [Azure PowerShell Az](#az-rm) 2018 yılında piyasaya sürüldü, farklı cmdlet adlarıyla da olsa AzureRM'ın tüm işlevlerini içerir
* [AzureRM](#az-rm) 2016 yılında piyasaya çıktı, yerine PowerShell Az
* [Azure Machine Learning PowerShell classic](#classic) 2016'da piyasaya sürüldü

Bu PowerShell modüllerinin bazı benzerlikleri olsa da, her biri belirli senaryolar için tasarlanmıştır. Bu makalede, PowerShell modülleri arasındaki farklar açıklanır ve hangilerini seçeceğiniz karar vermenize yardımcı olur.  

Her modül tarafından hangi kaynakların desteklenedildiğini görmek için aşağıdaki [destek tablosunu](#support-table) kontrol edin. 

## <a name="azure-powershell-az-and-azurerm"></a><a name="az-rm"></a>Azure PowerShell Az ve AzureRM

Az artık Azure ile etkileşim kurmak için tasarlanan PowerShell modülüdür ve AzureRM'ın önceki tüm işlevlerini içerir. AzureRM hata düzeltmeleri almaya devam eder, ancak yeni cmdlets veya özellik almaz.  Az ve AzureRM, **Azure Kaynak Yöneticisi** dağıtım modelini kullanarak dağıtılan çözümleri yönetir. Bu kaynaklar Studio (klasik) çalışma alanları ve Studio (klasik) "Yeni" web hizmetlerini içerir. 

PowerShell klasik az veya AzureRM ile birlikte hem "yeni" hem de "klasik" kaynak türlerini kapsayacak şekilde yüklenebilir. Ancak, Az ve AzureRM'nin aynı anda yüklenmesi önerilmez. Az ve AzureRM arasında karar vermek için Microsoft, gelecekteki tüm dağıtımlar için Az önerir.  [Azure PowerShell Az'a girişte](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)Az ve AzureRM ile geçiş yolu hakkında daha fazla bilgi edinin.

Az ile başlamak için [Azure Az yükleme yönergelerini](https://docs.microsoft.com/powershell/azure/install-az-ps)izleyin.

## <a name="powershell-classic"></a><a name="classic"></a>PowerShell klasik

Studio (klasik) [PowerShell klasik modülü,](https://aka.ms/amlps) klasik dağıtım **modelini**kullanarak dağıtılan kaynakları yönetmenize olanak tanır. Bu kaynaklar Studio (klasik) kullanıcı varlıkları, "klasik" web hizmetleri ve "klasik" web hizmeti bitiş noktalarını içerir.

Ancak Microsoft, kaynakların dağıtımını ve yönetimini kolaylaştırmak için gelecekteki tüm kaynaklar için Kaynak Yöneticisi dağıtım modelini kullanmanızı önerir. Dağıtım modelleri hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Kaynak Yöneticisi ile klasik dağıtım makalesine](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) bakın.

PowerShell klasik ile başlamak [için,](https://github.com/hning86/azuremlps/blob/master/README.md)GitHub [sürüm paketi](https://github.com/hning86/azuremlps/releases) indirin ve yükleme talimatları izleyin. Yönergeler, indirilen/fermuarsız DLL'nin engelini nasıl kaldıracağınız ve powershell ortamınıza nasıl aktarılabildiğini açıklar.

PowerShell klasik az veya AzureRM ile birlikte hem "yeni" hem de "klasik" kaynak türlerini kapsayacak şekilde yüklenebilir.

## <a name="powershell-support-table"></a><a name="support-table"></a>PowerShell destek tablosu


| | **Az** |  **PowerShell klasik** |
| --- | --- | --- |
| Çalışma alanlarını oluşturma/silme | [Resource Manager şablonları](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Çalışma alanı taahhüt planlarını yönetme | [Yeni-AzmlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | |
| Çalışma alanı kullanıcılarını yönetme |  | [Add-AmlWorkspaceKullanıcılar](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Web hizmetlerini yönetme | [Yeni-AzmlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br>("yeni" web hizmetleri)|| [Yeni AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br>("klasik" web hizmetleri) |
| Web hizmeti uç noktalarını/anahtarlarını yönetme |  [Get-AzmlWebServiceKey](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekey)|  [Ekle-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint)|
| Kullanıcı veri kümelerini/eğitimli modelleri yönetme| | [AmlDataset'i Al](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Kullanıcı denemelerini yönetme |  | [Başlangıç-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Özel modülleri yönetme | | [Yeni AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Sonraki adımlar
Bu PowerShell modülünün tüm belgelerine başvurun:
* [PowerShell klasik](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
