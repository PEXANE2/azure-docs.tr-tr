---
title: Kaynak gruplarını yönetme - Azure portalı
description: Kaynak gruplarınızı Azure Kaynak Yöneticisi aracılığıyla yönetmek için Azure portalını kullanın. Kaynak gruplarının nasıl oluşturulup listelenebildiğini ve silineceklerini gösterir.
author: mumian
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: 622c0cb8d58dddcdb13c7dc9412ada3f2e2d93e4
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632984"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Kaynak Yöneticisi kaynak gruplarını yönetme

Azure kaynak gruplarınızı yönetmek için [Azure Kaynak Yöneticisi](overview.md) ile Azure [portalını](https://portal.azure.com) nasıl kullanacağınızı öğrenin. Azure kaynaklarını yönetmek için Azure [portalını kullanarak Azure kaynaklarını yönet'e](manage-resources-portal.md)bakın.

Kaynak gruplarının yönetimi yle ilgili diğer makaleler:

- [Azure CLI'yi kullanarak Azure kaynak gruplarını yönetme](manage-resources-cli.md)
- [Azure PowerShell'i kullanarak Azure kaynak gruplarını yönetme](manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Kaynak grubu nedir

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genel olarak, aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynaklar ekleyin, böylece bunları grup olarak kolayca dağıtabilir, güncelleyebilir ve silebilirsiniz.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.


## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Kaynak gruplarını** seçin

    ![kaynak grubu ekleme](./media/manage-resource-groups-portal/manage-resource-groups-add-group.png)
3. **Ekle'yi**seçin.
4. Aşağıdaki değerleri girin:

   - **Abonelik**: Azure aboneliğinizi seçin. 
   - **Kaynak grubu**: Yeni bir kaynak grubu adı girin. 
   - **Bölge**: **Orta ABD**gibi bir Azure konumu seçin.

     ![kaynak grubu oluşturma](./media/manage-resource-groups-portal/manage-resource-groups-create-group.png)
5. **İnceleme Seç + Oluştur**
6. **Oluştur'u**seçin. Bir kaynak grubu oluşturmak birkaç saniye sürer.
7. Kaynak grubu listesini yenilemek için üst menüden **Yenile'yi** seçin ve ardından açmak için yeni oluşturulan kaynak grubunu seçin. Veya üstten **Bildirim**(çan simgesi) seçin ve ardından yeni oluşturulan kaynak grubunu açmak **için kaynak grubuna git'i** seçin

    ![kaynak grubuna gitme](./media/manage-resource-groups-portal/manage-resource-groups-add-group-go-to-resource-group.png)

## <a name="list-resource-groups"></a>Kaynak gruplarını listele

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Kaynak gruplarını listelemek için **Kaynak gruplarını** seçin

    ![kaynak gruplarına göz atın](./media/manage-resource-groups-portal/manage-resource-groups-list-groups.png)

3. Kaynak grupları için görüntülenen bilgileri özelleştirmek için **sütunları edit'i**seçin. Aşağıdaki ekran görüntüsü, ekrana ekleyebileceğiniz ek sütunları gösterir:

## <a name="open-resource-groups"></a>Açık kaynak grupları

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Kaynak grupları**’nı seçin.
3. Açmak istediğiniz kaynak grubunu seçin.

## <a name="delete-resource-groups"></a>Kaynak gruplarını silme

1. Silmek istediğiniz kaynak grubunu açın.  Bkz. [Açık kaynak grupları.](#open-resource-groups)
2. **Kaynak grubunu sil**'i seçin.

    ![azure kaynak grubunu silme](./media/manage-resource-groups-portal/delete-group.png)

Azure Kaynak Yöneticisi'nin kaynakların silinmesini nasıl emrettikleri hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi kaynak silme](delete-resource-group.md)bölümüne bakın.

## <a name="deploy-resources-to-a-resource-group"></a>Kaynakları kaynak grubuna dağıtma

Bir Kaynak Yöneticisi şablonu oluşturduktan sonra, Azure kaynaklarınızı dağıtmak için Azure portalını kullanabilirsiniz. Şablon oluşturmak için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](../templates/quickstart-create-templates-use-the-portal.md) Portalı kullanarak şablon dağıtmak için [Kaynak Yöneticisi şablonları ve Azure portalı yla kaynakları dağıt'a](../templates/deploy-portal.md)bakın.

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneye taşıma

Gruptaki kaynakları başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](move-resource-group-and-subscription.md).

## <a name="lock-resource-groups"></a>Kaynak gruplarını kilitleme

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi kritik kaynakları yanlışlıkla siler veya değiştirmesini engeller. 

1. Silmek istediğiniz kaynak grubunu açın.  Bkz. [Açık kaynak grupları.](#open-resource-groups)
2. Sol bölmede **Kilitler'i**seçin.
3. Kaynak grubuna kilit eklemek için **Ekle'yi**seçin.
4. **Kilit adı,** **Kilit türü**ve **Notlar**girin. Kilit türleri **salt okunur**ve **sil'** i içerir.

    ![azure kaynak grubunu kilitleme](./media/manage-resource-groups-portal/manage-resource-groups-add-lock.png)

Daha fazla bilgi [için, beklenmeyen değişiklikleri önlemek için kaynakları kilitle'ye](lock-resources.md)bakın.

## <a name="tag-resource-groups"></a>Kaynak gruplarını etiketleme

Varlıklarınızı mantıksal olarak düzenlemek için kaynak gruplarına ve kaynaklara etiketler uygulayabilirsiniz. Bilgi için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](tag-resources.md#portal)bakın.

## <a name="export-resource-groups-to-templates"></a>Kaynak gruplarını şablonlara dışa aktarma

Şablonları dışa aktarma hakkında bilgi için [şablona tek ve çok kaynak lı dışa aktarma - Portal' a](../templates/export-template-portal.md)bakın.

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için bkz. [RBAC ve Azure portalını kullanarak erişimi yönetme](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi'ni öğrenmek için [Azure Kaynak Yöneticisi'ne genel bakış](overview.md)'a bakın.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](../templates/template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.
