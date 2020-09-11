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
ms.reviewer: thsomasu
ms.lastreviewed: 03/18/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 5361931328ed107c7cc130b633a40b1582828aa1
ms.sourcegitcommit: 70ee014d1706e903b7d1e346ba866f5e08b22761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90024154"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure Notification Hub 'ı oluşturma

Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmet hakkında daha fazla bilgi için bkz. [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

Bu hızlı başlangıçta, Azure CLı kullanarak bir Bildirim Hub 'ı oluşturursunuz. İlk bölüm Notification Hubs bir ad alanı oluşturma adımları sağlar. İkinci bölüm, mevcut bir ad alanında Bildirim Hub 'ı oluşturma adımları sağlar. Ayrıca, özel erişim ilkesi oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

> [!IMPORTANT]
> Notification Hubs, Azure CLı 'nin sürüm 2.0.67 veya üstünü gerektirir. Yüklü olan sürümü ve bağımlı kitaplıkları bulmak için `az --version` kodunu çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Azure CLI için uzantı başvurularıyla çalışırken önce uzantıyı yüklemeniz gerekir. Azure CLı uzantıları, çekirdek CLı 'nin bir parçası olarak gönderilmeyen deneysel ve yayın öncesi komutlara erişmenizi sağlar. Güncelleştirme ve kaldırma da dahil olmak üzere uzantılar hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).

Notification Hubs için Azure CLı uzantısını yükler.

```azurecli
az extension add --name notification-hub
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure Notification Hubs, tüm Azure kaynakları gibi bir kaynak grubuna dağıtılmalıdır.  Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.  Kaynak grupları hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager nedir?](/azure/azure-resource-manager/management/overview) .

Bu hızlı başlangıç için, **spnhubrg** adlı bir kaynak grubunu aşağıdaki [az Group Create](/cli/azure/group#az-group-create) komutuyla **eastus** konumunda oluşturun.

```azurecli
az group create --name spnhubrg --location eastus
```

## <a name="create-a-notification-hubs-namespace"></a>Notification Hubs ad alanı oluşturma

1. Bildirim Hub 'larınız için bir ad alanı oluşturun.

   Bir ad alanı bir veya daha fazla hub içerir ve ad tüm Azure abonelikleri genelinde benzersiz olmalı ve en az altı karakter uzunluğunda olmalıdır. Bir adın kullanılabilirliğini denetlemek için [az Notification-Hub ad alanı Check-AVAILABILITY](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability) komutunu kullanın.

   ```azurecli
   az notification-hub namespace check-availability --name spnhubns
   ```

   Azure CLı, aşağıdaki konsol çıkışını görüntüleyerek kullanım isteğinize yanıt verir:

   ```shell
   {
   "id": "/subscriptions/yourSubscriptionID/providers/Microsoft.NotificationHubs/checkNamespaceAvailability",
   "isAvailable": true,
   "location": null,
   "name": "spnhubns",
   "properties": false,
   "sku": null,
   "tags": null,
   "type": "Microsoft.NotificationHubs/namespaces/checkNamespaceAvailability"
   }
   ```

   Azure CLı yanıtının ikinci satırına dikkat edin `"isAvailable": true` . Bu satır `false` , ad alanı için belirttiğiniz istenen adın kullanılabilir olup olmadığını okur. Adın kullanılabilirliğini onayladıktan sonra, ad alanınızı oluşturmak için [az Notification-Hub ad alanı oluştur](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) komutunu çalıştırın.  

   ```azurecli
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

   `--name` `az notification-hub namespace create` Komuta sağlanmış olan veya [Azure kaynakları için adlandırma kurallarını ve kısıtlamalarını](../azure-resource-manager/management/resource-name-rules.md)karşılamıyorsa, Azure CLI aşağıdaki konsol çıkışıyla yanıt verir:

   ```shell
   #the name is not available
   The specified name is not available. For more information visit https://aka.ms/eventhubsarmexceptions.

   #the name is invalid
   The specified service namespace is invalid.
   ```

   Denediğiniz ilk ad başarılı olmazsa, yeni ad alanınız için farklı bir ad seçin ve `az notification-hub namespace create` komutu yeniden çalıştırın.

   > [!NOTE]
   > Bu adım ileriye doğru, `--namespace` Bu hızlı başlangıçta kopyaladığınız her bir Azure CLI komutunda parametresinin değerini değiştirmelisiniz.

