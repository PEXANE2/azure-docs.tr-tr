---
title: Şablon dağıtma-Azure portal
description: Azure portalı kullanarak ilk Azure Resource Manager şablonunuzu oluşturmayı ve dağıtmayı öğrenin.
author: mumian
ms.date: 06/12/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: dd3d9caa8184b8637b509fc3318851751b211405
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80131865"
---
# <a name="quickstart-create-and-deploy-arm-templates-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak ARM şablonları oluşturma ve dağıtma

Azure portal kullanarak bir Azure Resource Manager (ARM) şablonu oluşturmayı ve portaldan şablonu düzenlemenin ve dağıtmanın işlemini öğrenin. ARM şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetme ile ilgili kavramları anlamak için bkz. [şablon dağıtımına genel bakış](overview.md).

![Kaynak Yöneticisi şablonu hızlı başlangıç portalı diyagramı](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-export-deploy-template-portal.png)

Öğreticiyi tamamladıktan sonra bir Azure depolama hesabı dağıtırsınız. Aynı işlem, diğer Azure kaynaklarını dağıtmak için de kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="generate-a-template-using-the-portal"></a>Portalı kullanarak şablon oluşturma

Sıfırdan bir ARM şablonu oluşturmak, özellikle de Azure dağıtımı için yeni bir işlemdir ve JSON biçimi hakkında bilgi sahibi değilseniz kolay bir görev değildir. Azure portal kullanarak bir kaynağı (örneğin, bir Azure Storage hesabı) yapılandırabilirsiniz. Kaynağı dağıtmadan önce, yapılandırmanızı bir şablona dışarı aktarabilirsiniz. Şablonu kaydedebilir ve daha sonra yeniden kullanabilirsiniz.

