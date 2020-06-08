---
title: Hızlı başlangıç-Azure CLı kullanarak bir Azure Notification Hub 'ı oluşturma | Microsoft Docs
description: Bu öğreticide, Azure CLı kullanarak bir Azure Notification Hub 'ı oluşturmayı öğreneceksiniz.
services: notification-hubs
author: dbradish-microsoft
manager: barbkess
editor: sethmanheim
ms.service: notification-hubs
ms.devlang: azurecli
ms.workload: mobile
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: dbradish
ms.reviewer: sethm
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: f39d5ca5e153da3d1644aabeb7e48b41d07fe253
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84485139"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure Notification Hub 'ı oluşturma

Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmet hakkında daha fazla bilgi için bkz. [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

Bu hızlı başlangıçta, Azure CLı kullanarak bir Bildirim Hub 'ı oluşturursunuz. İlk bölüm, size bir Bildirim Hub 'ı ad alanı oluşturma adımları sağlar.  İkinci bölüm, mevcut bir ad alanında Bildirim Hub 'ı oluşturma adımları sağlar.  Ayrıca, özel erişim ilkesi oluşturmayı öğreneceksiniz.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Notification Hubs, Azure CLı 'nin sürüm 2.0.67 veya üstünü gerektirir. `az --version`Yüklenen sürümü ve bağımlı kitaplıkları bulmak için ' i çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

1. Giriş yapın.

   CLı 'nın yerel bir yüklemesini kullanıyorsanız [az Login](/cli/azure/reference-index#az-login) komutunu kullanarak oturum açın.

    ```azurecli
    az login
    ```

    Kimlik doğrulama işlemini gerçekleştirmek için terminalinizde görünen adımları izleyin.

2. Azure CLI uzantısını yükleyin.

   Azure CLı için uzantı başvurularıyla çalışırken, önce uzantıyı yüklemeniz gerekir.  Azure CLı uzantıları, henüz çekirdek CLı 'nin bir parçası olarak gönderilmeyen deneysel ve yayın öncesi komutlara erişmenizi sağlar.  Güncelleştirme ve kaldırma dahil olmak üzere uzantılar hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).

   Aşağıdaki komutu çalıştırarak [Notification Hubs uzantısını](/cli/azure/ext/notification-hub/notification-hub) yükler:

    ```azurecli
    az extension add --name notification-hub
   ```

3. Bir kaynak grubu oluşturun.

   Azure Notification Hubs, tüm Azure kaynakları gibi bir kaynak grubuna dağıtılmalıdır. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

   Bu hızlı başlangıç için, _spnhubrg_ adlı bir kaynak grubunu aşağıdaki [az Group Create](/cli/azure/group#az-group-create) komutuyla _eastus_ konumunda oluşturun:

   ```azurecli
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Bildirim Hub 'ı ad alanı oluşturma

1. Bildirim Hub 'larınız için bir ad alanı oluşturun.

   Bir ad alanı bir veya daha fazla hub içerir ve **ad tüm Azure abonelikleri genelinde benzersiz olmalı ve en az altı karakter uzunluğunda**olmalıdır.  Bir adın kullanılabilirliğini denetlemek için [az Notification-Hub ad alanı Check-AVAILABILITY](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability) komutunu kullanın.

   ```azurecli
   az notification-hub namespace check-availability --name spnhubns
   ```

   Azure CLı, aşağıdaki konsol çıkışını görüntüleyerek kullanım isteğinize yanıt verir:

   ```output
   {
   "id": "/subscriptions/yourSubscriptionID/providers/Microsoft.NotificationHubs/checkNamespaceAvailability",
   "isAvailiable": true,
   "location": null,
   "name": "spnhubns",
   "properties": false,
   "sku": null,
   "tags": null,
   "type": "Microsoft.NotificationHubs/namespaces/checkNamespaceAvailability"
   }
   ```

   Azure CLı yanıtının ikinci satırına dikkat edin `"isAvailable": true` .  Bu satır `false` , ad alanı için belirttiğiniz istenen ad varsa okur.  Adın kullanılabilirliğini onayladıktan sonra, ad alanınızı oluşturmak için [az Notification-Hub ad alanı oluştur](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) komutunu çalıştırın.  

   ```azurecli
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

   `--name` `az notification-hub namespace create` Komuta sağlanmışsa veya [Azure kaynakları için adlandırma kurallarını ve kısıtlamalarını](/azure/azure-resource-manager/management/resource-name-rules)karşılamıyorsa, Azure CLI aşağıdaki konsol çıkışıyla yanıt verir:

   ```output
   #the name is not available
   The specified name is not available. For more information visit https://aka.ms/eventhubsarmexceptions.

   #the name is invalied
   The specified service namespace is invalid.
   ```

   Denediğiniz ilk ad başarılı olmazsa, yeni ad alanınız için farklı bir ad seçin ve `az notification-hub namespace create` komutu yeniden çalıştırın.

   > [!NOTE]
   > Bu adım ileriye doğru, `--namespace` Bu hızlı başlangıçta kopyaladığınız her bir Azure CLI komutunda parametresinin değerini değiştirmeniz gerekir.

2. Ad alanlarının bir listesini alın.

   Yeni ad alanınız hakkındaki ayrıntıları görmek için [az Notification-Hub Namespace List](/cli/azure/ext/notification-hub/notification-hub/namespace?view=azure-cli-latest#ext-notification-hub-az-notification-hub-namespace-list) komutunu kullanın.  `--resource-group`Bir abonelik için tüm ad alanlarını görmek istiyorsanız parametresi isteğe bağlıdır.

   ```azurecli
   az notification-hub namespace list --resource-group spnhubrg
   ```

## <a name="create-notification-hubs"></a>Bildirim Hub 'ları oluşturma

1. İlk Bildirim Hub 'ınızı oluşturun.

   Yeni ad alanında bir veya daha fazla Bildirim Hub 'ı artık oluşturulabilir.  Bir Bildirim Hub 'ı oluşturmak için [az Notification-Hub Create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) komutunu çalıştırın.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. İkinci bir Bildirim Hub 'ı oluşturun.

   Tek bir ad alanında birden çok Bildirim Hub 'ı oluşturulabilir.  Aynı ad alanında ikinci bir Bildirim Hub 'ı oluşturmak için, `az notification-hub create` farklı bir hub adı kullanarak komutu yeniden çalıştırın.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

3. Bildirim Hub 'larının listesini alın.

   Azure CLı, yürütülen her komutla bir başarı veya hata iletisi döndürür; Bununla birlikte, Bildirim Hub 'larının bir listesini sorgulayabilmek yeniden kullanılır.  [Az Notification-Hub List](/cli/azure/ext/notification-hub/notification-hub?view=azure-cli-latest#ext-notification-hub-az-notification-hub-list) komutu bu amaçla tasarlanmıştı.

   ```azurecli
   az notification-hub list --resource-group spnhubrg --namespace-name spnhubns --output table
   ```

## <a name="work-with-notification-hub-access-policies"></a>Notification Hub 'ı erişim ilkeleriyle çalışma

1. Bir Bildirim Hub 'ı için erişim ilkelerini listeleyin.

   Azure Notification Hubs, erişim ilkelerinin kullanımı ile [paylaşılan erişim imzası güvenliği](/azure/notification-hubs/notification-hubs-push-notification-security) kullanır.  Bir Bildirim Hub 'ı oluşturduğunuzda iki ilke otomatik olarak oluşturulur.  Bu ilkelerdeki bağlantı dizeleri anında iletme bildirimlerini yapılandırmak için gereklidir.  [Az Notification-Hub Authorization-Rule List](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) komutu ilke adlarının ve bunlara ait kaynak gruplarının bir listesini sağlar.

   ```azurecli
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > Uygulamanızda _Defaultfullsharedaccesssignature_ ilkesini kullanmayın. Bunu yalnızca arka ucunuzda kullanmanız gerekir.  `Listen`İstemci uygulamanızda yalnızca erişim ilkelerini kullanın.

2. Bir Bildirim Hub 'ı için yeni bir yetkilendirme kuralı oluşturun.

   Anlamlı adlarla ek yetkilendirme kuralları oluşturmak isterseniz, [az Notification-Hub Authorization-Rule Create](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create) komutunu kullanarak kendi erişim ilkenizi oluşturup özelleştirebilirsiniz.  `--rights`Parametresi, atamak istediğiniz izinlerin boşlukla ayrılmış bir listesidir.

   ```azurecli
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Manage Send
   ```

3. Bir Bildirim Hub 'ı erişim ilkesi için anahtarları ve bağlantı dizelerini listeleyin

   Her erişim ilkesi için iki anahtar ve bağlantı dizesi kümesi vardır.  Daha sonra [bir Bildirim Hub 'ı yapılandırmak](/azure/notification-hubs/configure-notification-hub-portal-pns-settings)için bunlara ihtiyacınız olacaktır.  Bir Bildirim Hub 'ı erişim ilkesi için anahtar ve bağlantı dizelerini listelemek için, [az Notification-Hub Authorization-Rule List-Keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu kullanın.

   ```azurecli
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output table
   ```

   ```azurecli
   #query the keys and connection strings for a custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > Bir [Bildirim Hub 'ı ad alanı](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) ve [Bildirim Hub 'ının](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) ayrı erişim ilkeleri vardır.  Anahtarlar ve bağlantı dizelerini sorgularken doğru Azure CLı başvurusunu kullandığınızdan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırın.

```azurecli
az group delete --name spnhubrg
```

## <a name="next-steps"></a>Sonraki adımlar

* Bu hızlı başlangıçta bir Bildirim Hub 'ı oluşturdunuz. Hub 'ı platform bildirim sistemi (PNS) ayarları ile yapılandırma hakkında bilgi edinmek için bkz. [Bildirim Hub 'ında anında iletme bildirimleri ayarlama](configure-notification-hub-portal-pns-settings.md)

* Azure CLı ile Bildirim Hub 'larını yönetmeye yönelik kapsamlı özellikleri öğrenin.

  [Notification Hubs tam başvuru listesi](/cli/azure/ext/notification-hub/notification-hub)

  [Notification Hubs ad alanı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/namespace)

  [Notification Hubs yetkilendirme kuralı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)

  [Notification Hubs kimlik bilgisi başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/credential)
