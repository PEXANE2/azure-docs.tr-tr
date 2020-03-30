---
title: Azure'da bir yönetim çözümü dosyası oluşturma | Microsoft Dokümanlar
description: Yönetim çözümleri, müşterilerin Azure ortamlarına ekleyebileceği paketlenmiş yönetim senaryoları sağlar.  Bu makalede, kendi ortamınızda kullanılmak üzere veya müşterilerinizin kullanımına sunulacak yönetim çözümlerini nasıl oluşturabileceğiniz hakkında ayrıntılı bilgi verilmektedir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/09/2018
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 999177f821b98adfa015520252bd3323d0892533
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275184"
---
# <a name="creating-a-management-solution-file-in-azure-preview"></a>Azure'da bir yönetim çözümü dosyası oluşturma (Önizleme)
> [!NOTE]
> Bu, Şu anda önizlemede olan Azure'da yönetim çözümleri oluşturmak için ön belgelerdir. Aşağıda açıklanan herhangi bir şema değişebilir.  

Azure'daki yönetim çözümleri [Kaynak Yöneticisi şablonları](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)olarak uygulanır.  Nasıl yazar yönetimi çözümleri öğrenmede temel görevi [nasıl bir şablon yazar](../../azure-resource-manager/templates/template-syntax.md)öğrenme.  Bu makalede, çözümler için kullanılan şablonların benzersiz ayrıntıları ve tipik çözüm kaynaklarının nasıl yapılandırılabildiğini sağlar.


## <a name="tools"></a>Araçlar

Çözüm dosyalarıyla çalışmak için herhangi bir metin düzenleyicisini kullanabilirsiniz, ancak aşağıdaki makalelerde açıklandığı şekilde Visual Studio veya Visual Studio Code'da sağlanan özelliklerden yararlanmanızı öneririz.

- [Visual Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md)
- [Visual Studio Code’da Azure Resource Manager Şablonları ile Çalışma](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)




