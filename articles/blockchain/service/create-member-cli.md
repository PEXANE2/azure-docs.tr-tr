---
title: Azure blok zinciri hizmeti üyesi oluşturma-Azure CLı
description: Azure CLı kullanarak bir blok zinciri Konsorsiyumu için Azure blok zinciri hizmeti üyesi oluşturun.
ms.date: 03/30/2020
ms.topic: quickstart
ms.reviewer: ravastra
ms.openlocfilehash: 1561f485917ecbb64d51ffbe045ab4120fbf58ad
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82116834"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure blok zinciri hizmeti blok zinciri üyesi oluşturma

Bu hızlı başlangıçta Azure CLı kullanarak Azure blok zinciri hizmeti 'ne yeni bir blok zinciri üyesi ve konsorsiyu dağıtırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Yok.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash). **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.51 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create \
                 --name myResourceGroup \
                 --location westus2
```

## <a name="create-a-blockchain-member"></a>Blok zinciri üyesi oluşturma

Bir Azure blok zinciri hizmeti üyesi, özel bir konsorsiyum blok zinciri ağında bir blok zinciri düğümüdür. Bir üyeyi sağlarken bir konsorsiyum ağı oluşturabilir veya katabilirsiniz. Bir konsorsiyum ağı için en az bir üyeye ihtiyacınız vardır. Katılımcılar tarafından gereken blok zinciri üyelerinin sayısı senaryonuza bağlıdır. Konsorsiyum katılımcıları bir veya daha fazla blok zinciri üyesine sahip olabilir veya diğer katılımcılarla üyeleri paylaşabilir. Consorda hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).

Geçirmeniz gereken birkaç parametre ve özellik vardır. Örnek parametreleri değerlerinizle değiştirin.

```azurecli-interactive
az resource create \
                    --resource-group myResourceGroup \
                    --name myblockchainmember \
                    --resource-type Microsoft.Blockchain/blockchainMembers \
                    --is-full-object \
                    --properties '{"location":"westus2", "properties":{"password":"strongMemberAccountPassword@1", "protocol":"Quorum", "consortium":"myConsortiumName", "consortiumManagementAccountPassword":"strongConsortiumManagementPassword@1"}, "sku":{"name":"S0"}}'
```

| Parametre | Açıklama |
|---------|-------------|
| **kaynak grubu** | Azure blok zinciri hizmeti kaynaklarının oluşturulduğu kaynak grubu adı. Önceki bölümde oluşturduğunuz kaynak grubunu kullanın.
| **ada** | Azure blok zinciri hizmeti blok zinciri üyesini tanımlayan benzersiz bir ad. Ad, genel uç nokta adresi için kullanılır. Örneğin, `myblockchainmember.blockchain.azure.com`.
| **konumuna** | Blok zinciri üyesinin oluşturulduğu Azure bölgesi. Örneğin, `westus2`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
| **parola** | Üyenin varsayılan işlem düğümü için parola. Blok zinciri üyesinin varsayılan işlem düğümü genel uç noktasına bağlanırken temel kimlik doğrulaması için parolayı kullanın.
| **unun** | Katılacak veya oluşturulacak konsorsiyumun adı. Consorda hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).
| **consortiumAccountPassword** | Konsorsiyum hesabı parolası üye hesap parolası olarak da bilinir. Üye hesabı parolası, üyelik için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
| **skuName** | Katman türü. Standart için S0 ve temel için B0 kullanın. Geliştirme, test ve kavram kanıtı için *temel* katmanı kullanın. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın. Ayrıca, blok Veri Yöneticisi Zinciri kullanıyorsanız veya yüksek hacimli özel işlemler gönderiyorsanız *Standart* katmanı da kullanmanız gerekir. Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanını değiştirmek desteklenmez.

Blok zinciri üyesini ve destekleyici kaynakları oluşturmak yaklaşık 10 dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıç veya öğretici için oluşturduğunuz blok zinciri üyesini kullanabilirsiniz. Artık gerekli değilse, hızlı başlangıç için oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete \
                 --name myResourceGroup \
                 --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti üyesi ve yeni bir konsorsiyu dağıttınız. Azure blok zinciri hizmeti üyesine eklemek için Ethereum için Azure blok zinciri geliştirme seti 'ni kullanmak üzere bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetine bağlanmak için Visual Studio Code kullanma](connect-vscode.md)
