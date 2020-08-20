---
title: Azure Izleyici Aracısı için veri toplamayı yapılandırma (Önizleme)
description: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/19/2020
ms.openlocfilehash: 97ba976c841e192646ebb484fe4d004556e34c54
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88640846"
---
# <a name="configure-data-collection-for-the-azure-monitor-agent-preview"></a>Azure Izleyici Aracısı için veri toplamayı yapılandırma (Önizleme)
Veri toplama kuralları (DCR), Azure Izleyici 'ye gelen verileri tanımlar ve nereye gönderileceğini belirtir. Bu makalede, Azure Izleyici Aracısı kullanılarak sanal makinelerden veri toplamak için bir veri toplama kuralının nasıl oluşturulacağı açıklanır.

Veri toplama kurallarının tamamen açıklaması için bkz. [Azure izleyici 'de veri toplama kuralları (Önizleme)](data-collection-rule-overview.md).

> [!NOTE]
> Bu makalede, şu anda önizleme aşamasında olan Azure Izleyici Aracısı ile sanal makineler için verilerin nasıl yapılandırılacağı açıklanır. Genel olarak kullanılabilen aracıların açıklaması ve verilerin toplanması için nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md) .


## <a name="dcr-associations"></a>DCR ilişkilendirmeleri
Bir sanal makineye bir DCR uygulamak için, sanal makine için bir ilişki oluşturursunuz. Bir sanal makinenin birden çok DTU ile ilişkilendirmesi olabilir ve bir DCR ilişkili birden fazla sanal makine olabilir. Bu, her biri belirli bir gereksinimle eşleşen bir dizi DTU tanımlamanızı ve bunları yalnızca uygulandıkları sanal makinelere uygulamanızı sağlar. 

Örneğin, bir iş kolu uygulaması çalıştıran bir sanal makine kümesi ve SQL Server çalıştıran bir ortam düşünün. Tüm sanal makineler için geçerli olan bir varsayılan veri toplama kuralınız ve özel olarak iş kolu uygulaması ve SQL Server için veri toplayacak ayrı veri toplama kuralları olabilir. Sanal makineler için veri toplama kurallarına ait ilişkilendirmeler Aşağıdaki diyagrama benzer şekilde görünür.

![İçermektedir](media/data-collection-rule-azure-monitor-agent/associations.png)

## <a name="create-using-the-azure-portal"></a>Azure portalını kullanarak oluşturma
Bir veri toplama kuralı oluşturmak ve aboneliğinizdeki sanal makineleri bu kurala ilişkilendirmek için Azure portal kullanabilirsiniz. Azure Izleyici Aracısı otomatik olarak yüklenir ve henüz yüklenmemiş sanal makineler için yönetilen bir kimlik oluşturulur.

Azure portal **Azure izleyici** menüsünde, **Ayarlar** bölümünden **veri toplama kuralları** ' nı seçin. Yeni bir veri toplama kuralı ve ataması eklemek için **Ekle** ' ye tıklayın.

[![Veri toplama kuralları](media/azure-monitor-agent/data-collection-rules.png)](media/azure-monitor-agent/data-collection-rules.png#lightbox)

Yeni bir kural ve ilişkilendirme kümesi oluşturmak için **Ekle** ' ye tıklayın. Bir **kural adı** girip bir **abonelik** ve **kaynak grubu**belirtin. Bu, DCR 'in nerede oluşturulacağını belirtir. Sanal makineler ve ilişkilendirmeleri, Kiracıdaki herhangi bir abonelikte veya kaynak grubunda bulunabilir.

[![Veri toplama kuralı temelleri](media/azure-monitor-agent/data-collection-rule-basics.png)](media/azure-monitor-agent/data-collection-rule-basics.png#lightbox)

**Sanal makineler** sekmesinde, veri toplama kuralı uygulanmış olması gereken sanal makineleri ekleyin. Azure Izleyici Aracısı, henüz yüklenmemiş sanal makinelere yüklenir.

[![Veri toplama kuralı sanal makineleri](media/azure-monitor-agent/data-collection-rule-virtual-machines.png)](media/azure-monitor-agent/data-collection-rule-virtual-machines.png#lightbox)

**Toplama ve teslim** sekmesinde veri kaynağı **Ekle** ' ye tıklayarak veri kaynağı ve hedef kümesi ekleyin. Bir **veri kaynağı türü**seçin ve bu seçilecek ayrıntılar görüntülenir. Performans sayaçları için, önceden tanımlanmış bir nesne kümesi ve örnekleme hızı arasından seçim yapabilirsiniz. Olaylar için, bir dizi günlük veya tesis ve önem düzeyi arasından seçim yapabilirsiniz. 

[![Veri kaynağı temel](media/azure-monitor-agent/data-collection-rule-data-source-basic.png)](media/azure-monitor-agent/data-collection-rule-data-source-basic.png#lightbox)


Diğer günlükleri ve performans sayaçlarını belirtmek için **özel**' i seçin. Ardından, toplanacak belirli değerler için bir [XPath ](https://www.w3schools.com/xml/xpath_syntax.asp) belirtebilirsiniz. Örnekler için bkz. [örnek DCR](data-collection-rule-overview.md#sample-data-collection-rule) .

[![Veri kaynağı özel](media/azure-monitor-agent/data-collection-rule-data-source-custom.png)](media/azure-monitor-agent/data-collection-rule-data-source-custom.png#lightbox)

**Hedef** sekmesinde, veri kaynağı için bir veya daha fazla hedef ekleyin. Windows olay ve Syslog veri kaynakları yalnızca Azure Izleyici günlüklerine gönderebilir. Performans sayaçları, hem Azure Izleyici ölçümleri hem de Azure Izleyici günlüklerine gönderebilir.

[![Hedef](media/azure-monitor-agent/data-collection-rule-destination.png)](media/azure-monitor-agent/data-collection-rule-destination.png#lightbox)

Veri toplama kuralının ayrıntılarını ve VM kümesiyle ilişkilendirmeyi gözden geçirmek için **veri kaynağı Ekle** ' ye tıklayın ve ardından **+ Oluştur ' u gözden geçirin** . Oluşturmak için **Oluştur** ' a tıklayın.

> [!NOTE]
> Veri toplama kuralı ve ilişkilendirmeleri oluşturulduktan sonra, verilerin hedeflere gönderilmesi 5 dakikaya kadar sürebilir.

## <a name="createusingrestapi"></a>REST API kullanarak oluşturma
REST API kullanarak bir DCR ve ilişki oluşturmak için aşağıdaki adımları izleyin. 
1. [Örnek DCR](data-collection-rule-overview.md#sample-data-collection-rule)' de gösterilen JSON BIÇIMINI kullanarak DCR dosyasını el ile oluşturun.
2. [REST API](https://docs.microsoft.com/rest/api/monitor/datacollectionrules/create#examples)kullanarak kural oluşturun.
3. [REST API](https://docs.microsoft.com/rest/api/monitor/datacollectionruleassociations/create#examples)kullanarak, her bir sanal makine için veri toplama kuralına bir ilişki oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Izleyici Aracısı](azure-monitor-agent-overview.md)hakkında daha fazla bilgi edinin.
- [Veri toplama kuralları](data-collection-rule-overview.md)hakkında daha fazla bilgi edinin.