## <a name="structure"></a>Yapı
Yönetim çözüm dosyasının temel yapısı, kaynak [yöneticisi şablonu](../../azure-resource-manager/templates/template-syntax.md#template-format)yla aynıdır ve aşağıdaki gibidir.  Aşağıdaki bölümlerin her biri bir çözümde üst düzey öğeleri ve içeriğini açıklar.  

    {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametreler
[Parametreler,](../../azure-resource-manager/templates/template-syntax.md#parameters) yönetim çözümcünü yüklediklerinde kullanıcıdan gerektiğingereken değerlerdir.  Tüm çözümlerin sahip olacağı standart parametreler vardır ve özel çözümünüz için gerekli olan ek parametreleri ekleyebilirsiniz.  Kullanıcıların çözümünüzü yüklerken parametre değerlerini nasıl sağlayacakları, belirli parametreye ve çözümün nasıl yüklendiğiniza bağlıdır.

Bir kullanıcı [yönetim çözümünüzü](solutions.md#install-a-monitoring-solution) Azure Marketi veya Azure QuickStart şablonları aracılığıyla yüklediğinde, bir [Log Analytics çalışma alanı ve Otomasyon hesabı](solutions.md#log-analytics-workspace-and-automation-account)seçmeleri istenir.  Bunlar, standart parametrelerin her birinin değerlerini doldurmak için kullanılır.  Kullanıcıdan standart parametreler için doğrudan değer sağlaması istenmez, ancak ek parametreler için değerler sağlaması istenir.


Örnek parametresi aşağıda gösterilmiştir.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Aşağıdaki tabloda bir parametreözözellikleri açıklanmaktadır.

| Öznitelik | Açıklama |
|:--- |:--- |
| type |Parametre için veri türü. Kullanıcı için görüntülenen giriş denetimi veri türüne bağlıdır.<br><br>bool - Açılan kutu<br>string - Metin kutusu<br>int - Metin kutusu<br>securestring - Parola alanı<br> |
| category |Parametre için isteğe bağlı kategori.  Aynı kategorideki parametreler birlikte gruplandırılır. |
|  denetimi |Dize parametreleri için ek işlevsellik.<br><br>datetime - Datetime denetimi görüntülenir.<br>guid - Guid değeri otomatik olarak oluşturulur ve parametre görüntülenmez. |
| açıklama |Parametre için isteğe bağlı açıklama.  Parametrenin yanındaki bilgi balonunda görüntülenir. |

### <a name="standard-parameters"></a>Standart parametreler
Aşağıdaki tabloda tüm yönetim çözümleri için standart parametreler listelenilmektedir.  Bu değerler, çözümünüz Azure Marketi veya Quickstart şablonlarından yüklendiğinde, onlar için istenmek yerine kullanıcı için doldurulur.  Çözüm başka bir yöntemle yüklenmişse, kullanıcı onlar için değerler sağlamalıdır.

> [!NOTE]
> Azure Marketi ve Quickstart şablonlarında bulunan kullanıcı arabirimi tablodaki parametre adlarını bekliyor.  Farklı parametre adları kullanırsanız, kullanıcıdan onlar için istenir ve bunlar otomatik olarak doldurulmaz.
>
>

| Parametre | Tür | Açıklama |
|:--- |:--- |:--- |
| accountName |string |Azure Otomasyon hesap adı. |
| fiyatlandırmaTier |string |Hem Log Analytics çalışma alanının hem de Azure Otomasyon hesabının fiyatlandırma katmanı. |
| regionId |string |Azure Otomasyon hesabının bölgesi. |
| Solutionname |string |Çözümün adı.  Çözümünüzü Quickstart şablonları aracılığıyla dağıtıyorsanız, solutionName'yi bir parametre olarak tanımlamanız gerekir, böylece kullanıcının bir tane belirtmesi gereken bir dize tanımlayabilirsiniz. |
| Workspacename |string |Günlük Analytics çalışma alanı adı. |
| çalışma alanıRegionId |string |Log Analytics çalışma alanının bölgesi. |


Çözüm dosyanıza kopyalayıp yapıştırabileceğiniz standart parametrelerin yapısı aşağıdadır.  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


Sözdizimi **parametreleri ('parametre adı')** ile çözümün diğer öğelerindeki parametre değerlerine başvurursunuz.  Örneğin, çalışma alanı adına erişmek için **parametreleri ('workspaceName')** kullanırsınız

## <a name="variables"></a>Değişkenler
[Değişkenler,](../../azure-resource-manager/templates/template-syntax.md#variables) yönetim çözümünün geri kalanında kullanacağınız değerlerdir.  Bu değerler, çözümü yükleyen kullanıcıya maruz kalmamış olur.  Bunlar, yazara çözüm boyunca birden çok kez kullanılabilecek değerleri yönetebilecekleri tek bir konum sağlamayı amaçlamaktadır. Kaynakları **öğesinde** sert kodlama yerine değişkenler içinde çözümünüze özgü herhangi bir değer koymalısınız.  Bu, kodu daha okunabilir hale getirir ve sonraki sürümlerde bu değerleri kolayca değiştirmenize olanak tanır.

Aşağıda, çözümlerde kullanılan tipik parametrelere sahip **değişkenler** öğesinin bir örneği verilmiştir.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Sözdizimi **değişkenleri ('değişken adı')** ile çözüm yoluyla değişken değerlerine başvurursunuz.  Örneğin, SolutionName değişkenine erişmek için **değişkenler ('SolutionName')** kullanırsınız.

Ayrıca, birden çok değer kümesi olan karmaşık değişkenler de tanımlayabilirsiniz.  Bunlar, farklı kaynak türleri için birden çok özellik tanımladığınız yönetim çözümlerinde özellikle yararlıdır.  Örneğin, yukarıda gösterilen çözüm değişkenlerini aşağıdaki lere yeniden yapılandırabilirsiniz.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Bu durumda, sözdizimi **değişkenleri ('değişken adı')** özelliği ile çözüm yoluyla değişken değerlerine başvurursunuz.  Örneğin, Çözüm Adı değişkenine erişmek için **değişkenler ('Çözüm') kullanırsınız. Adı**.

## <a name="resources"></a>Kaynaklar
[Kaynaklar,](../../azure-resource-manager/templates/template-syntax.md#resources) yönetim çözümünüzün yükleyeceği ve yapılandıracağı farklı kaynakları tanımlar.  Bu, şablonun en büyük ve en karmaşık bölümü olacaktır.  [Azure Kaynak Yöneticisi şablonları yazarken](../../azure-resource-manager/templates/template-syntax.md#resources)kaynak öğelerinin yapısını ve tam açıklamasını alabilirsiniz.  Genellikle tanımladığınız farklı kaynaklar, bu belgelerdeki diğer makalelerde ayrıntılı olarak açıklanır. 


### <a name="dependencies"></a>Bağımlılıklar
**Bağlı öğe** başka bir kaynağa [bir bağımlılık](../../azure-resource-manager/templates/define-resource-dependency.md) belirtir.  Çözüm yüklendiğinde, tüm bağımlılıkları oluşturulana kadar bir kaynak oluşturulmaz.  Örneğin, çözümünüzün bir [iş kaynağı](solutions-resources-automation.md#automation-jobs)kullanılarak yüklendiğinde [bir runbook başlatabilir.](solutions-resources-automation.md#runbooks)  İş kaynağı, çalışma defterinin iş oluşturulmadan önce oluşturulduğundan emin olmak için runbook kaynağına bağlıdır.

### <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı
Yönetim çözümleri, görünümler ve runbook'lar ve ilgili kaynakları içerecek bir [Otomasyon hesabı](../../automation/automation-security-overview.md#automation-account-overview) içerecek bir [Log Analytics çalışma alanı](../../azure-monitor/platform/manage-access.md) gerektirir.  Bunlar, çözümdeki kaynaklar oluşturulmadan önce kullanılabilir ve çözümün kendisinde tanımlanmamalıdır.  Kullanıcı çözümünüzü dağıtırken [bir çalışma alanı ve hesap belirtir,](solutions.md#log-analytics-workspace-and-automation-account) ancak yazar olarak aşağıdaki noktaları göz önünde bulundurmalısınız.


## <a name="solution-resource"></a>Çözüm kaynağı
Her çözüm, çözümün kendisini tanımlayan **kaynaklar** öğesinde bir kaynak girişi gerektirir.  Bu **microsoft.operationsmanagement/çözümleri** türüne sahip olacak ve aşağıdaki yapıya sahip olacaktır. Bu, çözümün özelliklerini tanımlamak için genellikle kullanılan [standart parametreleri](#parameters) ve [değişkenleri](#variables) içerir.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Bağımlılıklar
Çözüm oluşturulmadan önce var olmaları gerektiğinden, çözüm kaynağının çözümdeki diğer tüm kaynaklara [bağımlı](../../azure-resource-manager/templates/define-resource-dependency.md) olması gerekir.  Bunu, bağlı duruma bağlı öğesindeki her kaynak için bir giriş ekleyerek **yaparsınız.**

### <a name="properties"></a>Özellikler
Çözüm kaynağıaşağıdaki tabloda özelliklere sahiptir.  Bu, çözüm yüklendikten sonra kaynağın nasıl yönetildiğini tanımlayan çözüm tarafından başvurulan ve içerdiği kaynakları içerir.  Çözümdeki her **kaynak, başvurulan Kaynaklar** veya **bulunan Kaynaklar** özelliğinde listelenmelidir.

| Özellik | Açıklama |
|:--- |:--- |
| çalışma alanıResourceId |* \<Kaynak Grubu Kimliği>/sağlayıcıları/Microsoft.OperationalInsights/çalışma alanları/\<Çalışma Alanı Adı\>* formundaki Log Analytics çalışma alanının kimliği. |
| referans Kaynakları |Çözüm kaldırıldığında kaldırılmaması gereken kaynakların listesi. |
| containedResources |Çözüm kaldırıldığında kaldırılması gereken çözümdeki kaynakların listesi. |

Yukarıdaki örnek, runbook, zamanlama ve görünüm içeren bir çözüm içindir.  Zamanlama ve **runbook,** çözüm kaldırıldığında kaldırılmamak için özellikler öğesine *başvurur.*  Görünüm *içerdiği* nden çözüm kaldırıldığında kaldırılır.

### <a name="plan"></a>Planlama
Çözüm kaynağının **plan** varlığı aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| ad |Çözümün adı. |
| version |Yazar tarafından belirlenen çözümün sürümü. |
| ürün |Çözümü tanımlamak için benzersiz dize. |
| yayımcı |Çözümün yayıncısı. |



## <a name="next-steps"></a>Sonraki adımlar
* Yönetim çözümünüze [kaydedilmiş aramaları ve uyarıları ekleyin.](solutions-resources-searches-alerts.md)
* Yönetim çözümünüze [görünümler ekleyin.](solutions-resources-views.md)
* Yönetim çözümünüze [runbook'lar ve diğer Otomasyon kaynakları ekleyin.](solutions-resources-automation.md)
* Azure Kaynak [Yöneticisi şablonları yazma](../../azure-resource-manager/templates/template-syntax.md)ayrıntılarını öğrenin.
* Farklı Kaynak Yöneticisi şablonlarından örnekler için [Azure Quickstart Şablonlarında](https://azure.microsoft.com/documentation/templates) arama yapın.
