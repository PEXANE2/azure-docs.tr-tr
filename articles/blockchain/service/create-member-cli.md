---
title: Azure blok zinciri hizmeti üyesi oluşturma-Azure CLı
description: Azure CLı kullanarak bir blok zinciri Konsorsiyumu için Azure blok zinciri hizmeti üyesi oluşturun.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/20/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 894c6392c302e1be7c57b85f3f923ee1ba5467f6
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74286921"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure blok zinciri hizmeti blok zinciri üyesi oluşturma

Bu hızlı başlangıçta Azure CLı kullanarak Azure blok zinciri hizmeti 'ne yeni bir blok zinciri üyesi ve konsorsiyu dağıtırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/bash](https://shell.azure.com/bash) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.51 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create \
                 --name myResourceGroup \
                 --location westus2
```

## <a name="create-a-blockchain-member"></a>Blok zinciri üyesi oluşturma

Azure blok zinciri hizmetinde, yeni bir konsorsiyumun çekirdek defter protokolünü çalıştıran bir blok zinciri üyesi oluşturun. Geçirmeniz gereken birkaç parametre ve özellik vardır. Örnek parametreleri değerlerinizle değiştirin.

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
| **name** | Azure blok zinciri hizmeti blok zinciri üyesini tanımlayan benzersiz bir ad. Ad, genel uç nokta adresi için kullanılır. Örneğin, `myblockchainmember.blockchain.azure.com`.
| **konum** | Blok zinciri üyesinin oluşturulduğu Azure bölgesi. Örneğin, `westus2`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
| **Parola** | Üyenin varsayılan işlem düğümü için parola. Blok zinciri üyesinin varsayılan işlem düğümü genel uç noktasına bağlanırken temel kimlik doğrulaması için parolayı kullanın.
| **unun** | Katılacak veya oluşturulacak konsorsiyumun adı.
| **consortiumAccountPassword** | Konsorsiyum hesabı parolası üye hesap parolası olarak da bilinir. Üye hesabı parolası, üyelik için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
| **skuName** | Katman türü. Standart için S0 ve temel için B0 kullanın.

Blok zinciri üyesini ve destekleyici kaynakları oluşturmak yaklaşık 10 dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıç veya öğretici için oluşturduğunuz blok zinciri üyesini kullanabilirsiniz. Artık gerekli değilse, Azure blok zinciri hizmeti tarafından oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete \
                 --name myResourceGroup \
                 --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti üyesi ve yeni bir konsorsiyu dağıttınız. Azure blok zinciri hizmeti 'nde bir konsorsiyume eklemek için, Ethereum için Azure blok zinciri geliştirme seti 'ni kullanmak üzere bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmeti Consortium ağına bağlanmak için Visual Studio Code kullanma](connect-vscode.md)
