---
title: Şablonu dağıt - IntelliJ IDEA
description: IntelliJ IDEA'yı kullanarak ilk Azure Kaynak Yöneticisi şablonunuzu nasıl oluştureceğinizi ve nasıl dağıtılacak yapılacağını öğrenin.
ms.devlang: java
ms.date: 08/01/2019
ms.topic: conceptual
ms.openlocfilehash: a5015a018f905b4353afd6bf25a48a2d942b3b97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153361"
---
# <a name="create-and-deploy-arm-templates-by-using-the-intellij-idea"></a>IntelliJ IDEA'yı kullanarak ARM şablonları oluşturun ve dağıtın

IntelliJ IDEA'yı kullanarak Azure Kaynak Yöneticisi (ARM) şablonunun Azure'a nasıl dağıtılabildiğini ve şablonu doğrudan IDE'den düzenleme ve güncelleştirme işlemini öğrenin. ARM şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetme yle ilgili kavramları anlamak için [şablon dağıtımına genel bakış'a](overview.md)bakın.

![Kaynak Yöneticisi şablon portalı diyagramı](./media/quickstart-create-templates-use-the-portal/azure-resource-manager-export-deploy-template-portal.png)

Öğreticiyi tamamladıktan sonra bir Azure Depolama hesabı dağıtırsınız. Aynı işlem diğer Azure kaynaklarını dağıtmak için de kullanılabilir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Yüklü [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition veya Community Edition
* [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) yüklü; daha fazla bilgi için [IntelliJ eklentiler yönetimi kılavuzuna](https://www.jetbrains.com/help/idea/managing-plugins.html) bakın
* Azure Toolkit for IntelliJ için Azure hesabınızda [oturum açmış](/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) olmalısınız

## <a name="deploy-a-quickstart-template"></a>Hızlı Başlangıç şablonu dağıtma

Sıfırdan şablon oluşturmak yerine, [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/)’ndan bir şablon açarsınız. Azure Quickstart Şablonları, ARM şablonları için bir depodur. Bu makalede kullanılan [şablona standart depolama hesabı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-storage-account-create/)adı verilir. Bir Azure Depolama hesabı kaynağını tanımlar.

1. Sağ tıklayın ve [`azuredeploy.json`](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) yerel [`azuredeploy.parameters.json`](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json) bilgisayarınıza kaydedin.

1. Azure Araç Kitiniz düzgün şekilde yüklenmiş ve oturum açmışsa, IntelliJ IDEA'nızın kenar çubuğunda Azure Explorer'ı görmeniz gerekir. **Kaynak Yönetimi'ne** sağ tıklayın ve **Dağıtım Oluştur'u**seçin.

    ![Kaynak Yöneticisi şablonu dağıtım oluşturmak için sağ tıklatın](./media/create-templates-use-intellij/resource-manager-create-deployment-right-click.png)

1. **Dağıtım Adınızı,** **Aboneliğinizi,** **Kaynak Grubunuzu**ve **Bölgenizi**Config. Burada şablonu yeni bir kaynak `testRG`grubuna dağıtıyoruz. Ardından, Kaynak **Şablonu** `azuredeploy.json` için yolu `azuredeploy.parameters.json` ve karşıdan yüklenirken **Kaynak Parametreleri'ni** seçin.

    ![Kaynak Yöneticisi şablonu dağıtım oluşturmak için dosyaları seçin](./media/create-templates-use-intellij/resource-manager-create-deployment-select-files.png)

1. Tamam'ı tıklattıktan sonra dağıtım başlatılır. Dağıtım tamamlanana kadar, alttaki IntelliJ **IDEA'nın durum çubuğundan** ilerlemeyi bulabilirsiniz.

    ![Kaynak Yöneticisi şablon dağıtım durumu](./media/create-templates-use-intellij/resource-manager-create-deployment-status.png)

## <a name="browse-an-existing-deployment"></a>Varolan bir dağıtıma göz atın

1. Dağıtım yapıldıktan sonra, yeni kaynak grubunu `testRG` ve oluşturulan yeni bir dağıtımı görebilirsiniz. Dağıtıma sağ tıkladığınızda olası eylemlerin listesini görebilirsiniz. Şimdi **Özellikleri Göster'i**seçin.

    ![Kaynak Yöneticisi şablonu dağıtıma göz atma](./media/create-templates-use-intellij/resource-manager-deployment-browse.png)

1. Bir sekme görünümü dağıtım durumu ve şablon yapısı gibi bazı yararlı özellikleri göstermek için açık olacaktır.

    ![Kaynak Yöneticisi şablonu dağıtım özelliklerini gösterir](./media/create-templates-use-intellij/resource-manager-deployment-show-properties.png)

## <a name="edit-and-update-an-existing-deployment"></a>Varolan bir dağıtımı yeniden ve güncelleştir

1. Sağ tıklama menüsünden **Dağıtımı Edit'i** veya önce özellikleri göster görünümünü seçin. Azure'daki dağıtım için şablon ve parametre dosyalarını gösteren başka bir sekme görünümü açık olacaktır. Bu dosyaları yerele kaydetmek için **Şablon Dosyasını Dışa** Aktar'ı veya **Parametre Dosyalarını Dışa Aktar'ı**tıklatabilirsiniz.

    ![Kaynak Yöneticisi şablon dağıtım ını yeniden](./media/create-templates-use-intellij/resource-manager-edit-deployment.png)

1. Bu sayfadaki iki dosyayı ve değişiklikleri Azure'a dağıtabilirsiniz. Burada parametre dosyalarındaki **storageAccountType** değerini `Standard_LRS` değiştirmek, `Standard_GRS`için . Ardından, alttaki **Dağıtımı Güncelleştir'i** tıklatın ve güncelleştirmeyi onaylayın.

    ![Kaynak Yöneticisi şablon dağıtım ını yeniden](./media/create-templates-use-intellij/resource-manager-edit-deployment-update.png)

1. Güncelleştirme dağıtımı tamamlandıktan sonra, portalda oluşturulan depolama hesabının `Standard_GRS`'' olarak değiştirildiğini doğrulayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Azure portalından veya Azure CLI'den yapabilirsiniz. IntelliJ IDEA'daki Azure Gezgini'nde, oluşturduğunuz **kaynak grubuna** sağ tıklayın ve sil'i seçin.

    ![Azure Explorer'daki kaynak grubunu IntelliJ IDEA'dan silme](./media/create-templates-use-intellij/delete-resource-group.png)

> [!NOTE]
> Dağıtımın silinmesi dağıtım tarafından oluşturulan kaynakları silmez. Artık ihtiyacınız yoksa lütfen ilgili kaynak grubunu veya belirli kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalenin ana odak noktası, Azure Quickstart şablonlarından varolan bir şablonu dağıtmak için IntelliJ IDEA'yı kullanmaktır. Ayrıca Azure'da varolan bir dağıtımı görüntülemeyi ve güncelleştirmeyi de öğrendiniz. Azure Hızlı Başlangıç şablonlarındaki şablonlar size ihtiyacınız olan her şeyi sağlamayabilir. Şablon geliştirme hakkında daha fazla bilgi edinmek için yeni başlangıç eğitimi serimize bakın:

> [!div class="nextstepaction"]
> [Başlangıç öğretileri](./template-tutorial-create-first-template.md)

> [!div class="nextstepaction"]
> [Azure Dev merkezinde Java'yı ziyaret edin](https://docs.microsoft.com/azure/java)
