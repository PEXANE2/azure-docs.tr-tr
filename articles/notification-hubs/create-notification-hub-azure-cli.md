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
ms.date: 03/17/2020
ms.author: dbradish
ms.reviewer: sethm
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 830fd33e19a10ec6472650e3d26fec677b82c3d7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80082454"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure Notification Hub 'ı oluşturma

Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmet hakkında daha fazla bilgi için bkz. [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

Bu hızlı başlangıçta, Azure CLı kullanarak bir Bildirim Hub 'ı oluşturursunuz. İlk bölüm, size bir Bildirim Hub 'ı ad alanı oluşturma ve bu ad alanı için erişim ilkesi bilgilerini sorgulama adımları sağlar. İkinci bölüm, mevcut bir ad alanında Bildirim Hub 'ı oluşturma adımları sağlar.  Ayrıca, özel erişim ilkesi oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Notification Hubs, Azure CLı 'nin sürüm 2.0.67 veya üstünü gerektirir. Yüklenen `az --version` sürümü ve bağımlı kitaplıkları bulmak için ' i çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

1. Giriş yapın.

   CLı 'nın yerel bir yüklemesini kullanıyorsanız [az Login](/cli/azure/reference-index#az-login) komutunu kullanarak oturum açın.

    ```azurecli-interactive
    az login
    ```

    Kimlik doğrulama işlemini gerçekleştirmek için terminalinizde görünen adımları izleyin.

2. Azure CLI uzantısını yükleyin.

   Bildirim Hub 'ları için Azure CLı komutlarını çalıştırmak için, [Notification Hubs Için](/cli/azure/ext/notification-hub/notification-hub)Azure CLI uzantısını ' nu yüklersiniz.  

    ```azurecli-interactive
    az extension add --name notification-hub
   ```

3. Bir kaynak grubu oluşturun.

   Tüm Azure kaynakları gibi Azure Notification Hub 'larının bir kaynak grubuna dağıtılması gerekir. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

   Bu hızlı başlangıç için, *spnhubrg* adlı bir kaynak grubunu aşağıdaki [az Group Create](/cli/azure/group#az-group-create) komutuyla *eastus* konumunda oluşturun:

   ```azurecli-interactive
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Bildirim Hub 'ı ad alanı oluşturma

1. Bildirim Hub 'larınız için bir ad alanı oluşturma

   Bir ad alanı bir veya daha fazla hub içerir ve ad tüm Azure abonelikleri genelinde benzersiz olmalıdır.  Verilen hizmet ad alanının kullanılabilirliğini denetlemek için [az Notification-Hub ad alanı Check-AVAILABILITY](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability) komutunu kullanın.  Ad alanı oluşturmak için [az Notification-Hub ad alanı oluştur](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) komutunu çalıştırın.  

   ```azurecli-interactive
   #check availability
   az notification-hub namespace check-availability --name spnhubns

   #create the namespace
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

2. Ad alanı erişim ilkeniz için anahtarları ve bağlantı dizelerini listeleyin.

   Yeni bir ad alanı için **RootManageSharedAccessKey** adlı bir erişim ilkesi otomatik olarak oluşturulur.  Her erişim ilkesinin iki anahtar ve bağlantı dizesi kümesi vardır.  Ad alanı için anahtarları ve bağlantı dizelerini listelemek için [az Notification-Hub ad alanı Authorization-Rule List-Keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu çalıştırın.

   ```azurecli-interactive
   az notification-hub namespace authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --name RootManageSharedAccessKey
   ```

## <a name="create-notification-hubs"></a>Bildirim Hub 'ları oluşturma

1. İlk Bildirim Hub 'ınızı oluşturun.

   Yeni ad alanında şimdi bir Bildirim Hub 'ı oluşturulabilir.  Bir Bildirim Hub 'ı oluşturmak için [az Notification-Hub Create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) komutunu çalıştırın.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. İkinci bir Bildirim Hub 'ı oluşturun.

   Tek bir ad alanında birden çok Bildirim Hub 'ı oluşturulabilir.  Aynı ad alanında ikinci bir Bildirim Hub 'ı oluşturmak için, farklı bir `az notification-hub create` hub adı kullanarak komutu yeniden çalıştırın.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

## <a name="work-with-access-policies"></a>Erişim ilkeleriyle çalışma

1. Bir Bildirim Hub 'ı için yeni bir yetkilendirme kuralı oluşturun.

   Her yeni Bildirim Hub 'ı için bir erişim ilkesi otomatik olarak oluşturulur.  Kendi erişim ilkenizi oluşturup özelleştirmek için [az Notification-Hub yetkilendirme-kuralı oluştur](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create) komutunu kullanın.

   ```azurecli-interactive
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Send
   ```

2. Bir Bildirim Hub 'ı için erişim ilkelerini listeleyin.

   Bir Bildirim Hub 'ı için hangi erişim ilkelerinin mevcut olduğunu sorgulamak için, [az Notification-Hub Authorization-Rule List](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) komutunu kullanın.

   ```azurecli-interactive
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > Uygulamanızda **Defaultfullsharedaccesssignature** ilkesini kullanmayın. Bunu yalnızca arka ucunuzda kullanmanız gerekir.  İstemci uygulamanızda yalnızca **dinleme** erişim ilkelerini kullanın.

3. Bir Bildirim Hub 'ı erişim ilkesi için anahtarları ve bağlantı dizelerini listeleyin

   Her erişim ilkesi için iki anahtar ve bağlantı dizesi kümesi vardır.  Anında iletme bildirimlerini işlemek için onlara daha sonra ihtiyacınız olacak.  Bir Bildirim Hub 'ı erişim ilkesi için anahtar ve bağlantı dizelerini listelemek için, [az Notification-Hub Authorization-Rule List-Keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu kullanın.

   ```azurecli-interactive
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output json

   #query the keys and connection strings for the custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > Bir [Bildirim Hub 'ı ad alanı](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) ve [Bildirim Hub 'ının](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) ayrı erişim ilkeleri vardır.  Anahtarlar ve bağlantı dizelerini sorgularken doğru Azure CLı başvurusunu kullandığınızdan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive
az group delete --name spnhubrg
```

## <a name="see-also"></a>Ayrıca bkz.

Azure CLı ile Bildirim Hub 'larını yönetmek için tam özellikleri bulun.

* [Tam Azure CLı başvuru listesi Notification Hubs](/cli/azure/ext/notification-hub/notification-hub)
* [Notification Hubs ad alanı Azure CLı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/namespace)
* [Notification Hubs yetkilendirme kuralı Azure CLı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)
* [Notification Hubs kimlik bilgileri Azure CLı başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/credential)
