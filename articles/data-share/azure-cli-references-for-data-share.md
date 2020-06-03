---
title: Azure veri paylaşımıyla ilgili Azure CLı başvuruları
description: Azure veri paylaşımının Azure CLı başvuru giriş sayfası
services: data-share
author: dbradish-microsoft
manager: barbkess
ms.service: data-share
ms.devlang: azurecli
ms.topic: reference
ms.date: 05/27/2020
ms.author: dbradish
ms.openlocfilehash: 404022b13f44174e4b647f0430a58fac5eeb81df
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300622"
---
# <a name="azure-cli-for-azure-data-share"></a>Azure veri paylaşımında Azure CLı

Azure komut satırı arabirimi ([Azure CLI](/cli/azure/what-is-azure-cli)), Azure kaynaklarını oluşturmak ve yönetmek için kullanılan bir komut kümesidir.  Azure veri paylaşımıyla birlikte birçok Azure hizmeti arasında kullanılabilir.  Veri paylaşımıyla ilgili 65 farklı komut vardır!  Bu komutlar, hizmeti bir komut satırından etkin bir şekilde çalışmanıza olanak sağlar.

## <a name="references-for-data-share"></a>Veri paylaşımıyla ilgili başvurular

Azure veri paylaşımıyla ilgili tüm Azure CLı komutları Şu anda Azure CLı için uzantılardır.  Bir uzantı, deneysel ve yayın öncesi komutlara erişmenizi sağlar.  Uzantı başvuruları hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).

|Azure CLı başvurusu |Description
|-|-|-|
| [az datashare](/cli/azure/ext/datashare/datashare) | Azure veri paylaşımının yönetilmesine yönelik tüm komutlar.
| [az datashare hesabı](/cli/azure/ext/datashare/datashare/account) | Azure veri paylaşma hesaplarını yönetmek için komutlar.
| [az datashare tüketicisi](/cli/azure/ext/datashare/datashare/consumer) | Müşteriler için Azure veri paylaşımının yönetiminde kullanılacak komutlar.
| [az datashare veri kümesi](/cli/azure/ext/datashare/datashare/dataset) | Azure Data Share veri kümelerini yönetmek için sağlayıcıların komutları.
| [az datashare davetiyesi](/cli/azure/ext/datashare/datashare/invitation) | Azure veri paylaşımında davetleri yönetmek için tüketiciler komutları.
| [az datashare sağlayıcı-Share-Subscription](/cli/azure/ext/datashare/datashare/provider-share-subscription) | Azure veri paylaşma aboneliklerini yönetmek için sağlayıcıların komutları.
| [az datashare eşitlemesi](/cli/azure/ext/datashare/datashare/synchronization)  | Azure Data Share eşitlemesini yönetme komutları.
| [az datashare eşitlemesi-ayar](/cli/azure/ext/datashare/datashare/synchronization-setting)  | Azure veri paylaşımının eşitleme ayarlarını yönetmek için sağlayıcılar komutları.

## <a name="reference-examples"></a>Başvuru örnekleri

Her Azure CLı başvurusunda örnek verilmiştir. Bu görevleri Azure portal aracılığıyla da tamamlayabilirsiniz, ancak Azure CLı 'nin kullanılması tek bir komut satırı gerektirir.  Azure CLı kullanmanın ne kadar kolay olduğunu öğrenmek için birkaç kod bloğu aşağıda verilmiştir.

Azure veri paylaşımıyla çalışmak için öncelikle bir kaynak grubuna ihtiyacınız olacaktır.  Azure kaynak gruplarının Azure CLı ile oluşturulması ve yönetilmesi kolaydır.  

```azurecli
#create a resource group
az group create -location westus -name MyResourceGroup
```

```azurecli
#get a list of resource groups for a subscription
az group list --subscription MySubscription --output table
```

Veri paylaşma hesabı oluşturmak açıktır.

```azurecli
#create a data share account
az datashare account create --location "West US 2" --tags tag1=Red tag2=White --name MyAccount --resource-group MyResourceGroup
```

## <a name="see-also"></a>Ayrıca bkz.

* Yükleme ve oturum açma hakkında bilgi edinmek için [Azure CLI ile çalışmaya](/cli/azure/get-started-with-azure-cli) başlayın.

* Azure CLı belgelerindeki ek [çekirdek](/cli/azure/reference-index) ve [uzantı](/cli/azure/azure-cli-extensions-list) başvurularını bulun.
