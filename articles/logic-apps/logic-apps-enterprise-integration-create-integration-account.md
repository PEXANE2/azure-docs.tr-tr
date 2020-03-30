---
title: B2B tümleştirme hesapları oluşturma veya yönetme
description: Azure Logic Apps ile kurumsal tümleştirme için entegrasyon hesapları oluşturma, bağlantı kurma ve yönetme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: 083ed0001adb5524c124295eb3bc31f4afad99cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270335"
---
# <a name="create-and-manage-integration-accounts-for-b2b-enterprise-integrations-in-azure-logic-apps"></a>Azure Logic Apps'ta B2B kurumsal tümleştirmeleri için entegrasyon hesapları oluşturma ve yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) kullanarak [kurumsal tümleştirme ve B2B çözümleri](../logic-apps/logic-apps-enterprise-integration-overview.md) oluşturabilmek için önce bir tümleştirme hesabı oluşturmanız gerekir. Bu hesap, mantıksal uygulama iş akışlarınızla tanımladığınız ve kullandığınız tümleştirme yapıları için güvenli, ölçeklenebilir ve yönetilebilir bir kapsayıcı sağlayan ayrı bir Azure kaynağıdır.

Örneğin, ticaret ortakları, anlaşmalar, haritalar, şemalar, sertifikalar ve toplu iş yapılandırmaları gibi B2B yapılarını oluşturabilir, depolayabilir ve yönetebilirsiniz. Ayrıca, mantık uygulamanız bu yapılarla çalışabilmesi ve Logic Apps B2B konektörlerini kullanmadan önce entegrasyon hesabınızı mantık uygulamanıza [bağlamanız](#link-account) gerekir. Hem entegrasyon hesabınız hem de mantık uygulamanız *aynı* konumda veya bölgede bulunmalıdır.

> [!TIP]
> [Bir tümleştirme hizmeti ortamında](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)bir tümleştirme hesabı oluşturmak için bkz. [Create integration accounts in an ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#create-integration-account-environment)

Bu konu, bu görevleri nasıl gerçekleştireceklerini gösterir:

* Entegrasyon hesabınızı oluşturun.
* Entegrasyon hesabınızı bir mantık uygulamasına bağla.
* Entegrasyon hesabınız için fiyatlandırma katmanını değiştirin.
* Entegrasyon hesabınızı bir mantık uygulamasından açın.
* Tümleştirme hesabınızı başka bir Azure kaynak grubuna veya aboneye taşıyın.
* Tümleştirme hesabınızı silin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

## <a name="create-integration-account"></a>Tümleştirme hesabı oluşturma

Bu görev için, bu bölümdeki adımları, [Azure PowerShell'i](/powershell/module/Az.LogicApp/New-AzIntegrationAccount)veya [Azure CLI'yi](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-create)izleyerek Azure portalını kullanabilirsiniz.

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure ana menüsünde **Kaynak oluştur**'u seçin. Arama kutusuna, filtreniz olarak "entegrasyon hesabı" girin ve **Tümleştirme Hesabı'nı**seçin.

   ![Yeni tümleştirme hesabı oluşturma](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

1. **Entegrasyon Hesabı**altında, **Oluştur'u**seçin.

   ![Entegrasyon hesabı oluşturmak için "Ekle"yi seçin](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

1. Entegrasyon hesabınız la ilgili şu bilgileri sağlayın:

   ![Entegrasyon hesabı ayrıntılarını sağlama](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Adı** | Evet | <*tümleştirme-hesap-adı*> | Tümleştirme hesabınızın yalnızca harfleri, sayıları, tireleri (`-`), alt`_`tümleçleri`(` `)`( ),`.`parantez ( , ), ve dönemleri ( ) içerebilen adı . Bu örnekte "Fabrikam-Entegrasyon" kullanabı vardır. |
   | **Abonelik** | Evet | <*Azure abonelik adı*> | Azure aboneliğinizin adı |
   | **Kaynak grubu** | Evet | <*Azure-kaynak grubu adı*> | İlgili kaynakları düzenlemek için kullanılacak [Azure kaynak grubunun](../azure-resource-manager/management/overview.md) adı. Bu örnekte, "FabrikamIntegration-RG" adında yeni bir kaynak grubu oluşturun. |
   | **Fiyatlandırma Katmanı** | Evet | <*fiyatlandırma düzeyi*> | Daha sonra değiştirebileceğiniz tümleştirme hesabının fiyatlandırma katmanı. Bu **örnekiçin, Ücretsiz'i**seçin. Daha fazla bilgi için şu konulara bakın: <p>- [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md#integration-accounts) <p>- [Logic Apps sınırları ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) <p>- [Mantık Uygulamaları fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/) |
   | **Konum** | Evet | <*Azure bölgesi*> | Entegrasyon hesabı meta verilerinizin depolandığı bölge. Mantık uygulamanızla aynı konumu seçin veya mantık uygulamalarınızı entegrasyon hesabınızla aynı konumda oluşturun. Bu örnekiçin "Batı ABD"yi kullanın. <p>**Not**: Bir [entegrasyon hizmet ortamı (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)içinde bir entegrasyon hesabı oluşturmak için, konum olarak O İmKB'yi seçin. Daha fazla bilgi [Create integration accounts in an ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#create-integration-account-environment)için bk. |
   | **Log Analytics** | Hayır | Kapalı, Kapalı | Bu örnek için **Kapalı ayarını** koruyun. |
   |||||

1. Bitirdikten sonra **Oluştur'u**seçin.

   Dağıtım tamamlandıktan sonra Azure tümleştirme hesabınızı açar.

   ![Azure entegrasyon hesabını açtı](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

1. Mantık uygulamanız entegrasyon hesabınızı kullanmadan önce, entegrasyon hesabınızı ve mantık uygulamanızı birbirine bağlamak için sonraki adımları izleyin.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Mantıksal uygulama ile bağlantı oluşturma

Mantık uygulamalarınızın B2B yapılarınızı içeren bir entegrasyon hesabına erişmesi için öncelikle entegrasyon hesabınızı mantık uygulamanıza bağlamanız gerekir. Hem mantık uygulaması hem de tümleştirme hesabı aynı bölgede bulunmalıdır. Bu görevi tamamlamak için Azure portalını kullanabilirsiniz. Visual Studio kullanıyorsanız ve mantık uygulamanız bir [Azure Kaynak Grubu projesindeyse,](../azure-resource-manager/templates/create-visual-studio-deployment-project.md)Visual [Studio'u kullanarak mantık uygulamanızı bir entegrasyon hesabına](../logic-apps/manage-logic-apps-with-visual-studio.md#link-integration-account)bağlayabilirsiniz.

1. Azure portalında mantık uygulamanızı bulun ve açın.

1. Azure [portalında,](https://portal.azure.com)varolan bir mantık uygulamasını açın veya yeni bir mantık uygulaması oluşturun.

1. Mantık uygulamanızın menüsünde Ayarlar **altında** **İş Akışı ayarlarını**seçin. **Tümleştirme hesabı**altında, **Tümleştirme hesabı** seç listesini açın. Mantık uygulamanıza bağlanmak için entegrasyon hesabını seçin.

   ![Entegrasyon hesabınızı seçin](./media/logic-apps-enterprise-integration-create-integration-account/select-integration-account.png)

1. Bağlamayı bitirmek için **Kaydet'i**seçin.

   ![Entegrasyon hesabınızı seçin](./media/logic-apps-enterprise-integration-create-integration-account/save-link.png)

   Tümleştirme hesabınız başarıyla bağlandıktan sonra Azure bir onay iletisi gösterir.

   ![Azure başarılı bağlantıyı doğruladı](./media/logic-apps-enterprise-integration-create-integration-account/link-confirmation.png)

Artık mantık uygulamanız, entegrasyon hesabınızdaki yapıları ve XML doğrulama ve düz dosya kodlama veya kod çözme gibi B2B bağlayıcılarını kullanabilir.  

<a name="change-pricing-tier"></a>

## <a name="change-pricing-tier"></a>Fiyatlandırma katmanını değiştirme

Bir tümleştirme hesabının [sınırlarını](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) artırmak için, varsa [daha yüksek bir fiyatlandırma katmanına yükseltebilirsiniz.](#upgrade-pricing-tier) Örneğin, Serbest katmandan Temel katmana veya Standart katmana yükseltebilirsiniz. Ayrıca, varsa [daha düşük bir katmana da düşürebilirsiniz.](#downgrade-pricing-tier) Daha fazla fiyatlandırma bilgisi için şu konulara bakın:

* [Mantık Uygulamaları fiyatlandırma](https://azure.microsoft.com/pricing/details/logic-apps/)
* [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md#integration-accounts)

<a name="upgrade-pricing-tier"></a>

### <a name="upgrade-pricing-tier"></a>Yükseltme fiyatlandırma katmanı

Bu değişikliği yapmak için, bu bölümdeki adımları izleyerek Azure portalını veya [Azure CLI'yi](#upgrade-tier-azure-cli)kullanabilirsiniz.

#### <a name="azure-portal"></a>Azure portalında

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Ana Azure arama kutusuna, filtreniz olarak "tümleştirme hesapları"nı girin ve **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesabını bulma](./media/logic-apps-enterprise-integration-create-integration-account/find-integration-account.png)

   Azure, Azure aboneliklerinizdeki tüm tüm tümleştirme hesaplarını gösterir.

1. **Tümleştirme hesapları**altında, taşımak istediğiniz tümleştirme hesabını seçin. Entegrasyon hesabı menüsünde **Genel Bakış'ı**seçin.

   ![Tümleştirme hesabı menüsünde "Genel Bakış"ı seçin](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-overview.png)

1. Genel Bakış bölmesinde, kullanılabilir yüksek katmanları listeleyen **Yükseltme fiyatlandırma katmanını**seçin. Bir katman seçtiğinizde, değişiklik hemen etkine geçer.

<a name="upgrade-tier-azure-cli"></a>

#### <a name="azure-cli"></a>Azure CLI

1. Bunu daha önce yapmadıysanız, [Azure CLI ön koşulları yükleyin.](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)

1. Azure portalında Azure [**Bulut Bulutu**](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) ortamını açın.

   ![Azure Cloud Shell’i açma](./media/logic-apps-enterprise-integration-create-integration-account/open-azure-cloud-shell-window.png)

1. Komut isteminde az [ **kaynak** komutunu](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-update)girin `skuName` ve istediğiniz daha yüksek katmana ayarlayın.

   ```azurecli
   az resource update --resource-group {ResourceGroupName} --resource-type Microsoft.Logic/integrationAccounts --name {IntegrationAccountName} --subscription {AzureSubscriptionID} --set sku.name={SkuName}
   ```
  
   Örneğin, Temel katmanınız varsa, aşağıdakileri `skuName` `Standard`ayarlayabilirsiniz:

   ```azurecli
   az resource update --resource-group FabrikamIntegration-RG --resource-type Microsoft.Logic/integrationAccounts --name Fabrikam-Integration --subscription XXXXXXXXXXXXXXXXX --set sku.name=Standard
   ```

<a name="downgrade-pricing-tier"></a>

### <a name="downgrade-pricing-tier"></a>Düşürme fiyatlandırma katmanı

Bu değişikliği yapmak için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanın.

1. Bunu daha önce yapmadıysanız, [Azure CLI ön koşulları yükleyin.](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)

1. Azure portalında Azure [**Bulut Bulutu**](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) ortamını açın.

   ![Azure Cloud Shell’i açma](./media/logic-apps-enterprise-integration-create-integration-account/open-azure-cloud-shell-window.png)

1. Komut isteminde az [ **kaynak** komutunu](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-update) `skuName` girin ve istediğiniz alt katmana ayarlayın.

   ```azurecli
   az resource update --resource-group <resourceGroupName> --resource-type Microsoft.Logic/integrationAccounts --name <integrationAccountName> --subscription <AzureSubscriptionID> --set sku.name=<skuName>
   ```
  
   Örneğin, Standart katmana sahipseniz, `skuName` aşağıdakileri `Basic`ayarlayabilirsiniz:

   ```azurecli
   az resource update --resource-group FabrikamIntegration-RG --resource-type Microsoft.Logic/integrationAccounts --name Fabrikam-Integration --subscription XXXXXXXXXXXXXXXXX --set sku.name=Basic
   ```

## <a name="unlink-from-logic-app"></a>Mantıksal uygulama bağlantısını kaldırma

Mantık uygulamanızı başka bir tümleştirme hesabına bağlamak istiyorsanız veya artık mantık uygulamanızla bir entegrasyon hesabı kullanmıyorsanız, Azure Kaynak Gezgini'ni kullanarak bağlantıyı silin.

1. Tarayıcı pencerenizi açın ve [Azure Kaynakhttps://resources.azure.com)Gezgini'ne](https://resources.azure.com)gidin ( . Aynı Azure hesap kimlik bilgileriyle oturum açın.

   ![Azure Resource Manager](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

1. Arama kutusuna, mantık uygulamanızı bulup seçebilmeniz için mantık uygulamanızın adını girin.

   ![Mantık uygulamasını bulma ve seçme](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

1. Explorer başlık çubuğunda **Oku/Yaz'ı**seçin.

   !["Okuma/Yazma" modunu açma](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-select-read-write.png)

1. **Veriler** sekmesinde, **Edit'i**seçin.

   !["Veriler" sekmesinde "Edit" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-select-edit.png)

1. Düzenleyicide, nesneyi `integrationAccount` bulun ve bu biçime sahip olan özelliği silin:

   ```json
   {
      // <other-attributes>
      "integrationAccount": {
         "name": "<integration-account-name>",
         "id": "<integration-account-resource-ID>",
         "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Örnek:

   !["integrationAccount" nesnesini bulma](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

1. **Veriler** sekmesinde, değişikliklerinizi kaydetmek için **Put'u** seçin.

   ![Değişiklikleri kaydetmek için "Put" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

1. Azure portalında mantık uygulamanızı bulun ve seçin. Uygulamanızın İş **Akışı ayarları**altında, **Tümleştirme hesabı** özelliğinin artık boş görünün.

   ![Tümleştirme hesabının bağlantılı olup olmadığını kontrol edin](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Tümleştirme hesabını taşıma

Tümleştirme hesabınızı başka bir Azure kaynak grubuna veya Azure aboneliğine taşıyabilirsiniz. Kaynakları taşıdığınızda, Azure yeni kaynak tanımları oluşturur, bu nedenle bunun yerine yeni iT'leri kullandığınızdan ve taşınan kaynaklarla ilişkili komut dosyalarını veya araçları güncelleştirdiğinizden emin olun. Aboneliği değiştirmek istiyorsanız, varolan veya yeni bir kaynak grubu da belirtmeniz gerekir.

Bu görev için, bu bölümdeki adımları izleyerek Azure portalını veya [Azure CLI'yi](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-move)kullanabilirsiniz.

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Ana Azure arama kutusuna, filtreniz olarak "tümleştirme hesapları"nı girin ve **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesabını bulma](./media/logic-apps-enterprise-integration-create-integration-account/find-integration-account.png)

   Azure, Azure aboneliklerinizdeki tüm tüm tümleştirme hesaplarını gösterir.

1. **Tümleştirme hesapları**altında, taşımak istediğiniz tümleştirme hesabını seçin. Entegrasyon hesabı menüsünde **Genel Bakış'ı**seçin.

   ![Tümleştirme hesabı menüsünde "Genel Bakış"ı seçin](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-overview.png)

1. **Kaynak grubu** veya **Abonelik adının**yanında **değiştir'i**seçin.

   ![Kaynak grubunu veya aboneliği değiştirme](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

1. Taşımak istediğiniz ilgili kaynakları seçin.

1. Seçiminizi temel alan kaynak grubunu veya aboneliği değiştirmek için aşağıdaki adımları izleyin:

   * Kaynak grubu: **Kaynak grubu** listesinden hedef kaynak grubunu seçin. Veya farklı bir kaynak grubu oluşturmak için **yeni bir kaynak grubu oluştur'u**seçin.

   * Abonelik: **Abonelik** listesinden hedef aboneliği seçin. Kaynak **grup** listesinden hedef kaynak grubunu seçin. Veya farklı bir kaynak grubu oluşturmak için **yeni bir kaynak grubu oluştur'u**seçin.

1. Taşınan kaynaklarla ilişkili komut dosyalarının veya araçlarının, yeni kaynak iT'leriyle güncelleştirene kadar çalışmayacağını anlamak için onay kutusunu seçin ve **ardından Tamam'ı**seçin.

1. İşleminizi tamamladıktan sonra, taşınan kaynaklarınız için yeni kaynak tanımlarıyla tüm komut dosyalarını güncelleştirdiğinizden emin olun.  

## <a name="delete-integration-account"></a>Tümleştirme hesabını silme

Bu görev için, bu bölümdeki adımları, [Azure CLI'yi](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-delete)veya [Azure PowerShell'i](/powershell/module/az.logicapp/remove-azintegrationaccount)izleyerek Azure portalını kullanabilirsiniz.

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Ana Azure arama kutusuna, filtreniz olarak "tümleştirme hesapları"nı girin ve **Tümleştirme hesaplarını**seçin.

   ![Tümleştirme hesabını bulma](./media/logic-apps-enterprise-integration-create-integration-account/find-integration-account.png)

   Azure, Azure aboneliklerinizdeki tüm tüm tümleştirme hesaplarını gösterir.

1. **Tümleştirme hesapları**altında, silmek istediğiniz tümleştirme hesabını seçin. Entegrasyon hesabı menüsünde **Genel Bakış'ı**seçin.

   ![Tümleştirme hesabı menüsünde "Genel Bakış"ı seçin](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-overview.png)

1. Genel Bakış bölmesine Sil'i **seçin.**

   !["Genel Bakış" bölmesinde "Sil" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

1. Entegrasyon hesabınızı silmek istediğinizi doğrulamak için **Evet'i**seçin.

   ![Silmeyi onaylamak için "Evet" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Entegrasyon hesabınızda ticaret ortakları oluşturun](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Entegrasyon hesabınızdaki iş ortakları arasında anlaşmalar oluşturma](../logic-apps/logic-apps-enterprise-integration-agreements.md)