2. Ad alanlarının bir listesini alın.

   Yeni ad alanınız hakkındaki ayrıntıları görmek için [az Notification-Hub Namespace List](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-list) komutunu kullanın. `--resource-group`Bir abonelik için tüm ad alanlarını görmek istiyorsanız parametresi isteğe bağlıdır.

   ```azurecli
   az notification-hub namespace list --resource-group spnhubrg
   ```

## <a name="create-notification-hubs"></a>Bildirim Hub 'ları oluşturma

1. İlk Bildirim Hub 'ınızı oluşturun.

   Yeni ad alanında bir veya daha fazla Bildirim Hub 'ı artık oluşturulabilir. Bir Bildirim Hub 'ı oluşturmak için [az Notification-Hub Create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) komutunu çalıştırın.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. İkinci bir Bildirim Hub 'ı oluşturun.

   Tek bir ad alanında birden çok Bildirim Hub 'ı oluşturulabilir. Aynı ad alanında ikinci bir Bildirim Hub 'ı oluşturmak için, `az notification-hub create` farklı bir hub adı kullanarak komutu yeniden çalıştırın.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

3. Bildirim Hub 'larının listesini alın.

   Azure CLı, yürütülen her komutla bir başarı veya hata iletisi döndürür; Bununla birlikte, Bildirim Hub 'larının bir listesini sorgulayabilmek yeniden kullanılır. [Az Notification-Hub List](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-list) komutu bu amaçla tasarlanmıştı.

   ```azurecli
   az notification-hub list --resource-group spnhubrg --namespace-name spnhubns --output table
   ```

## <a name="work-with-access-policies"></a>Erişim ilkeleriyle çalışma

1. Azure Notification Hubs, erişim ilkelerinin kullanımı ile [paylaşılan erişim imzası güvenliği](./notification-hubs-push-notification-security.md) kullanır. Bir Bildirim Hub 'ı oluşturduğunuzda iki ilke otomatik olarak oluşturulur. Bu ilkelerdeki bağlantı dizeleri anında iletme bildirimlerini yapılandırmak için gereklidir. [Az Notification-Hub Authorization-Rule List](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) komutu ilke adlarının ve bunlara ait kaynak gruplarının bir listesini sağlar.

   ```azurecli
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > Uygulamanızda _Defaultfullsharedaccesssignature_ ilkesini kullanmayın. Bu ilke yalnızca arka uçta kullanılmak üzere tasarlanmıştır. `Listen`İstemci uygulamanızda yalnızca erişim ilkelerini kullanın.

2. Anlamlı adlarla ek yetkilendirme kuralları oluşturmak isterseniz, [az Notification-Hub Authorization-Rule Create](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create) komutunu kullanarak kendi erişim ilkenizi oluşturup özelleştirebilirsiniz. `--rights`Parametresi, atamak istediğiniz izinlerin boşlukla ayrılmış bir listesidir.

   ```azurecli
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Manage Send
   ```

3. Her erişim ilkesi için iki anahtar ve bağlantı dizesi kümesi vardır. Daha sonra [bir Bildirim Hub 'ı yapılandırmak](./configure-notification-hub-portal-pns-settings.md)için bunlara ihtiyacınız olacaktır. Notification Hubs erişim ilkesi için anahtarları ve bağlantı dizelerini listelemek için [az Notification-Hub Authorization-Rule List-Keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu kullanın.

   ```azurecli
   # query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output table
   ```

   ```azurecli
   # query the keys and connection strings for a custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > [Notification Hubs ad alanı](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) ve [Bildirim Hub 'ının](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) ayrı erişim ilkeleri vardır. Anahtarlar ve bağlantı dizelerini sorgularken doğru Azure CLı başvurusunu kullandığınızdan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırın:

```azurecli
az group delete --name spnhubrg
```

## <a name="next-steps"></a>Sonraki adımlar

* Bu hızlı başlangıçta bir Bildirim Hub 'ı oluşturdunuz. Hub 'ı platform bildirim sistemi (PNS) ayarları ile yapılandırma hakkında bilgi edinmek için bkz. [Bildirim Hub 'ında anında iletme bildirimleri ayarlama](configure-notification-hub-portal-pns-settings.md)

* Azure CLı ile Bildirim Hub 'larını yönetmeye yönelik kapsamlı özellikleri öğrenin:

  [Notification Hubs tam başvuru listesi](/cli/azure/ext/notification-hub/notification-hub)

  [Notification Hubs ad alanı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/namespace)

  [Notification Hubs yetkilendirme kuralı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)

  [Notification Hubs kimlik bilgisi başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/credential)
