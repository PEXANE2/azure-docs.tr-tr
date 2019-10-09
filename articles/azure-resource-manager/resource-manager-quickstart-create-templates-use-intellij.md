---
title: IntelliJ FIKRINI kullanarak bir Azure Resource Manager şablonu oluşturun ve dağıtın | Microsoft Docs
description: IntelliJ FIKRINI kullanarak ilk Azure Resource Manager şablonunuzu oluşturmayı ve bunu dağıtmayı öğrenin.
services: azure-resource-manager
documentationcenter: ''
author: yucwan
manager: ''
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: java
ms.date: 08/01/2019
ms.topic: quickstart
ms.author: yucwan
ms.openlocfilehash: d25a5720e3aaa8ad6306a72d2db8d3ca2e89861b
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169439"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-intellij-idea"></a>Hızlı başlangıç: IntelliJ FIKRINI kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın

IntelliJ FIKRINI kullanarak Azure 'a bir Kaynak Yöneticisi şablonu dağıtmayı ve doğrudan IDE 'den şablonu düzenlemeyle güncelleştirme işlemini öğrenin. Kaynak Yöneticisi şablonlar, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi dağıtma ve yönetme ile ilgili kavramları anlamak için bkz. [Azure Resource Manager genel bakış](resource-group-overview.md).

![Kaynak Yöneticisi şablonu hızlı başlangıç portalı diyagramı](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-export-deploy-template-portal.png)

Öğreticiyi tamamladıktan sonra bir Azure depolama hesabı dağıtırsınız. Aynı işlem, diğer Azure kaynaklarını dağıtmak için de kullanılabilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için şunlar gerekir:

