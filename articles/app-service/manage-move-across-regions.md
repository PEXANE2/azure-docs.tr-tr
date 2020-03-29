---
title: Uygulamayı başka bir bölgeye taşıma
description: Uygulama Hizmeti kaynaklarını bir bölgeden diğerine nasıl taşıyabildiğini öğrenin.
ms.topic: how-to
ms.date: 02/27/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 7e68f12ce062831ad361c88345188aca61922c4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77925715"
---
# <a name="move-an-app-service-app-to-another-region"></a>Uygulama Hizmeti uygulamasını başka bir bölgeye taşıma

Bu makalede, Uygulama Hizmeti kaynaklarının nasıl farklı bir Azure bölgesine taşınır. Kaynaklarınızı birkaç nedenden dolayı başka bir bölgeye taşıyabilirsiniz. Örneğin, yeni bir Azure bölgesinden yararlanmak, yalnızca belirli bölgelerde kullanılabilen özellikleri veya hizmetleri dağıtmak, iç ilke ve yönetim gereksinimlerini karşılamak veya kapasite planlama gereksinimlerine yanıt olarak.

Uygulama Hizmeti kaynakları bölgeye özgür ve bölgeler arasında taşınamamaktadır. Hedef bölgedeki mevcut Uygulama Hizmeti kaynaklarınızın bir kopyasını oluşturmalı ve içeriğinizi yeni uygulamaya taşımalısınız. Kaynak uygulamanız özel bir etki alanı kullanıyorsa, işiniz bittiğinde [bu uygulamayı hedef bölgedeki yeni uygulamaya geçirebilirsiniz.](manage-custom-dns-migrate-domain.md)

Uygulamanızı kopyalamayı kolaylaştırmak için, [tek bir Uygulama Hizmeti uygulamasını](app-service-web-app-cloning.md) başka bir bölgedeki bir Uygulama Hizmeti planına kopyalayabilirsiniz, ancak özellikle Linux uygulamalarını desteklemediği gibi [sınırlamaları](app-service-web-app-cloning.md#current-restrictions)vardır.

## <a name="prerequisites"></a>Ön koşullar

- Uygulama Hizmeti uygulamasının taşınmak istediğiniz Azure bölgesinde olduğundan emin olun.
- Hedef bölgenin, kaynaklarını taşımak istediğiniz App Service'i ve ilgili hizmetleri desteklediğinden emin olun.
<!-- - Domain bindings, certificates, and managed identities can't replicated using the **Export template** method. You must create them manually. -->

## <a name="prepare"></a>Hazırlama

Şu anda kullanmakta olduğunuz tüm Uygulama Hizmeti kaynaklarını tanımlayın. Örnek:

- App Service uygulamaları
- [Uygulama Hizmeti planları](overview-hosting-plans.md)
- [Dağıtım yuvaları](deploy-staging-slots.md)
- [Azure'da satın alınan özel etki alanları](manage-custom-dns-buy-domain.md)
- [SSL sertifikaları](configure-ssl-certificate.md)
- [Azure Sanal Ağ entegrasyonu](web-sites-integrate-with-vnet.md)
- [Hibrit bağlantılar.](app-service-hybrid-connections.md)
- [Yönetilen kimlikler](overview-managed-identity.md)
- [Yedekleme ayarları](manage-backup.md)

İçe aktarılan sertifikalar veya karma bağlantılar gibi belirli kaynaklar, diğer Azure hizmetleriyle tümleştirme içerir. Bu kaynakların bölgeler arasında nasıl taşınacakla ilgili bilgi için ilgili hizmetlerin belgelerine bakın.

## <a name="move"></a>Taşı

1. [Kaynak uygulamanın yedeklerini oluşturun.](manage-backup.md)
1. [Hedef bölgede yeni bir Uygulama Hizmeti planında bir uygulama oluşturun.](app-service-plan-manage.md#create-an-app-service-plan)
2. [Hedef uygulamasında yedeklemeyi geri yükleme](web-sites-restore.md)
2. Özel bir etki alanı kullanıyorsanız, hedef `awverify.` uygulamaya [önceden bağlayıp](manage-custom-dns-migrate-domain.md#bind-the-domain-name-preemptively) hedef [uygulamadaki etki alanını etkinleştirin.](manage-custom-dns-migrate-domain.md#enable-the-domain-for-your-app)
3. Hedef uygulamanızdaki diğer her şeyi kaynak uygulamayla aynı olacak şekilde yapılandırın ve yapılandırmanızı doğrulayın.
4. Özel etki alanının hedef uygulamayı işaret etmesi için hazır olduğunuzda, [etki alanı adını yeniden eşleştirin.](manage-custom-dns-migrate-domain.md#remap-the-active-dns-name)

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

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını temizleme

Kaynak uygulamayı ve Uygulama Hizmeti planını silin. [Ücretsiz olmayan katmandaki bir Uygulama Hizmeti planı, içinde hiçbir uygulama çalışmasa bile bir ücret taşır.](app-service-plan-manage.md#delete-an-app-service-plan)

## <a name="next-steps"></a>Sonraki adımlar

[PowerShell kullanarak Azure Uygulama Hizmeti Uygulaması Klonlama](app-service-web-app-cloning.md)