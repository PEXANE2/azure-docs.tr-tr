---
title: Azure Blockchain Hizmeti üyesi oluşturma - Azure CLI
description: Azure CLI'yi kullanan blockchain konsorsiyumu için bir Azure Blockchain Hizmeti üyesi oluşturun.
ms.date: 03/12/2020
ms.topic: quickstart
ms.reviewer: ravastra
ms.openlocfilehash: 0a3cf3d7c7f3dc0b8ece6fd6a466e42ae970b61c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79214727"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Hızlı başlatma: Azure CLI'yi kullanarak Bir Azure Blockchain Hizmeti blockchain üyesi oluşturun

Bu hızlı başlangıçta, Azure CLI'yi kullanarak Azure Blockchain Hizmeti'nde yeni bir blockchain üyesi ve konsorsiyum uyguluyorsunuz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/bash](https://shell.azure.com/bash)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yüklemeyi ve kullanmayı tercih ederseniz, bu hızlı başlatma azure CLI sürümü 2.0.51 veya sonrası gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye](https://docs.microsoft.com/cli/azure/install-azure-cli)bakın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create \
                 --name myResourceGroup \
                 --location westus2
```

## <a name="create-a-blockchain-member"></a>Blockchain üyesi oluşturma

Azure Blockchain Service üyesi, özel bir konsorsiyum blockchain ağında ki blockchain düğümüdür. Bir üye sağlarken, bir konsorsiyum ağı oluşturabilir veya katılabilirsiniz. Bir konsorsiyum ağı için en az bir üyeye ihtiyacınız var. Katılımcıların ihtiyaç duyduğu blockchain üye sayısı senaryonuza bağlıdır. Konsorsiyum katılımcılarının bir veya daha fazla blockchain üyesi olabilir veya üyeleri diğer katılımcılarla paylaşabilirler. Konsorsiyumlar hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.

Geçirmeniz gereken çeşitli parametreler ve özellikler vardır. Örnek parametreleri değerlerinizle değiştirin.

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
| **kaynak grubu** | Azure Blockchain Hizmet kaynaklarının oluşturulduğu kaynak grubu adı. Önceki bölümde oluşturduğunuz kaynak grubunu kullanın.
| **Adı** | Azure Blockchain Hizmeti blockchain üyenizi tanımlayan benzersiz bir ad. Ad, ortak bitiş noktası adresi için kullanılır. Örneğin, `myblockchainmember.blockchain.azure.com`.
| **Konum** | Blockchain üyesinin oluşturulduğu Azure bölgesi. Örneğin, `westus2`. Kullanıcılarınıza veya diğer Azure uygulamalarınıza en yakın konumu seçin.
| **parola** | Üyenin varsayılan hareket düğümünün parolası. Blockchain üyesinin varsayılan işlem düğümü ortak bitiş noktasına bağlanırken temel kimlik doğrulaması için parolayı kullanın.
| **Konsorsiyumu** | Katılmak veya oluşturmak için konsorsiyumun adı. Konsorsiyumlar hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.
| **konsorsiyumAccountPassword** | Konsorsiyum hesap parolası üye hesabı parolası olarak da bilinir. Üye hesabı parolası, üyeniz için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
| **skuName** | Katman türü. Temel için Standart ve B0 için S0 kullanın. Kavramların geliştirilmesi, test edilmesi ve kanıtedilmesi için *Temel* katmanı kullanın. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın.

Blockchain üyesi ve destekleyici kaynakları oluşturmak yaklaşık 10 dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz blockchain üyesini bir sonraki hızlı başlangıç veya öğretici için kullanabilirsiniz. Artık gerekmediğinde, hızlı başlangıç için oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete \
                 --name myResourceGroup \
                 --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Blockchain Service üyesi ve yeni bir konsorsiyum görevlendirdiniz. Bir Azure Blockchain Hizmeti üyesine takmak için Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanmak için bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmetine bağlanmak için Visual Studio Kodunu kullanma](connect-vscode.md)
