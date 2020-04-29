---
title: Bir uygulamayı başka bir bölgeye taşıma
description: App Service kaynaklarını bir bölgeden diğerine taşımayı öğrenin.
ms.topic: how-to
ms.date: 02/27/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 7e68f12ce062831ad361c88345188aca61922c4c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77925715"
---
# <a name="move-an-app-service-app-to-another-region"></a>App Service uygulamasını başka bir bölgeye taşıma

Bu makalede App Service kaynaklarının farklı bir Azure bölgesine nasıl taşınacağı açıklanır. Birkaç nedenden dolayı kaynaklarınızı başka bir bölgeye taşıyabilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanmak için, yalnızca belirli bölgelerde bulunan özellikleri veya hizmetleri dağıtmak, iç ilke ve idare gereksinimlerini karşılamak veya kapasite planlama gereksinimlerine yanıt vermek için.

App Service kaynaklar bölgeye özeldir ve bölgeler arasında taşınamaz. Hedef bölgede mevcut App Service kaynaklarınızın bir kopyasını oluşturmanız ve içeriğinizi yeni uygulamaya taşımanız gerekir. Kaynak uygulamanız özel bir etki alanı kullanıyorsa, işiniz bittiğinde [hedef bölgedeki yeni uygulamaya geçirebilirsiniz](manage-custom-dns-migrate-domain.md) .

Uygulamanızı daha kolay bir şekilde kopyalamak için, [tek bir App Service uygulamasını](app-service-web-app-cloning.md) başka bir bölgedeki bir App Service planına kopyalayabilirsiniz, ancak özellikle de Linux uygulamalarını desteklemeyen [kısıtlamalar](app-service-web-app-cloning.md#current-restrictions)vardır.

## <a name="prerequisites"></a>Ön koşullar

- App Service uygulamasının taşımak istediğiniz Azure bölgesinde olduğundan emin olun.
- Hedef bölgenin, kaynaklarını taşımak istediğiniz App Service ve ilgili hizmeti desteklediğinden emin olun.
<!-- - Domain bindings, certificates, and managed identities can't replicated using the **Export template** method. You must create them manually. -->

## <a name="prepare"></a>Hazırlama

Kullanmakta olduğunuz tüm App Service kaynaklarını belirler. Örneğin:

- App Service uygulamaları
- [App Service planları](overview-hosting-plans.md)
- [Dağıtım yuvaları](deploy-staging-slots.md)
- [Azure 'da satın alınan özel etki alanları](manage-custom-dns-buy-domain.md)
- [SSL sertifikaları](configure-ssl-certificate.md)
- [Azure sanal ağ tümleştirmesi](web-sites-integrate-with-vnet.md)
- [Karma bağlantılar](app-service-hybrid-connections.md).
- [Yönetilen kimlikler](overview-managed-identity.md)
- [Yedekleme ayarları](manage-backup.md)

İçeri aktarılan sertifikalar veya karma bağlantılar gibi belirli kaynaklar, diğer Azure hizmetleriyle tümleştirmeyi içerir. Bu kaynakları bölgeler arasında taşıma hakkında daha fazla bilgi için ilgili hizmetlere yönelik belgelere bakın.

## <a name="move"></a>Taşı

1. [Kaynak uygulamanın yedeklemesini oluşturun](manage-backup.md).
1. [Hedef bölgede yeni bir App Service planında bir uygulama oluşturun](app-service-plan-manage.md#create-an-app-service-plan).
2. [Hedef uygulamada yedeklemeyi geri yükleme](web-sites-restore.md)
2. Özel bir etki alanı kullanıyorsanız, preemptively ile `awverify.` [hedef uygulamaya bağlayın](manage-custom-dns-migrate-domain.md#bind-the-domain-name-preemptively) ve [Hedef uygulamadaki etki alanını etkinleştirin](manage-custom-dns-migrate-domain.md#enable-the-domain-for-your-app).
3. Hedef uygulamanızdaki diğer her şeyi, kaynak uygulamayla aynı olacak şekilde yapılandırın ve yapılandırmanızı doğrulayın.
4. Özel etki alanı için hedef uygulamayı işaret etmeye hazırsanız, [etki alanı adını yeniden eşleyin](manage-custom-dns-migrate-domain.md#remap-the-active-dns-name).

<!-- 1. Login to the [Azure portal](https://portal.azure.com) > **Resource Groups**.
2. Locate the Resource Group that contains the source App Service resources and click on it.
3. Select > **Settings** > **Export template**.
4. Choose **Deploy** in the **Export template** blade.
5. Click **TEMPLATE** > **Edit template** to open the template in the online editor.
6. Click inside the online editor and type Ctrl+F (or ⌘+F on a Mac) and type `"identity": {` to find any managed identity definition. The following is an example if you have a user-assigned managed identity.
    ```json
    "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/<group-name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<identity-name>": {
                "principalId": "00000000-0000-0000-0000-000000000000",
                "clientId": "00000000-0000-0000-0000-000000000000"
            }
        }
    },
    ```
6. Click inside the online editor and type Ctrl+F (or ⌘+F on a Mac) and type `"Microsoft.Web/sites/hostNameBindings` to find all hostname bindings. The following is an example if you have a user-assigned managed identity.
    ```json
    {
        "type": "Microsoft.Web/sites/hostNameBindings",
        "apiVersion": "2018-11-01",
        "name": "[concat(parameters('sites_webapp_name'), '/', parameters('sites_webapp_name'), '.azurewebsites.net')]",
        "location": "West Europe",
        "dependsOn": [
            "[resourceId('Microsoft.Web/sites', parameters('sites_webapp_name'))]"
        ],
        "properties": {
            "siteName": "<app-name>",
            "hostNameType": "Verified"
        }
    },
    ```
6. Click inside the online editor and type Ctrl+F (or ⌘+F on a Mac) and type `"Microsoft.Web/certificates` to find all hostname bindings. The following is an example if you have a user-assigned managed identity.
    ```json
    {
        "type": "Microsoft.Web/certificates",
        "apiVersion": "2018-11-01",
        "name": "[parameters('certificates_test2_cephaslin_com_name')]",
        "location": "West Europe",
        "properties": {
            "hostNames": [
                "[parameters('certificates_test2_cephaslin_com_name')]"
            ],
            "password": "[parameters('certificates_test2_cephaslin_com_password')]"
        }
    },
    ```
7. Delete the entire JSON block. Click **Save** in the online editor.
8. Click **BASICS** > **Create new** to create a new resource group. Type the group name and click **OK**.
9. In **BASICS** > **Location**, select the region you want.   -->

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını Temizleme

Kaynak uygulamayı ve App Service planı silin. [Ücretsiz olmayan katmanda bir App Service planı, üzerinde hiçbir uygulama çalışmadığı halde bir ücret taşır.](app-service-plan-manage.md#delete-an-app-service-plan)

## <a name="next-steps"></a>Sonraki adımlar

[PowerShell kullanarak uygulama kopyalamayı Azure App Service](app-service-web-app-cloning.md)