Birçok deneyimli şablon geliştiricisi, bu yöntemi, alışık oldukları Azure kaynaklarını dağıtmaya çalıştıklarında şablonlar oluşturmak için kullanır. Portalı kullanarak şablonları dışarı aktarma hakkında daha fazla bilgi için bkz. [kaynak gruplarını şablonlara dışarı aktarma](../management/manage-resource-groups-portal.md#export-resource-groups-to-templates). Çalışma şablonu bulmanın diğer yolu [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/)de bulunur.

1. Bir Web tarayıcısında [Azure Portal](https://portal.azure.com) gidin ve oturum açın.
1. Azure portal menüsünde **kaynak oluştur**' u seçin.

    ![Azure portal menüsünden kaynak oluştur ' u seçin](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-a-resource.png)

1. **Depolama** > **depolama hesabı**' nı seçin.

    ![Azure Storage hesabı oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
1. Aşağıdaki bilgileri girin:

    |Adı|Değer|
    |----|----|
    |**Kaynak grubu**|**Yeni oluştur**' u seçin ve tercih ettiğiniz bir kaynak grubu adı belirtin. Ekran görüntüsünde kaynak grubu adı *mystorage1016rg* olarak belirtilmiştir. Kaynak grubu, Azure kaynakları için bir kapsayıcıdır. Kaynak grubu, Azure kaynaklarını yönetmeyi kolaylaştırır. |
    |**Adı**|Depolama hesabınıza benzersiz bir ad verin. Depolama hesabı adı tüm Azure genelinde benzersiz olmalıdır ve yalnızca küçük harf ve rakam içermelidir. Ad 3 ile 24 karakter arasında olmalıdır. "Depolama hesabı adı ' mystorage1016 ' zaten alınmış" olduğunu söyleyen bir hata iletisi alırsanız, ** &lt;adınızı>>&lt; **, örneğin **johndolestorage1016**. Daha fazla bilgi için bkz. [adlandırma kuralları ve kısıtlamaları](/azure/architecture/best-practices/resource-naming).|

    Diğer özellikler için varsayılan değerleri kullanabilirsiniz.

    ![Azure portalı kullanarak Azure depolama hesabı yapılandırması oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Dışarı aktarılan şablonların bazılarını kullanabilmeniz için yapmanız gereken düzenlemeler vardır.

1. Ekranın alt tarafından **Gözden geçir + oluştur**'u seçin. Sonraki adımda **Oluştur** ' u seçmeyin.
1. Ekranın alt tarafından **Otomasyon için bir şablon indir**'i seçin. Portalda oluşturulan şablon gösterilir:

    ![Portaldan şablon oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    Şablon ana bölmede gösterilir. Bu, en üst düzey `schema`altı öğe olan (, `contentVersion`, `parameters`, `variables`, `resources`, ve `output`) bir JSON dosyasıdır. Daha fazla bilgi için bkz [. ARM şablonlarının yapısını ve sözdizimini anlama](./template-syntax.md)

    Tanımlı altı parametre bulunur. Bunlardan biri **storageAccountName** olarak adlandırılmıştır. Önceki ekran görüntüsünde vurgulanan ikinci bölüm, şablondaki bu parametreye nasıl başvurulacağını gösterir. Sonraki bölümde şablonu düzenleyerek depolama hesabı için oluşturulan bir adı kullanacaksınız.

    Şablonda bir Azure kaynağı tanımlanmıştır. Türü `Microsoft.Storage/storageAccounts`. Kaynağın nasıl tanımlandığını ve tanım yapısını göz atın.
1. Ekranın üstünden **İndir** ' i seçin.
1. İndirilen ZIP dosyasını açın ve ardından **Template. JSON** ' u bilgisayarınıza kaydedin. Sonraki bölümde bir şablon dağıtım aracı kullanarak şablonu düzenleyeceksiniz.
1. Parametre için sağladığınız değerleri görmek üzere **Parametre** sekmesini seçin. Bu değerleri not alın. Bir sonraki bölümde şablonu dağıtırken ihtiyacınız olacak.

    ![Portaldan şablon oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Hem şablon dosyası hem de parametreler dosyasını kullanarak bir Azure depolama hesabı olan bu öğreticide bir kaynak oluşturabilirsiniz.

## <a name="edit-and-deploy-the-template"></a>Şablonu düzenleme ve dağıtma

Basit şablon düzenleme işlemleri için Azure portalı kullanabilirsiniz. Bu hızlı başlangıçta *Şablon Dağıtımı* adlı bir portal aracını kullanacaksınız. Bu öğreticide *şablon dağıtımı* , tek bir arabirim kullanarak tüm öğreticiyi tamamlayabilmeniz için kullanılır-Azure Portal. Daha karmaşık bir şablonu düzenlemek için, daha zengin düzenleme işlevleri sağlayan [Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md)kullanmayı düşünün.

> [!IMPORTANT]
> Şablon dağıtımı, basit şablonları test etmek için bir arabirim sağlar. Bu özelliğin üretimde kullanılması önerilmez. Bunun yerine, şablonlarınızı bir Azure depolama hesabında veya GitHub gibi bir kaynak kod deposunda saklayın.

Azure'daki her Azure hizmetinin adının benzersiz olması gerekir. Zaten var olan bir depolama hesabı adı girdiyseniz dağıtım başarısız olabilir. Bu sorundan kaçınmak için şablonu, benzersiz bir depolama hesabı adı oluşturmak üzere bir şablon `uniquestring()` işlevi çağrısı kullanacak şekilde değiştirirsiniz.

1. Azure portal menüsünden veya **giriş** sayfasından **kaynak oluştur**' u seçin.
1. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.
1. **Şablon dağıtımı**'nı seçin.

    ![Azure Resource Manager şablon kitaplığı](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
1. **Oluştur**’u seçin.
1. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.
1. **Dosya yükle**'yi seçin ve ardından yönergeleri izleyerek önceki bölümde indirdiğiniz template.json dosyasını yükleyin.
1. Şablonda aşağıdaki üç değişikliği yapın:

    ![Azure Resource Manager şablonları](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

   - **StorageAccountName** parametresini önceki ekran görüntüsünde gösterildiği gibi kaldırın.
   - Önceki ekran görüntüsünde gösterildiği gibi **storageAccountName** adlı bir değişken ekleyin:

       ```json
       "storageAccountName": "[concat(uniqueString(subscription().subscriptionId), 'storage')]"
       ```

       İki şablon işlevi burada kullanılır: `concat()` ve. `uniqueString()`
   - **Microsoft.Storage/storageAccounts** kaynağının name öğesini parametre yerine yeni tanımlanan değişkeni kullanacak şekilde güncelleştirin:

       ```json
       "name": "[variables('storageAccountName')]",
       ```

     Şablonun son halinin şu şekilde olması gerekir:

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
1. **Kaydet**’i seçin.
1. Aşağıdaki değerleri girin:

    |Adı|Değer|
    |----|----|
    |**Kaynak grubu**|Son bölümde oluşturduğunuz kaynak grubu adını seçin. |
    |**Konum**|Depolama hesabı için bir konum seçin. Örneğin, **Orta ABD**. |
    |**Hesap türü**|Bu hızlı başlangıç için **Standard_LRS** girin. |
    |**Denetlenmesi**|Bu hızlı başlangıç için **StorageV2** girin. |
    |**Erişim katmanı**|Bu hızlı başlangıç için **sık** erişimli girin. |
    |**Yalnızca HTTPS trafiği etkin**| Bu hızlı başlangıç için **true** değerini seçin. |
    |**Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**|seçin|

    Örnek bir dağıtımın ekran görüntüsü:

    ![Azure Resource Manager şablonlarını dağıtma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

1. **Satın al**'ı seçin.
1. Dağıtım durumunu görmek için ekranın en üstündeki zil simgesini (bildirimler) seçin. **Dağıtım devam ediyor**görürsünüz. Dağıtım tamamlanana kadar bekleyin.

    ![Azure Resource Manager şablonlarını dağıtma bildirimi](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

1. Bildirim bölmesinden **Kaynak grubuna git**'i seçin. Şuna benzer bir ekran görmeniz gerekir:

    ![Azure Resource Manager şablonlarını dağıtma kaynak grubu](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    Dağıtım durumunun başarılı olduğunu ve kaynak grubunda yalnızca bir depolama hesabı olduğunu görebilirsiniz. Depolama hesabı adı, şablon tarafından oluşturulan benzersiz bir dizedir. Azure depolama hesaplarını kullanma hakkında daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: Azure portalı kullanarak blobları yükleme, indirme ve listeleme](../../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
1. **Ada göre filtrele** alanına kaynak grubu adını girin.
1. Kaynak grubu adını seçin.  Depolama hesabı kaynak grubunda bulunmalıdır.
1. Üstteki menüden **Kaynak grubunu sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portaldan şablon oluşturmayı ve portalı kullanarak şablonu dağıtmayı öğrendiniz. Bu Hızlı Başlangıçta kullanılan şablon, tek bir Azure kaynağına sahip basit bir şablondur. Şablon karmaşık olduğunda geliştirme için Visual Studio Code veya Visual Studio uygulamasını kullanmak daha kolaydır. Şablon geliştirme hakkında daha fazla bilgi edinmek için bkz. Yeni Başlangıç Öğreticisi serisi:

> [!div class="nextstepaction"]
> [Başlangıç öğretileri](./template-tutorial-create-first-template.md)