* [IntelliJ fikir](https://www.jetbrains.com/idea/download/) Ultimate sürümü veya Community Edition yüklendi
* [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) yüklendi, daha fazla bilgi Için [IntelliJ 'ın eklenti yönetim kılavuzunu](https://www.jetbrains.com/help/idea/managing-plugins.html) denetleyin
* Azure Toolkit for IntelliJ için Azure hesabınızda [oturum](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-sign-in-instructions) açmanız

## <a name="deploy-a-quickstart-template"></a>Hızlı başlangıç şablonu dağıtma

Sıfırdan şablon oluşturmak yerine [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/)bir şablon açarsınız. Azure hızlı başlangıç şablonları Kaynak Yöneticisi şablonlar için bir depodur. Bu hızlı başlangıçta kullanılan şablona [Standart depolama hesabı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-storage-account-create/)adı verilir. Bir Azure depolama hesabı kaynağını tanımlar.

1. [@No__t-1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) ve [`azuredeploy.parameters.json`](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json) ' i sağ tıklayıp yerel bilgisayarınıza kaydedin.

1. Azure Toolkit 'niz düzgün yüklenip oturum açmışsa, IntelliJ fıkrın kenar çubuğunda Azure Explorer ' ı görmeniz gerekir. **Kaynak yönetimine** sağ tıklayın ve **dağıtım oluştur**' u seçin.

    ![Kaynak Yöneticisi şablonu dağıtım oluşturmak için sağ tıklayın](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-create-deployment-right-click.png)

1. **Dağıtım adınızı**, **aboneliğinizi**, **kaynak grubunuzu**ve **bölgenizi**yapılandırma. Burada, şablonu yeni bir kaynak grubuna dağıttık `testRG`. Daha sonra, `azuredeploy.json` ve **kaynak parametreleri** olarak **kaynak şablonu** ' nu, indirdiğiniz `azuredeploy.parameters.json` olarak seçin.

    ![Kaynak Yöneticisi şablonu dağıtım oluşturmak için dosyaları seçin](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-create-deployment-select-files.png)

1. Tamam ' a tıkladıktan sonra dağıtım başlatılır. Dağıtım tamamlanana kadar, en altta IntelliJ fıkrın **durum çubuğundan** ilerlemeyi bulabilirsiniz.

    ![Kaynak Yöneticisi şablonu dağıtım durumu](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-create-deployment-status.png)

## <a name="browse-an-existing-deployment"></a>Mevcut bir dağıtıma gözatın

1. Dağıtım yapıldıktan sonra, yeni kaynak grubu `testRG` ve yeni bir dağıtım oluşturulur. Dağıtıma sağ tıklayın ve olası eylemlerin bir listesini görebilirsiniz. Şimdi **özellikleri göster**' i seçin.

    ![Kaynak Yöneticisi şablonu dağıtıma gözatmayı görüntüle](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-deployment-browse.png)

1. Bir sekme görünümü, dağıtım durumu ve şablon yapısı gibi bazı yararlı özellikleri göstermek için açılacak.

    ![Kaynak Yöneticisi şablonu dağıtım özelliklerini göster](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-deployment-show-properties.png)

## <a name="edit-and-update-an-existing-deployment"></a>Mevcut bir dağıtımı düzenleyin ve güncelleştirin

1. Sağ tıklama menüsünde **dağıtımı Düzenle** ' yi veya daha önce özellikleri göster görünümünü seçin. Azure üzerinde dağıtım için şablon ve parametre dosyalarını gösteren başka bir sekme görünümü açılır. Bu dosyaları yerel olarak kaydetmek için, **şablon dosyasını dışarı aktar** veya **parametre dosyalarını dışarı aktar**' a tıklayabilirsiniz.

    ![Kaynak Yöneticisi şablonu dağıtım düzenleme](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-edit-deployment.png)

1. Bu sayfadaki iki dosyayı düzenleyebilir ve değişiklikleri Azure 'a dağıtabilirsiniz. Burada, `Standard_LRS` ' den `Standard_GRS` ' ye kadar parametre dosyalarındaki **Storageaccounttype** değerini değiştirirsiniz. Ardından, alt kısımdaki **güncelleştirme dağıtımı** ' na tıklayın ve güncelleştirmeyi onaylayın.

    ![Kaynak Yöneticisi şablonu dağıtım düzenleme](./media/resource-manager-quickstart-create-templates-use-intellij/resource-manager-edit-deployment-update.png)

1. Güncelleştirme dağıtımı tamamlandıktan sonra, portalda oluşturulan depolama hesabının `Standard_GRS` olarak değiştirildiğini doğrulayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. Bunu Azure portal veya Azure CLı 'den yapabilirsiniz. Azure Explorer 'ın IntelliJ FIKRINDEN, oluşturduğunuz **kaynak grubunuza** sağ tıklayıp Sil ' i seçin.

    ![Azure Explorer 'da IntelliJ FIKRINDEN kaynak grubunu silme](./media/resource-manager-quickstart-create-templates-use-intellij/delete-resource-group.png)

> [!NOTE]
> Bir dağıtımı silme, dağıtım tarafından oluşturulan kaynakları silmeyecektir. Artık gerekmiyorsa, ilgili kaynak grubunu veya belirli kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç 'nin ana odağı, Azure hızlı başlangıç şablonlarından mevcut bir şablonu dağıtmak için IntelliJ FIKRINI kullanmaktır. Ayrıca, Azure 'da var olan bir dağıtımı görüntülemeyi ve güncelleştirmeyi de öğrendiniz. Azure hızlı başlangıç şablonlarından şablonlar, ihtiyacınız olan her şeyi sunmayabilir. Şablon geliştirme hakkında daha fazla bilgi edinmek için bkz. Yeni Başlangıç Öğreticisi serisi:

> [!div class="nextstepaction"]
> [Başlangıç öğreticileri](./template-tutorial-create-first-template.md)

> [!div class="nextstepaction"]
> [Azure Geliştirme Merkezi 'nde Java 'Yı ziyaret edin](https://docs.microsoft.com/azure/java)
