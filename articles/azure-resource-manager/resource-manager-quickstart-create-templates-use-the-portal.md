---
title: Azure portal kullanarak bir Azure Resource Manager şablonu oluşturun ve dağıtın | Microsoft Docs
description: Azure portal kullanarak ilk Azure Resource Manager şablonunuzu oluşturmayı ve nasıl dağıtılacağını öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 06/12/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 708656ee88f5bacaa498bcd84fde34d69228d28b
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170278"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın

Azure portal kullanarak bir Kaynak Yöneticisi şablonu oluşturmayı ve portaldan şablonu düzenlemenin ve dağıtmanın işlemini öğrenin. Kaynak Yöneticisi şablonlar, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetme ile ilgili kavramları anlamak için bkz. [Azure Resource Manager genel bakış](resource-group-overview.md).

![Resource Manager şablonu hızlı başlangıç portalı diyagramı](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-export-deploy-template-portal.png)

Öğreticiyi tamamladıktan sonra bir Azure depolama hesabı dağıtırsınız. Aynı işlem, diğer Azure kaynaklarını dağıtmak için de kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="generate-a-template-using-the-portal"></a>Portalı kullanarak şablon oluşturma

Sıfırdan bir Kaynak Yöneticisi şablonu oluşturmak, özellikle de Azure dağıtımı için yeni bir işlemdir ve JSON biçimi hakkında bilgi sahibi değilseniz kolay bir görev değildir. Azure portal kullanarak bir kaynağı (örneğin, bir Azure Storage hesabı) yapılandırabilirsiniz. Kaynağı dağıtmadan önce, yapılandırmanızı bir Kaynak Yöneticisi şablonuna dışarı aktarabilirsiniz. Şablonu kaydedebilir ve daha sonra yeniden kullanabilirsiniz.

