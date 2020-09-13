---
title: Hızlı başlangıç-CLı kullanarak Azure API Management örneği oluşturma (Önizleme)
description: Azure CLı kullanarak yeni bir Azure API Management hizmet örneği oluşturun.
author: dlepow
ms.service: api-management
ms.topic: quickstart
ms.custom: ''
ms.date: 09/10/2020
ms.author: apimpm
ms.openlocfilehash: 763a4f5555e9ea6cdb7e78840251fda5d6861f16
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90042563"
---
# <a name="quickstart-create-a-new-azure-api-management-service-instance-by-using-the-azure-cli-preview"></a>Hızlı başlangıç: Azure CLı (Önizleme) kullanarak yeni bir Azure API Management hizmet örneği oluşturma

Azure API Management (APIM), kuruluşların kendi veri ve hizmet potansiyellerini ortaya çıkarmak üzere API’leri dış, iş ortağı ve iç geliştiricilere yayımlamalarına yardımcı olur. API Management; geliştirici katılımı, iş öngörüleri, analizler, güvenlik ve koruma aracılığıyla başarılı bir API programı yürütmeye ilişkin temel uzmanlıklar sağlar. APıM, her yerde barındırılan mevcut arka uç hizmetleri için modern API ağ geçitleri oluşturmanıza ve yönetmenize olanak sağlar. Daha fazla bilgi için bkz. [Genel Bakış](api-management-key-concepts.md).

Bu hızlı başlangıçta, Azure CLI 'de [az APIM](/cli/azure/apim) komutlarını kullanarak yeni bir API Management örneği oluşturma adımları açıklanmaktadır. `az apim`Komut grubundaki komutlar Şu anda önizlemededir ve gelecekteki bir sürümde değiştirilebilir veya kaldırılabilir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.11.1 veya üzeri önerilir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm Azure kaynakları gibi Azure API Management örnekleri bir kaynak grubuna dağıtılmalıdır. Kaynak grupları, ilgili Azure kaynaklarını düzenlemenizi ve yönetmenizi sağlar.

İlk olarak, aşağıdaki [az Group Create](/cli/azure/group#az-group-create) komutuyla Orta ABD konumunda *myresourcegroup* adlı bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location centralus
```

## <a name="create-a-new-service"></a>Yeni hizmet oluşturma

Artık bir kaynak grubunuz olduğuna göre, bir API Management hizmet örneği oluşturabilirsiniz. [Az APIM Create](/cli/azure/apim#az-apim-create) komutunu kullanarak bir tane oluşturun ve bir hizmet adı ve Yayımcı Ayrıntıları sağlayın. Hizmet adı Azure içinde benzersiz olmalıdır. 

Aşağıdaki örnekte, hizmet adı için *uygulamaım* kullanılır. Adı benzersiz bir değer olarak güncelleştirin. Ayrıca API yayımcısının kuruluşunun adını ve bildirimleri alacak e-posta adresini güncelleştirin. 

```azurecli-interactive
az apim create --name myapim --resource-group myResourceGroup \
  --publisher-name Contoso --publisher-email admin@contoso.com \
  --no-wait
```

Varsayılan olarak, komut, Azure API Management değerlendirmek için ekonomik bir seçenek olan geliştirici katmanında örneği oluşturur. Bu katman üretim kullanımı için değildir. API Management katmanlarını ölçeklendirme hakkında daha fazla bilgi için bkz. [yükseltme ve ölçeklendirme](upgrade-and-scale.md). 

> [!TIP]
> Genellikle bu katmanda bir API Management hizmeti oluşturmak ve etkinleştirmek için 20 ila 30 dakika arasında sürer. Önceki komut, `--no-wait` hizmet oluşturulduğunda komutun hemen döndürdüğü şekilde seçeneğini kullanır.

[Az APIM Show](/cli/azure/apim#az-apim-show) komutunu çalıştırarak dağıtım durumunu denetleyin:

```azurecli-interactive
az apim show --name myapim --resource-group myResourceGroup --output table
```

İlk olarak, çıkış aşağıdakine benzer ve `Activating` durumu gösterir:

```console
NAME         RESOURCE GROUP    LOCATION    GATEWAY ADDR    PUBLIC IP    PRIVATE IP    STATUS      TIER       UNITS
-----------  ----------------  ----------  --------------  -----------  ------------  ----------  ---------  -------
myapim       myResourceGroup   Central US                                             Activating  Developer  1
```

Etkinleştirildikten sonra durum olur `Online` ve hizmet örneği bir ağ geçidi adresine ve genel IP adresine sahiptir. Şimdilik, bu adresler herhangi bir içerik sunmaz. Örneğin:

```console
NAME         RESOURCE GROUP    LOCATION    GATEWAY ADDR                       PUBLIC IP     PRIVATE IP    STATUS    TIER       UNITS
-----------  ----------------  ----------  ---------------------------------  ------------  ------------  --------  ---------  -------
myapim       myResourceGroup   Central US  https://myapim.azure-api.net       203.0.113.1                 Online    Developer  1
```

API Management hizmet örneğiniz çevrimiçi olduğunda, bunu kullanmaya hazırsınız demektir. [Ilk API 'nizi içeri aktarma ve yayımlama](import-and-publish.md)öğreticisiyle başlayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve API Management hizmet örneğini kaldırabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk API’nizi içeri aktarma ve yayımlama](import-and-publish.md)
