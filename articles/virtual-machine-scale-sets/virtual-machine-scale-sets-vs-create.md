---
title: Visual Studio'u kullanarak Sanal Makine Ölçeği Seti'ni dağıtın
description: Visual Studio ve Resource Manager şablonu kullanarak Sanal Makine Ölçeği Kümelerini dağıtma
ms.custom: vs-azure, H1Hack27Feb2017
ms.workload: azure-vs
author: mayanknayar
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: manayar
ms.openlocfilehash: c49b4f42bc726c68880bdd4d6f58956936e83177
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066970"
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-visual-studio"></a>Visual Studio ile Sanal Makine Ölçeği Seti Nasıl Oluşturulur?

Bu makalede, Visual Studio Kaynak Grubu dağıtımını kullanarak Azure Sanal Makine Ölçeği Kümesi'ni nasıl dağıtabileceğiniz gösterilmektedir.

[Azure Sanal Makine Ölçeği Setleri,](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) otomatik ölçeklendirme ve yük dengeleme ile benzer sanal makineler den oluşan bir koleksiyonu dağıtmak ve yönetmek için bir Azure İşlem kaynağıdır. [Azure Kaynak Yöneticisi Şablonlarını](https://github.com/Azure/azure-quickstart-templates)kullanarak Sanal Makine Ölçeği Kümelerini sağlayabilir ve dağıtabilirsiniz. Azure Kaynak Yöneticisi şablonları Azure CLI, PowerShell, REST ve doğrudan Visual Studio kullanılarak dağıtılabilir. Visual Studio, Azure Kaynak Grubu dağıtım projesinin bir parçası olarak dağıtabileceğiniz bir dizi örnek şablon sağlar.

Azure Kaynak Grubu dağıtımları, ilgili Azure kaynaklarını tek bir dağıtım işleminde gruplandırmanın ve yayımlamanın bir yoludur. Daha fazla bilgi için Visual [Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)ya da dağıtma'ya bakın.

## <a name="prerequisites"></a>Ön koşullar

Visual Studio'da Sanal Makine Ölçeği Kümeleri dağıtmaya başlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

* Visual Studio 2013 veya sonrası
* Azure SDK 2.7, 2.8 veya 2.9

>[!NOTE]
>Bu makalede [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)ile Visual Studio 2019 kullanabilirsiniz.

## <a name="create-a-project"></a>Proje Oluşturma<a name="creating-a-project"></a> 

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin.

1. **Yeni bir proje oluştur'da**C# için Azure Kaynak **Grubu'nü** seçin ve ardından **İleri'yi**seçin.

1. **Yeni projenizi Yapılandır'da**bir ad girin ve **Oluştur'u**seçin.

    ![Projenizi adlandırın ve oluşturun](media/virtual-machine-scale-sets-vs-create/configure-azure-resource-group.png)

1. Şablonlar **listesinden, Windows Sanal Makine Ölçeği Kümesi'ni** veya Linux Sanal Makine Ölçeği **Seti** şablonu'nu seçin. **Tamam'ı**seçin.

   ![Sanal makine şablonu seçin](media/virtual-machine-scale-sets-vs-create/select-vm-template.png)

Projenizi oluşturduktan sonra **Solution Explorer** bir PowerShell dağıtım komut dosyası, Bir Azure Kaynak Yöneticisi şablonu ve Sanal Makine Ölçeği Kümesi için bir parametre dosyası içerir.

## <a name="customize-your-project"></a>Projenizi özelleştirin

Artık şablonu uygulamanızın gereksinimlerine göre özelleştirmek için edinebilirsiniz. Sanal makine uzantısı özellikleri ekleyebilir veya yük dengeleme kurallarını edebilirsiniz. Varsayılan olarak, Sanal Makine Ölçeği Seti şablonları **Azure Diagnostics** uzantısını dağıtacak şekilde yapılandırılır ve bu da otomatik ölçeklendirme kuralları eklemeyi kolaylaştırır. Şablonlar ayrıca, gelen NAT kurallarıyla yapılandırılan, genel IP adresine sahip bir yük dengeleyicisi de dağıtılır.

Yük dengeleyicisi, Sanal makine örneklerine SSH (Linux) veya RDP (Windows) ile bağlanmanızı sağlar. Ön uç bağlantı noktası aralığı 50000'den başlar. Linux için, SSH'den 50000 portuna kadar, yük dengeleme sileri sizi Ölçek Kümesi'ndeki ilk sanal makinenin 22. 50001 portuna bağlanmak, ikinci sanal makinenin 22.

 Visual Studio ile şablonlarınızı düzenlenin iyi bir yolu **JSON Anahat'ı**kullanmaktır. Parametreleri, değişkenleri ve kaynakları düzenleyebilirsiniz. Şema bir anlayış ile, Visual Studio dağıtmadan önce şablonunuzda hataları işaret edebilir.

![JSON Explorer](media/virtual-machine-scale-sets-vs-create/json-explorer.png)

## <a name="deploy-the-project"></a>Projeyi dağıtma

Sanal Makine Ölçeği Kümesi kaynağını oluşturmak için Azure Kaynak Yöneticisi şablonuna dağıtın:

1. **Çözüm Gezgini'nde,** projeyi sağ tıklatın ve**Yeni** **Dağıt'ı** > seçin.

    ![Projenizi dağıtma](media/virtual-machine-scale-sets-vs-create/deploy-new-project.png)

1. **Kaynak Grubuna**Dağıt'ta, hangi aboneliği n kullanacağını seçin ve bir kaynak grubu seçin. Gerekirse bir kaynak grubu oluşturabilirsiniz.

1. Ardından, şablonunuza geçirilen parametreleri girmek için **Parametreleri Edit'i** seçin.

   ![Abonelik ve kaynak grubunu girin](media/virtual-machine-scale-sets-vs-create/deploy-to-resource-group.png)

1. İşletim sistemi için kullanıcı adı ve parola sağlayın. Dağıtım oluşturmak için bu değerler gereklidir. Visual Studio için PowerShell Tools yüklü yoksa, gizli bir PowerShell komut istemiönlemek için **parolaları kaydet'i** seçin veya [Key Vault desteğini](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/)kullanın. Devam etmek için **Kaydet'i** seçin.

    ![Dağıtım parametrelerini edin](media/virtual-machine-scale-sets-vs-create/edit-deployment-parameters.png)

1. **Kaynak Grubuna**Dağıt'ta, **Dağıt'ı**seçin. Eylem, **Dağıt-AzureResourceGroup.ps1** komut dosyasını çalıştırıyor. **Çıktı** penceresi dağıtım ilerlemesini gösterir.

   ![Çıktı sonuçları gösterir](media/virtual-machine-scale-sets-vs-create/deployment-output.png)

## <a name="explore-your-virtual-machine-scale-set"></a>Sanal Makine Ölçek Setinizi Keşfedin<a name="exploring-your-virtual-machine-scale-set"></a>

Yeni Sanal Makine Ölçeği Kümesini görüntülemek için**Bulut Gezginini** **Görüntüle'yi** > seçin. Gerekirse **Tümlerini Yenile'yi**kullanın.

![Cloud Explorer](media/virtual-machine-scale-sets-vs-create/cloud-explorer.png)

**Cloud Explorer,** uygulamaları geliştirirken Visual Studio'da Azure kaynaklarını yönetmenize olanak tanır. Sanal Makine Ölçeği Setinizi Azure [portalında](https://portal.azure.com) ve [Azure Kaynak Gezgini'nde](https://resources.azure.com/)de görüntüleyebilirsiniz.

 Portal, Azure altyapınızı bir web tarayıcısıyla yönetmenin en iyi yolunu sağlar. Azure Kaynak Gezgini, Azure kaynaklarını keşfetmek ve hata ayıklamak için kolay bir yol sağlar. Azure Kaynak Gezgini örnek görünümünü sunar ve baktığınız kaynaklar için PowerShell komutlarını da gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio aracılığıyla Sanal Makine Ölçeği Kümelerini başarıyla dağıttıktan sonra, projenizi uygulama gereksinimlerinize uyacak şekilde daha da özelleştirebilirsiniz. Örneğin, **bir Insights** kaynağı ekleyerek otomatik ölçeklendirmeyi yapılandırın. Şablonunuza bağımsız sanal makineler gibi altyapı ekleyebilir veya özel komut dosyası uzantısını kullanarak uygulamaları dağıtabilirsiniz. Azure [Quickstart Şablonları](https://github.com/Azure/azure-quickstart-templates) GitHub deposunda iyi örnek şablonlar bulunabilir. `vmss` arayın.
