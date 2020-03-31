---
title: Quickstart - Azure CLI'yi kullanarak bir Azure bildirim merkezi oluşturun | Microsoft Dokümanlar
description: Bu eğitimde, Azure CLI'yi kullanarak bir Azure bildirim hub'ı oluşturmayı öğrenirsiniz.
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80082454"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Hızlı başlatma: Azure CLI'yi kullanarak bir Azure bildirim merkezi oluşturun

Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmet hakkında daha fazla bilgi için Azure [Bildirim Hub'ları nedir?](notification-hubs-push-notification-overview.md)

Bu hızlı başlangıçta, Azure CLI'yi kullanarak bir bildirim hub'ı oluşturursunuz. İlk bölümde, bir bildirim merkezi ad alanı oluşturmak ve bu ad alanı için erişim ilkesi bilgilerini sorgulamak için adımlar verir. İkinci bölümde, varolan bir ad alanında bir bildirim merkezi oluşturmak için adımlar verir.  Ayrıca, özel bir erişim ilkesi oluşturmayı da öğrenirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bildirim Hub'ları, Azure CLI sürümü 2.0.67 veya daha sonra gerektirir. Yüklenen `az --version` sürümü ve bağımlı kitaplıkları bulmak için çalıştırın. Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

1. Giriş yapın.

   CLI'nin yerel bir yüklemesini kullanıyorsanız [az giriş](/cli/azure/reference-index#az-login) komutunu kullanarak oturum açın.

    ```azurecli-interactive
    az login
    ```

    Kimlik doğrulama işlemini tamamlamak için terminalinizde görüntülenen adımları izleyin.

2. Azure CLI uzantısını yükleyin.

   Bildirim hub'ları için Azure CLI komutlarını çalıştırmak [için Bildirim Hub'ları için](/cli/azure/ext/notification-hub/notification-hub)Azure CLI uzantısını yükleyin.  

    ```azurecli-interactive
    az extension add --name notification-hub
   ```

3. Bir kaynak grubu oluşturun.

   Azure bildirim hub'ları, tüm Azure kaynakları gibi bir kaynak grubuna dağıtılmalıdır. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

   Bu hızlı başlangıç için, *eastus* konumunda *spnhubrg* adlı bir kaynak grubu oluşturmak aşağıdaki [az grup oluşturma](/cli/azure/group#az-group-create) komutu ile:

   ```azurecli-interactive
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Bildirim merkezi ad alanı oluşturma

1. Bildirim hub'larınız için bir ad alanı oluşturma

   Ad alanı bir veya daha fazla hub içerir ve adın tüm Azure aboneliklerinde benzersiz olması gerekir.  Verilen hizmet ad alanının kullanılabilirliğini denetlemek için [az bildirim merkezi ad alanı denetim-kullanılabilirlik](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability) komutunu kullanın.  Ad alanı oluşturmak için [az bildirim merkezi ad alanını çalıştırın.](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create)  

   ```azurecli-interactive
   #check availability
   az notification-hub namespace check-availability --name spnhubns

   #create the namespace
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

2. Ad alanı erişim ilkeniz için anahtarları ve bağlantı dizelerini listeleyin.

   **RootManageSharedAccessKey** adlı bir erişim ilkesi yeni bir ad alanı için otomatik olarak oluşturulur.  Her erişim ilkesinin iki anahtar kümesi ve bağlantı dizeleri vardır.  Ad alanının anahtarlarını ve bağlantı dizelerini listelemek için [az bildirim-hub ad alanı yetkilendirme-kural-tuşları](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu çalıştırın.

   ```azurecli-interactive
   az notification-hub namespace authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --name RootManageSharedAccessKey
   ```

## <a name="create-notification-hubs"></a>Bildirim hub'ları oluşturma

1. İlk bildirim hub'ınızı oluşturun.

   Artık yeni ad alanında bir bildirim hub'ı oluşturulabilir.  Bildirim [hub'ı](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) oluşturmak için az bildirim merkezi oluşturma komutunu çalıştırın.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. İkinci bir bildirim merkezi oluşturun.

   Tek bir ad alanında birden çok bildirim hub'ı oluşturulabilir.  Aynı ad alanında ikinci bir bildirim hub'ı oluşturmak için, farklı bir hub adı kullanarak komutu yeniden çalıştırın. `az notification-hub create`

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

## <a name="work-with-access-policies"></a>Erişim ilkeleriyle çalışma

1. Bildirim hub'ı için yeni bir yetkilendirme kuralı oluşturun.

   Her yeni bildirim hub'ı için otomatik olarak bir erişim ilkesi oluşturulur.  Kendi erişim ilkenizi oluşturmak ve özelleştirmek için [az bildirim merkezi yetkilendirme kuralı oluşturma](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create) komutunu kullanın.

   ```azurecli-interactive
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Send
   ```

2. Bildirim hub'ı için erişim ilkelerini listele.

   Bildirim hub'ı için hangi erişim ilkelerinin bulunduğunu sorgulamak için [az bildirim hub yetkilendirme kuralı listesini](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) kullanın.

   ```azurecli-interactive
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > Uygulamanızda **VarsayılanFullSharedAccessSignature** ilkesini kullanmayın. Bunu yalnızca arka ucunuzda kullanmanız gerekir.  İstemci uygulamanızda yalnızca **Dinleyin** erişim ilkelerini kullanın.

3. Bildirim merkezi erişim ilkesi için liste anahtarları ve bağlantı dizeleri

   Her erişim ilkesi için iki anahtar kümesi ve bağlantı dizeleri vardır.  Anında iletme bildirimlerini işlemek için daha sonra bunlara ihtiyacınız olur.  Bildirim hub erişim ilkesinin anahtarlarını ve bağlantı dizelerini listelemek için [az bildirim-hub yetkilendirme-kural-anahtar lar](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) komutunu kullanın.

   ```azurecli-interactive
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output json

   #query the keys and connection strings for the custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > [Bildirim hub'ı ad alanı](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) ve bildirim [hub'ı](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) ayrı erişim ilkelerine sahiptir.  Anahtarlar ve bağlantı dizeleri için sorgulama yaparken doğru Azure CLI başvurularını kullandığınızdan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve ilgili tüm kaynakları kaldırmak için [az grubu silme](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group delete --name spnhubrg
```

## <a name="see-also"></a>Ayrıca bkz.

Azure CLI ile bildirim hub'larını yönetmek için tam yetenekleri keşfedin.

* [Bildirim Hub'ları tam Azure CLI başvuru listesi](/cli/azure/ext/notification-hub/notification-hub)
* [Bildirim Hub'ları ad alanı Azure CLI başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/namespace)
* [Bildirim Hub'ları yetkilendirme kuralı Azure CLI başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)
* [Bildirim Hub'ları kimlik bilgileri Azure CLI başvuru listesi](/cli/azure/ext/notification-hub/notification-hub/credential)
