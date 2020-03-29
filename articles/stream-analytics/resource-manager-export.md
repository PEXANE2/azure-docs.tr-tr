---
title: Azure Akışı Analizi işi Azure Kaynak Yöneticisi şablonu dışa aktarma
description: Bu makalede, Azure Akış Analizi işiniz için Azure Kaynak Yöneticisi şablonu nasıl dışa aktarılacak açıklanmaktadır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 52ea7b45d0dcdb3ae16b8212557ba6ab3344ff15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968926"
---
# <a name="export-an-azure-stream-analytics-job-azure-resource-manager-template"></a>Azure Akışı Analizi işi Azure Kaynak Yöneticisi şablonu dışa aktarma

[Azure Kaynak Yöneticisi şablonları,](../azure-resource-manager/templates/overview.md) altyapıyı kod olarak uygulamanıza olanak tanır. Şablon, kaynaklarınız için altyapıyı ve yapılandırmayı tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

Azure Kaynak Yöneticisi şablonu dışa aktararak bir Azure Akış Analizi işini yeniden dağıtabilirsiniz.

## <a name="open-a-job-in-vs-code"></a>VS Code'da iş açma

Bir şablon dışa aktarmadan önce, visual studio code'da varolan bir Akış Analizi işini açmanız gerekir. 

Yerel bir projeye iş aktarmak için, azure portalındaki **Akış Analizi** Gezgini'nde dışa aktarmak istediğiniz işi bulun. **Sorgu** sayfasından **Visual Studio'da Aç'ı**seçin. Sonra **Visual Studio Kodu**seçin.

![Visual Studio Code'da Açık Akış Analizi işi](./media/resource-manager-export/open-job-vs-code.png)

Akış Analizi işlerini yönetmek için Visual Studio Code'u kullanma hakkında daha fazla bilgi için [Visual Studio Code quickstart](quick-create-vs-code.md)bölümüne bakın.

## <a name="compile-the-script"></a>Komut dosyasını derleme 

Bir sonraki adım, iş komut dosyasını bir Azure Kaynak Yöneticisi şablonuna derlemektir. Komut dosyasını derlemeden önce, işinizin en az bir giriş ve bir çıktıyapılandırılmış olduğundan emin olun. Hiçbir giriş veya çıktı yapılandırılmamışsa, önce giriş ve çıktıyı yapılandırmanız gerekir.

1. Visual Studio Code'da, işinizin *Transformation.asaql* dosyasına gidin.

   ![Visual Studio Code'da Transformation.asaql dosyası](./media/resource-manager-export/transformation-asaql.png)

1. *Transformation.asaql* dosyasına sağ tıklayın ve MENÜden **ASA: Derleme Komut Dosyası'nı** seçin.

1. Akış Analizi iş çalışma alanınızda bir **Dağıtım** klasörü görünür dikkat edin.

1. Dağıtmak için kullanılan Azure Kaynak Yönetimi şablonu olan *JobTemplate.json* dosyasını keşfedin.

## <a name="complete-the-parameters-file"></a>Parametreler dosyasını tamamla

Ardından, Azure Kaynak Yönetimi şablon parametreleri dosyasını tamamlayın.

1. Visual Studio Code'da Stream Analytics iş çalışma alanınızın **Dağıtım** klasöründe bulunan *JobTemplate.parameters.json* dosyasını açın.

1. Giriş ve çıkış anahtarlarının null olduğuna dikkat edin. Null değerlerini giriş ve çıktı kaynaklarınız için gerçek erişim anahtarlarıyla değiştirin.

1. Parametreler dosyasını kaydedin.

## <a name="deploy-using-templates"></a>Şablonları kullanarak dağıtma

Önceki bölümde oluşturduğunuz Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Akış Analizi işinizi dağıtmaya hazırsınız.

PowerShell penceresinde aşağıdaki komutu çalıştırın. Gerçek kaynak grup adınız ve İş Çalışma alanınızın **Dağıtım Klasöründe** *WorkTemplate.json* ve *JobTemplate.parameters.json* dosyalarına giden tam dosya yollarını *içeren ResourceGroupName*, *TemplateFile*ve *TemplateParameterFile'ı* yeniden e-iş olarak yeniden uyguladığınızdan emin olun.

Azure PowerShell yapılandırıldıysanız, [Azure PowerShell modüllerini yükle'deki](https://docs.microsoft.com/powershell/azure/install-Az-ps)adımları izleyin.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName "<your resource group>" -TemplateFile "<path to JobTemplate.json>" -TemplateParameterFile "<path to JobTemplate.parameters.json>"
```

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code kullanarak azure akış analizi işlerini canlı girişle yerel olarak test edin](visual-studio-code-local-run-live-input.md)

* [Visual Studio Code (Önizleme) ile Azure Akış Analizi işlerini keşfedin](visual-studio-code-explore-jobs.md)