Birçok deneyimli şablon geliştiricisi, bu yöntemi, alışık oldukları Azure kaynaklarını dağıtmaya çalıştıklarında şablonlar oluşturmak için kullanır. Portalı kullanarak şablonları dışarı aktarma hakkında daha fazla bilgi için bkz. [kaynak gruplarını şablonlara dışarı aktarma](./manage-resource-groups-portal.md#export-resource-groups-to-templates). Çalışma şablonu bulmanın diğer yolu [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/)de bulunur.

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
2. @No__t **kaynak oluştur**' u seçin-1**depolama** > **depolama hesabı-blob, dosya, tablo, kuyruk**.

    ![Azure portal kullanarak bir Azure depolama hesabı oluşturun](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Aşağıdaki bilgileri girin:

    |Ad|Değer|
    |----|----|
    |**Kaynak grubu**|**Yeni oluştur**' u seçin ve tercih ettiğiniz bir kaynak grubu adı belirtin. Ekran görüntüsünde, kaynak grubu adı *mystorage1016rg*' dir. Kaynak grubu, Azure kaynakları için bir kapsayıcıdır. Kaynak grubu, Azure kaynaklarını yönetmeyi kolaylaştırır. |
    |**Ad**|Depolama hesabınıza benzersiz bir ad verin. Depolama hesabı adı tüm Azure genelinde benzersiz olmalıdır ve yalnızca küçük harf ve rakam içermelidir. Ad 3 ile 24 karakter arasında olmalıdır. "Depolama hesabı adı ' mystorage1016 ' zaten alınmış" olduğunu söyleyen bir hata iletisi alırsanız, **&lt;adınızı > depolama @ no__t-2Today's date >** , örneğin **johndolestorage1016**. Daha fazla bilgi için bkz. [adlandırma kuralları ve kısıtlamaları](/azure/cloud-adoption-framework/ready/considerations/naming-and-tagging#resource-naming).|

    Diğer özellikler için varsayılan değerleri kullanabilirsiniz.

    ![Azure portal kullanarak bir Azure depolama hesabı yapılandırması oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > İçe aktarılmış şablonlardan bazıları dağıtmadan önce bazı düzenlemeler yapılmasını gerektirir.

4. Ekranın alt kısmında **gözden geçir + oluştur** ' u seçin. Sonraki adımda **Oluştur** ' u seçmeyin.
5. Ekranın alt kısmındaki **Otomasyon için bir şablon indir** ' i seçin. Portal oluşturulan şablonu gösterir:

    ![Portaldan şablon oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    Ana bölmede şablon gösterilir. Altı üst düzey öğe içeren bir JSON dosyasıdır-`schema`, `contentVersion`, `parameters`, `variables`, `resources` ve `output`. Daha fazla bilgi için bkz [. Azure Resource Manager şablonlarının yapısını ve sözdizimini anlama](./resource-group-authoring-templates.md)

    Tanımlanmış altı parametre vardır. Bunlardan birine **storageAccountName**adı verilir. Önceki ekran görüntüsünde vurgulanan ikinci bölüm, şablondaki bu parametreye nasıl başvurulacağını gösterir. Sonraki bölümde, depolama hesabı için oluşturulmuş bir ad kullanmak üzere şablonu düzenlersiniz.

    Şablonda, bir Azure kaynağı tanımlanmıştır. Tür `Microsoft.Storage/storageAccounts` ' dır. Kaynağın nasıl tanımlandığını ve tanım yapısını göz atın.
6. Ekranın üstünden **İndir** ' i seçin.
7. İndirilen ZIP dosyasını açın ve ardından **Template. JSON** ' u bilgisayarınıza kaydedin. Sonraki bölümde, şablonu düzenlemek için bir şablon dağıtım aracı kullanırsınız.
8. Parametreler için belirttiğiniz değerleri görmek için **parametre** sekmesini seçin. Bu değerleri yazın, şablonu dağıttığınızda bir sonraki bölümde gereklidir.

    ![Portaldan şablon oluşturma](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Hem şablon dosyası hem de parametreler dosyasını kullanarak bir Azure depolama hesabı olan bu öğreticide bir kaynak oluşturabilirsiniz.

## <a name="edit-and-deploy-the-template"></a>Şablonu düzenleme ve dağıtma

Azure portal, bazı temel şablon düzenlemesini gerçekleştirmek için kullanılabilir. Bu hızlı başlangıçta, *şablon dağıtımı*adlı bir Portal aracı kullanırsınız. Bu öğreticide *şablon dağıtımı* , tek bir arabirim kullanarak tüm öğreticiyi tamamlayabilmeniz için kullanılır-Azure Portal. Daha karmaşık bir şablonu düzenlemek için, daha zengin düzenleme işlevleri sağlayan [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)kullanmayı düşünün.

> [!IMPORTANT]
> Şablon dağıtımı, basit şablonları test etmek için bir arabirim sağlar. Bu özelliğin üretimde kullanılması önerilmez. Bunun yerine, şablonlarınızı bir Azure depolama hesabında veya GitHub gibi bir kaynak kod deposunda saklayın.

Azure, her bir Azure hizmetinin benzersiz bir adı olmasını gerektirir. Zaten var olan bir depolama hesabı adı girdiyseniz dağıtım başarısız olabilir. Bu sorundan kaçınmak için, şablonu benzersiz bir depolama hesabı adı oluşturmak üzere `uniquestring()` şablon işlev çağrısı kullanacak şekilde değiştirirsiniz.

1. Azure portal **kaynak oluştur**' u seçin.
2. **Market 'Te ara**' te, **şablon dağıtımı**yazın ve ardından **ENTER**tuşuna basın.
3. **Şablon dağıtımı**seçin.

    ![Azure Resource Manager şablonları kitaplığı](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. **Oluştur**’u seçin.
5. **Düzenleyicide kendi şablonunuzu oluşturun öğesini**seçin.
6. **Dosya Yükle**' yi seçin ve ardından son bölümde indirdiğiniz Template. json dosyasını yüklemek için yönergeleri izleyin.
7. Şablonda aşağıdaki üç değişikliği yapın:

    ![Azure Resource Manager şablonları](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

   - **StorageAccountName** parametresini önceki ekran görüntüsünde gösterildiği gibi kaldırın.
   - Önceki ekran görüntüsünde gösterildiği gibi **storageAccountName** adlı bir değişken ekleyin:

       ```json
       "storageAccountName": "[concat(uniqueString(subscription().subscriptionId), 'storage')]"
       ```

       İki şablon işlevi burada kullanılır: `concat()` ve `uniqueString()`.
   - **Microsoft. Storage/storageAccounts** kaynağının Name öğesini parametresi yerine yeni tanımlanmış değişkeni kullanacak şekilde güncelleştirin:

       ```json
       "name": "[variables('storageAccountName')]",
       ```

     Son şablon şöyle görünmelidir:

     ```json
     {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "location": {
               "type": "string"
           },
           "accountType": {
               "type": "string"
           },
           "kind": {
               "type": "string"
           },
           "accessTier": {
               "type": "string"
           },
           "supportsHttpsTrafficOnly": {
               "type": "bool"
           }
       },
       "variables": {
           "storageAccountName": "[concat(uniqueString(subscription().subscriptionId), 'storage')]"
       },
       "resources": [
           {
               "name": "[variables('storageAccountName')]",
               "type": "Microsoft.Storage/storageAccounts",
               "apiVersion": "2018-07-01",
               "location": "[parameters('location')]",
               "properties": {
                   "accessTier": "[parameters('accessTier')]",
                   "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
               },
               "dependsOn": [],
               "sku": {
                   "name": "[parameters('accountType')]"
               },
               "kind": "[parameters('kind')]"
           }
       ],
       "outputs": {}
     }
     ```
8. **Kaydet**’i seçin.
9. Aşağıdaki değerleri girin:

    |Ad|Değer|
    |----|----|
    |**Kaynak grubu**|Son bölümde oluşturduğunuz kaynak grubu adını seçin. |
    |**Konum**|Depolama hesabı için bir konum seçin. Örneğin, **Orta ABD**. |
    |**Hesap türü**|Bu hızlı başlangıç için **Standard_LRS** girin. |
    |**Denetlenmesi**|Bu hızlı başlangıç için **StorageV2** girin. |
    |**Erişim katmanı**|Bu hızlı başlangıç için **sık** erişimli girin. |
    |**Yalnızca HTTPS trafiği etkin**| Bu hızlı başlangıç için **doğru** öğesini seçin. |
    |**Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**|seçin|

    Örnek dağıtımın bir ekran görüntüsü aşağıda verilmiştir:

    ![Azure Resource Manager şablonları dağıtımı](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. **Satın al**' ı seçin.
11. Dağıtım durumunu görmek için ekranın üst kısmından zil simgesini (bildirimler) seçin. **Dağıtım devam ediyor**görürsünüz. Dağıtım tamamlanana kadar bekleyin.

    ![Azure Resource Manager şablonları dağıtım bildirimi](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Bildirim bölmesinden **kaynak grubuna git** ' i seçin. Aşağıdakine benzer bir ekran görürsünüz:

    ![Azure Resource Manager şablonları dağıtım kaynak grubu](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Dağıtım durumunun başarılı olduğunu ve kaynak grubunda yalnızca bir depolama hesabı olduğunu görebilirsiniz. Depolama hesabı adı, şablon tarafından oluşturulan benzersiz bir dizedir. Azure depolama hesaplarını kullanma hakkında daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Portal kullanarak Blobları karşıya yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal Sol menüdeki **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Depolama hesabını kaynak grubunda görürsünüz.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portal bir şablon oluşturmayı ve portalı kullanarak şablonu dağıtmayı öğrendiniz. Bu hızlı başlangıçta kullanılan şablon, tek bir Azure kaynağı olan basit bir şablondur. Şablon karmaşıksa, şablonu geliştirmek için Visual Studio Code veya Visual Studio kullanmak daha kolay olur. Şablon geliştirme hakkında daha fazla bilgi edinmek için bkz. Yeni Başlangıç Öğreticisi serisi:

> [!div class="nextstepaction"]
> [Başlangıç öğreticileri](./template-tutorial-create-first-template.md)