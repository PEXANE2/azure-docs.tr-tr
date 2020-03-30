---
title: Kaynakları yönetme - Azure portalı
description: Kaynaklarınızı yönetmek için Azure portalını ve Azure Kaynak Yöneticisi'ni kullanın. Kaynakların nasıl dağıtılsüreceğini ve silinini gösterir.
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 00d260a74807774d5bf226c3ec00a6b84f93b8d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248352"
---
# <a name="manage-azure-resources-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure kaynaklarını yönetme

Azure kaynaklarınızı yönetmek için [Azure Kaynak Yöneticisi](overview.md) ile Azure [portalını](https://portal.azure.com) nasıl kullanacağınızı öğrenin. Kaynak gruplarını yönetmek için Azure [portalını kullanarak Azure kaynak gruplarını yönet'e](manage-resource-groups-portal.md)bakın.

Kaynakların yönetimi yle ilgili diğer makaleler:

- [Azure CLI'yi kullanarak Azure kaynaklarını yönetme](manage-resources-cli.md)
- [Azure PowerShell'i kullanarak Azure kaynaklarını yönetme](manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

## <a name="deploy-resources-to-a-resource-group"></a>Kaynakları kaynak grubuna dağıtma

Bir Kaynak Yöneticisi şablonu oluşturduktan sonra, Azure kaynaklarınızı dağıtmak için Azure portalını kullanabilirsiniz. Şablon oluşturmak için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](../templates/quickstart-create-templates-use-the-portal.md) Portalı kullanarak şablon dağıtmak için [Kaynak Yöneticisi şablonları ve Azure portalı yla kaynakları dağıt'a](../templates/deploy-portal.md)bakın.

## <a name="open-resources"></a>Kaynakları aç

Azure kaynakları Azure hizmetleri ve kaynak grupları tarafından düzenlenir. Aşağıdaki yordamlar **mystorage0207**adlı bir depolama hesabının nasıl açılacağını gösterir. Sanal makine **mystorage0207rg**adlı bir kaynak grubunda bulunur.

Hizmet türüne göre bir kaynak açmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol bölmede Azure hizmetini seçin. Bu durumda, **Depolama hesapları**.  Listelenen hizmeti görmüyorsanız, **Tüm Hizmetler'i**seçin ve ardından hizmet türünü seçin.

    ![portalda azure kaynağını açma](./media/manage-resources-portal/manage-azure-resources-portal-open-service.png)

3. Açmak istediğiniz kaynağı seçin.

    ![portalda azure kaynağını açma](./media/manage-resources-portal/manage-azure-resources-portal-open-resource.png)

    Bir depolama hesabı gibi görünür:

    ![portalda azure kaynağını açma](./media/manage-resources-portal/manage-azure-resources-portal-open-resource-storage.png)

Kaynak grubuna göre kaynak açmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol bölmede, grup içindeki kaynağı listelemek için **Kaynak gruplarını** seçin.
3. Açmak istediğiniz kaynağı seçin. 

## <a name="manage-resources"></a>Kaynakları yönetme

Portaldaki bir kaynağı görüntülerken, belirli bir kaynağı yönetme seçeneklerini görürsünüz.

![Azure kaynaklarını yönetme](./media/manage-resources-portal/manage-azure-resources-portal-manage-resource.png)

Ekran görüntüsü, bir Azure sanal makinesinin yönetim seçeneklerini gösterir. Sanal makineyi başlatma, yeniden başlatma ve durdurma gibi işlemleri gerçekleştirebilirsiniz.

## <a name="delete-resources"></a>Kaynakları silme

1. Portaldaki kaynağı açın. Adımlar için [kaynakları aç'a](#open-resources)bakın.
2. **Sil**’i seçin. Aşağıdaki ekran görüntüsü, sanal bir makinenin yönetim seçeneklerini gösterir.

    ![azure kaynağını silme](./media/manage-resources-portal/manage-azure-resources-portal-delete-resource.png)
3. Silme işlemini onaylamak için kaynağın adını yazın ve sonra **Sil'i**seçin.

Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl emrettikleri hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi kaynak silme](delete-resource-group.md)bölümüne bakın.

## <a name="move-resources"></a>Kaynakları taşıma

1. Portaldaki kaynağı açın. Adımlar için [kaynakları aç'a](#open-resources)bakın.
2. **Taşı'yı**seçin. Aşağıdaki ekran görüntüsü, bir depolama hesabının yönetim seçeneklerini gösterir.

    ![azure kaynağını taşıma](./media/manage-resources-portal/manage-azure-resources-portal-move-resource.png)
3. İhtiyaçlarınıza bağlı olarak **başka bir kaynak grubuna taşı** veya başka bir **aboneliğe taşı'nı** seçin.

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

## <a name="lock-resources"></a>Kaynakları kilitleme

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi kritik kaynakları yanlışlıkla siler veya değiştirmesini engeller. 

1. Portaldaki kaynağı açın. Adımlar için [kaynakları aç'a](#open-resources)bakın.
2. **Kilitleri**seçin. Aşağıdaki ekran görüntüsü, bir depolama hesabının yönetim seçeneklerini gösterir.

    ![azure kaynağını kilitleme](./media/manage-resources-portal/manage-azure-resources-portal-lock-resource.png)
3. **Ekle'yi**seçin ve ardından kilit özelliklerini belirtin.

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resources"></a>Kaynakları etiketleme

Etiketleme, kaynak grubunuzu ve kaynaklarınızı mantıksal olarak düzenlemeye yardımcı olur. 

1. Portaldaki kaynağı açın. Adımlar için [kaynakları aç'a](#open-resources)bakın.
2. **Etiketler**'i seçin. Aşağıdaki ekran görüntüsü, bir depolama hesabının yönetim seçeneklerini gösterir.

    ![etiket azure kaynak](./media/manage-resources-portal/manage-azure-resources-portal-tag-resource.png)
3. Etiket özelliklerini belirtin ve sonra **Kaydet'i**seçin.

Bilgi için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md#portal)bakın.

## <a name="monitor-resources"></a>Kaynakları izleme

Bir kaynak açtığınızda, portal bu kaynak türünü izlemek için varsayılan grafikler ve tablolar sunar. Aşağıdaki ekran görüntüsü sanal bir makinenin grafiklerini gösterir:

![azure kaynağını izleme](./media/manage-resources-portal/manage-azure-resources-portal-monitor-resource.png)

Grafiği panoya sabitlemek için grafiklerin sağ üst köşesindeki pin simgesini seçebilirsiniz. Panolarla çalışma hakkında bilgi edinmek için Azure [portalında pano oluşturma ve paylaşma](../../azure-portal/azure-portal-dashboards.md)'ya bakın.

## <a name="manage-access-to-resources"></a>Kaynaklara erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi'ni öğrenmek için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](../templates/template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.
