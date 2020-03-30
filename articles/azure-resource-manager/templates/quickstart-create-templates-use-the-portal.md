---
title: Şablonu dağıt - Azure portalı
description: Azure portalı kullanarak ilk Azure Resource Manager şablonunuzu oluşturmayı ve dağıtmayı öğrenin.
author: mumian
ms.date: 06/12/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: dd3d9caa8184b8637b509fc3318851751b211405
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80131865"
---
# <a name="quickstart-create-and-deploy-arm-templates-by-using-the-azure-portal"></a>Quickstart: Azure portalını kullanarak ARM şablonları oluşturma ve dağıtma

Azure portalını kullanarak bir Azure Kaynak Yöneticisi (ARM) şablonu ve şablonu portaldan düzenleme ve dağıtma işlemini öğrenin. ARM şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetme yle ilgili kavramları anlamak için [şablon dağıtımına genel bakış'a](overview.md)bakın.

![Kaynak Yöneticisi şablonu quickstart portal diyagramı](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-export-deploy-template-portal.png)

Öğreticiyi tamamladıktan sonra bir Azure Depolama hesabı dağıtırsınız. Aynı işlem diğer Azure kaynaklarını dağıtmak için de kullanılabilir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="generate-a-template-using-the-portal"></a>Portalı kullanarak şablon oluşturma

Özellikle Azure dağıtımında yeniyseniz ve JSON biçimini bilmiyorsanız, sıfırdan ARM şablonu oluşturmak kolay bir iş değildir. Azure portalını kullanarak, örneğin bir Azure Depolama hesabı gibi bir kaynağı yapılandırabilirsiniz. Kaynağı dağıtmadan önce yapılandırmanızı bir şablona aktarabilirsiniz. Şablonu kaydedebilir ve daha sonra yeniden kullanabilirsiniz.

Birçok deneyimli şablon geliştiricisi, aşina olmadıkları Azure kaynaklarını dağıtmaya çalıştıklarında şablon oluşturmak için bu yöntemi kullanır. Portalı kullanarak şablonları dışa aktarma hakkında daha fazla bilgi için kaynak [gruplarını şablonlara dışa aktarma'ya](../management/manage-resource-groups-portal.md#export-resource-groups-to-templates)bakın. Çalışma şablonu bulmanın diğer yolu azure [Quickstart şablonlarıdır.](https://azure.microsoft.com/resources/templates/)

1. Bir web tarayıcısında Azure [portalına](https://portal.azure.com) gidin ve oturum açın.
1. Azure portalı menüsünden **kaynak oluştur'u**seçin.

    ![Azure portalı menüsünden kaynak oluştur'u seçin](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-a-resource.png)

1. **Depolama** > **Deposu hesabını**seçin.

    ![Azure Storage hesabı oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
1. Aşağıdaki bilgileri girin:

    |Adı|Değer|
    |----|----|
    |**Kaynak grubu**|**Yeni Oluştur'u**seçin ve seçtiğiniz bir kaynak grubu adı belirtin. Ekran görüntüsünde kaynak grubu adı *mystorage1016rg* olarak belirtilmiştir. Kaynak grubu, Azure kaynakları için bir kapsayıcıdır. Kaynak grubu, Azure kaynaklarını yönetmeyi kolaylaştırır. |
    |**Adı**|Depolama hesabınıza benzersiz bir ad verin. Depolama hesabı adı tüm Azure'da benzersiz olmalıdır ve yalnızca küçük harfler ve sayılar içerir. Ad 3 ile 24 karakter arasında olmalıdır. "Depolama hesabı adı 'mystorage1016' zaten alınmıştır" yazan bir hata iletisi alırsanız, ** &lt;adınızı>depolama&lt;Bugünün tarihini MMDD>**,örneğin **johndolestorage1016'da**kullanmayı deneyin. Daha fazla bilgi için [Bkz. Adlandırma kuralları ve kısıtlamaları.](/azure/architecture/best-practices/resource-naming)|

    Diğer özellikler için varsayılan değerleri kullanabilirsiniz.

    ![Azure portalı kullanarak Azure depolama hesabı yapılandırması oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Dışarı aktarılan şablonların bazılarını kullanabilmeniz için yapmanız gereken düzenlemeler vardır.

1. Ekranın alt tarafından **Gözden geçir + oluştur**'u seçin. Bir sonraki adımda **Oluştur'u** seçmeyin.
1. Ekranın alt tarafından **Otomasyon için bir şablon indir**'i seçin. Portalda oluşturulan şablon gösterilir:

    ![Portaldan şablon oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    Şablon ana bölmede gösterilir. Bu altı üst düzey elemanları ile bir `schema` `contentVersion`JSON `variables` `resources`dosyasıdır `output`- , `parameters`, , , ve . Daha fazla bilgi için [bkz.](./template-syntax.md)

    Tanımlı altı parametre bulunur. Bunlardan biri **storageAccountName** olarak adlandırılmıştır. Önceki ekran görüntüsünde vurgulanan ikinci bölüm, şablondaki bu parametreye nasıl başvurulsüreceğini gösterir. Sonraki bölümde şablonu düzenleyerek depolama hesabı için oluşturulan bir adı kullanacaksınız.

    Şablonda bir Azure kaynağı tanımlanmıştır. Türü . `Microsoft.Storage/storageAccounts` Kaynağın nasıl tanımlandığına ve tanım yapısına bir göz atın.
1. Ekranın üst kısmından **İndir'i** seçin.
1. İndirilen zip dosyasını açın ve **sonra template.json'ı** bilgisayarınıza kaydedin. Sonraki bölümde bir şablon dağıtım aracı kullanarak şablonu düzenleyeceksiniz.
1. Parametre için sağladığınız değerleri görmek üzere **Parametre** sekmesini seçin. Bu değerleri not alın. Bir sonraki bölümde şablonu dağıtırken ihtiyacınız olacak.

    ![Portaldan şablon oluşturma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Hem şablon dosyasını hem de parametreler dosyasını kullanarak, bu öğreticide bir azure depolama hesabı olan bir kaynak oluşturabilirsiniz.

## <a name="edit-and-deploy-the-template"></a>Şablonu düzenleme ve dağıtma

Basit şablon düzenleme işlemleri için Azure portalı kullanabilirsiniz. Bu hızlı başlangıçta *Şablon Dağıtımı* adlı bir portal aracını kullanacaksınız. *Şablon Dağıtımı* bu öğreticide kullanılır, böylece tüm öğreticiyi tek bir arabirimi -Azure portalı- kullanarak tamamlayabilirsiniz. Daha karmaşık bir şablonu daha iyi bir şekilde düzenlenecek işlevleri sağlayan [Visual Studio Code'u](quickstart-create-templates-use-visual-studio-code.md)kullanmayı düşünün.

> [!IMPORTANT]
> Şablon Dağıtımı, basit şablonları sınaması için bir arabirim sağlar. Bu özelliğin üretimde kullanılması önerilmez. Bunun yerine, şablonlarınızı bir Azure depolama hesabında veya GitHub gibi bir kaynak kodu deposunda depolayın.

Azure'daki her Azure hizmetinin adının benzersiz olması gerekir. Zaten var olan bir depolama hesabı adı girdiyseniz dağıtım başarısız olabilir. Bu sorunu önlemek için, benzersiz bir depolama `uniquestring()` hesabı adı oluşturmak için şablon işlev çağrısı nı kullanacak şekilde şablonu değiştirirsiniz.

1. Azure portalı menüsünden veya **Ana** sayfadan **kaynak oluştur'u**seçin.
1. **Market içinde ara** alanına **şablon dağıtımı** yazın ve **ENTER** tuşuna basın.
1. **Şablon dağıtımı**'nı seçin.

    ![Azure Resource Manager şablon kitaplığı](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
1. **Oluştur'u**seçin.
1. **Düzenleyicide kendi şablonunuzu oluşturun**'u seçin.
1. **Dosya yükle**'yi seçin ve ardından yönergeleri izleyerek önceki bölümde indirdiğiniz template.json dosyasını yükleyin.
1. Şablonda aşağıdaki üç değişikliği yapın:

    ![Azure Resource Manager şablonları](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

   - Önceki ekran görüntüsünde gösterildiği gibi **storageAccountName** parametresini kaldırın.
   - Önceki ekran görüntüsünde gösterildiği gibi **storageAccountName** adlı bir değişken ekleyin:

       ```json
       "storageAccountName": "[concat(uniqueString(subscription().subscriptionId), 'storage')]"
       ```

       Burada iki şablon işlevi `concat()` `uniqueString()`kullanılır: ve .
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
1. **Kaydet'i**seçin.
1. Aşağıdaki değerleri girin:

    |Adı|Değer|
    |----|----|
    |**Kaynak grubu**|Son bölümde oluşturduğunuz kaynak grubu adını seçin. |
    |**Konum**|Depolama hesabı için bir konum seçin. Örneğin, **Orta ABD**. |
    |**Hesap Türü**|Bu hızlı başlangıç için **Standard_LRS** girin. |
    |**Tür**|Bu hızlı başlangıç için **StorageV2'yi** girin. |
    |**Erişim Katmanı**|Bu hızlı başlangıç için **Hot'a** girin. |
    |**Https Traffic Yalnızca Etkin**| Bu hızlı başlangıç için **true** değerini seçin. |
    |**Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**|(seçin)|

    Örnek bir dağıtımın ekran görüntüsü:

    ![Azure Resource Manager şablonlarını dağıtma](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

1. **Satın al**'ı seçin.
1. Dağıtım durumunu görmek için ekranın en üstündeki zil simgesini (bildirimler) seçin. **Dağıtım'ın devam ettiğini**göreceksiniz. Dağıtım tamamlanana kadar bekleyin.

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

Bu öğreticide, Azure portaldan şablon oluşturmayı ve portalı kullanarak şablonu dağıtmayı öğrendiniz. Bu Hızlı Başlangıçta kullanılan şablon, tek bir Azure kaynağına sahip basit bir şablondur. Şablon karmaşık olduğunda geliştirme için Visual Studio Code veya Visual Studio uygulamasını kullanmak daha kolaydır. Şablon geliştirme hakkında daha fazla bilgi edinmek için yeni başlangıç eğitimi serimize bakın:

> [!div class="nextstepaction"]
> [Başlangıç öğretileri](./template-tutorial-create-first-template.md)