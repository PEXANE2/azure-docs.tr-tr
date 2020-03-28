---
title: Dağıtımlarla ilgili sorunları giderme
description: Azure Kaynak Yöneticisi şablon dağıtımlarını nasıl izleyeceğinizi ve sorun gidermeyi öğrenin. Etkinlik günlüklerini ve dağıtım geçmişini gösterir.
author: mumian
ms.date: 01/15/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 30b66414e87f642bc72b8723ebff57f2e9009f17
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239247"
---
# <a name="tutorial-troubleshoot-arm-template-deployments"></a>Öğretici: Sorun GiderME ARM şablon dağıtımları

Azure Kaynak Yöneticisi (ARM) şablon dağıtım hatalarını nasıl gidereceklerini öğrenin. Bu öğreticide, bir şablonda iki hata ayarlın ve sorunları gidermek için etkinlik günlüklerini ve dağıtım geçmişini nasıl kullanacağınızı öğrenirsiniz.

Şablon dağıtımıyla ilgili iki tür hata vardır:

- **Doğrulama hataları,** dağıtımdan önce belirlenebilen senaryolardan kaynaklansın. Bunlar şablonunuzdaki söz dizimi hataları veya abonelik kotalarınızı aşabilecek kaynak dağıtımı denemeleri olabilir.
- **Dağıtım hataları,** dağıtım işlemi sırasında oluşan koşullardan kaynaklanmalıdır. Bu paralel olarak dağıtılan bir kaynağa erişme denemesi olabilir.

Her iki tür hata da dağıtım sorunlarını gidermek için kullanabileceğiniz bir hata kodu döndürür. Her iki tür hata da etkinlik günlüğünde görüntülenir. Öte yandan doğrulama hataları dağıtım geçmişinizde görüntülenmez çünkü dağıtım hiç başlatılmamıştır.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Sorunlu bir şablon oluşturma
> * Sorun giderme doğrulama hataları
> * Dağıtım hatalarını giderme
> * Kaynakları temizleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)

## <a name="create-a-problematic-template"></a>Sorunlu bir şablon oluşturma

[Azure QuickStart Şablonlarından](https://azure.microsoft.com/resources/templates/) [standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) adlı bir şablon açın ve iki şablon sorunu oluşturun.

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. ApiVersion satırını aşağıdaki satırla **değiştirin:**

    ```json
    "apiVersion1": "2018-07-02",
    ```
    - **apiVersion1** geçersiz öğe adıdır. Bu bir doğrulama hatasıdır.
    - API sürümü "2018-07-01" olacaktır.  Bu bir dağıtım hatasıdır.

5. Dosyayı **azuredeploy.json** olarak yerel bilgisayarınıza kaydetmek için**Dosya Yı Kaydet'i** **seçin.**>

## <a name="troubleshoot-the-validation-error"></a>Doğrulama hatasını giderme

Şablonu dağıtmak için [şablonu dağıt](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) bölümüne bakın.

Aşağıdakilere benzer bir hata alırsınız:

```
New-AzResourceGroupDeployment : 4:29:24 PM - Error: Code=InvalidRequestContent; Message=The request content was invalid and could not be deserialized: 'Could not find member 'apiVersion1' on object of type 'TemplateResource'. Path 'properties.template.resources[0].apiVersion1', line 36, position 24.'.
```

Hata iletisi, sorunun **apiVersion1**ile olduğunu gösterir.

**ApiVersion1'i** **apiVersion**olarak değiştirerek sorunu düzeltmek için Visual Studio Code'u kullanın ve ardından şablonu kaydedin.

## <a name="troubleshoot-the-deployment-error"></a>Dağıtım hatasını giderme

Şablonu dağıtmak için [şablonu dağıt](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) bölümüne bakın.

Aşağıdakilere benzer bir hata alırsınız:

```
New-AzResourceGroupDeployment : 4:48:50 PM - Resource Microsoft.Storage/storageAccounts 'storeqii7x2rce77dc' failed with message '{
  "error": {
    "code": "NoRegisteredProviderFound",
    "message": "No registered resource provider found for location 'centralus' and API version '2018-07-02' for type 'storageAccounts'. The supported api-versions are '2018-07-01, 2018-03-01-preview, 2018-02-01, 2017-10-01, 2017-06-01, 2016-12-01, 2016-05-01, 2016-01-01, 2015-06-15, 2015-05-01-preview'. The supported locations are 'eastus, eastus2, westus, westeurope, eastasia, southeastasia, japaneast, japanwest, northcentralus, southcentralus, centralus, northeurope, brazilsouth, australiaeast, australiasoutheast, southindia, centralindia, westindia, canadaeast, canadacentral, westus2, westcentralus, uksouth, ukwest, koreacentral, koreasouth, francecentral'."
  }
}'
```

Dağıtım hatası Aşağıdaki yordamı kullanarak Azure portalından bulunabilir:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Kaynak gruplarını** ve ardından kaynak grup adını seçerek kaynak grubunu açın. **Dağıtım**altında **1 Başarısız'ı** göreceksiniz.

    ![Kaynak Yöneticisi öğretici sorun giderme](./media/template-tutorial-troubleshoot/resource-manager-template-deployment-error.png)
3. **Hata ayrıntılarını**seçin.

    ![Kaynak Yöneticisi öğretici sorun giderme](./media/template-tutorial-troubleshoot/resource-manager-template-deployment-error-details.png)

    Hata iletisi, daha önce gösterilenle aynıdır:

    ![Kaynak Yöneticisi öğretici sorun giderme](./media/template-tutorial-troubleshoot/resource-manager-template-deployment-error-summary.png)

Ayrıca, etkinlik günlüklerinden hatayı da bulabilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Etkinlik**günlüğünü** **İzle'yi** > seçin.
3. Günlüğü bulmak için filtreleri kullanın.

    ![Kaynak Yöneticisi öğretici sorun giderme](./media/template-tutorial-troubleshoot/resource-manager-template-deployment-activity-log.png)

Sorunu düzeltmek için Visual Studio Code'u kullanın ve ardından şablonu yeniden dağıtın.

Sık karşılaşılan hatalar listesi için Azure [Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](common-deployment-errors.md)bölümüne bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ARM şablon dağıtım hatalarını nasıl gidererini öğrendiniz.  Daha fazla bilgi için Azure [Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](common-deployment-errors.md)sorununa bakın.
