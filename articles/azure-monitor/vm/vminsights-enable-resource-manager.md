---
title: Kaynak Yöneticisi şablonları kullanarak VM öngörülerini etkinleştirme
description: Bu makalede, Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak bir veya daha fazla Azure sanal makinesi veya sanal makine ölçek kümesi için VM öngörülerini nasıl etkinleştireceğinizi açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 57e2649dfe651bfa1e2ef18ff52ca611c122d696
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707498"
---
# <a name="enable-vm-insights-using-resource-manager-templates"></a>Kaynak Yöneticisi şablonları kullanarak VM öngörülerini etkinleştirme
Bu makalede, Kaynak Yöneticisi şablonları kullanarak bir sanal makine veya sanal makine ölçek kümesi için VM öngörülerinin nasıl etkinleştirileceği açıklanır. Bu yordam aşağıdakiler için kullanılabilir:

- Azure sanal makine
- Azure sanal makine ölçek kümesi
- Azure Arc ile bağlantılı karma sanal makine

## <a name="prerequisites"></a>Önkoşullar

- [Log Analytics çalışma alanı oluşturun ve yapılandırın](./vminsights-configure-workspace.md). 
- Etkinleştirmiş olduğunuz sanal makine veya sanal makine ölçek kümesinin işletim sisteminin desteklendiğinden emin olmak için [desteklenen işletim sistemleri](./vminsights-enable-overview.md#supported-operating-systems) bölümüne bakın. 

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Sanal makinelerinizi ve sanal makine ölçek kümelerinizi eklemeye yönelik örnek Azure Resource Manager şablonlar oluşturduk. Bu şablonlar, var olan bir kaynakta izlemeyi etkinleştirmek ve izleme özelliği etkin olan yeni bir kaynak oluşturmak için kullanabileceğiniz senaryolar içerir.

>[!NOTE]
>Şablonun, sanal makine veya sanal makine ölçek kümesi etkinleştirilmiş olarak aynı kaynak grubunda dağıtılması gerekir.


Azure Resource Manager şablonları, GitHub deponuzdan [indirebileceğiniz](https://aka.ms/VmInsightsARMTemplates) bir arşiv dosyasında (. zip) verilmiştir. Dosya içeriği, bir şablon ve parametre dosyası ile her bir dağıtım senaryosunu temsil eden klasörleri içerir. Çalıştırmadan önce, parametreler dosyasını değiştirin ve gerekli değerleri belirtin. 

İndirme dosyası farklı senaryolar için aşağıdaki şablonları içerir:

- **Existingvmontaslak** şablonu, sanal makine zaten mevcutsa VM öngörülerini sunar.
- **Newvmontaslak** şablonu, bir sanal makine oluşturur ve VM öngörülerini izlemek için sağlar.
- **ExistingVmssOnboarding** şablonu, sanal makine ölçek kümesi zaten varsa VM öngörülerini mümkün bir şekilde sunar.
- **NewVmssOnboarding** şablonu, sanal makine ölçek kümeleri oluşturur ve VM öngörülerini bunları izlemeye olanak sağlar.
- **Configureworkspace** şablonu, Linux ve Windows işletim sistemi performans sayaçlarının çözümlerini ve toplanmasını etkinleştirerek, sanal makine öngörülerini desteklemek için Log Analytics çalışma alanınızı yapılandırır.

>[!NOTE]
>Sanal Makine Ölçek Kümeleri zaten varsa ve yükseltme ilkesi **el ile** olarak ayarlandıysa, **ExistingVmssOnboarding** Azure Resource Manager şablonu çalıştırıldıktan sonra varsayılan olarak VM öngörüleri varsayılan olarak etkinleştirilmez. Örnekleri el ile yükseltmeniz gerekir.

## <a name="deploy-templates"></a>Şablon dağıtma
Şablonlar, PowerShell ve CLı kullanarak aşağıdaki örnekleri içeren [Kaynak Yöneticisi şablonları için herhangi bir dağıtım yöntemi](../../azure-resource-manager/templates/deploy-powershell.md) kullanılarak dağıtılabilir.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```


```azurecli
az deployment group create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```



## <a name="next-steps"></a>Sonraki adımlar

Artık sanal makineleriniz için izleme etkin olduğuna göre, bu bilgiler VM öngörüleri ile analiz edilmek üzere kullanılabilir.

- Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM Insights haritasını görüntüleme](vminsights-maps.md).

- VM performanlarınızın performans sorunlarını ve genel kullanımını belirlemek için bkz. [Azure VM performansını görüntüleme](vminsights-performance.md).