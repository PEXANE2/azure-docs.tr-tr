---
title: Sanal ağları yönetme-Azure CLı-PostgreSQL için Azure veritabanı-esnek sunucu
description: Azure CLı kullanarak PostgreSQL için Azure veritabanı için sanal ağlar oluşturma ve yönetme-esnek sunucu
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 727eb4cd7e7c3de090e1573cb5358ef23118385e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941012"
---
# <a name="create-and-manage-virtual-networks-for-azure-database-for-postgresql---flexible-server-using-the-azure-cli"></a>Azure CLı kullanarak PostgreSQL için Azure veritabanı için sanal ağlar oluşturma ve yönetme-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

PostgreSQL için Azure veritabanı-esnek sunucu, esnek sunucunuza bağlanmak için iki tür karşılıklı kullanım dışı ağ bağlantısı yöntemini destekler. Bu iki seçenek şunlardır:

* Genel erişim (izin verilen IP adresleri)
* Özel erişim (Sanal Ağ Tümleştirmesi)

Bu makalede, Azure CLı kullanarak **özel erişim (VNET tümleştirmesi)** Ile PostgreSQL sunucusu oluşturmaya odaklanacağız. *Özel erişim (VNET tümleştirmesi)* ile esnek sunucunuzu kendi [Azure sanal ağınıza](../../virtual-network/virtual-networks-overview.md)dağıtabilirsiniz. Azure sanal ağları özel ve güvenli ağ iletişimi sağlar. Özel erişim 'de, PostgreSQL sunucusuna olan bağlantılar yalnızca sanal ağınız dahilinde kısıtlıdır. Bu konuda daha fazla bilgi edinmek için bkz. [özel erişim (VNET tümleştirmesi)](./concepts-networking.md#private-access-vnet-integration).

PostgreSQL için Azure veritabanı-esnek sunucu, sunucu oluşturma sırasında yalnızca sunucuyu bir sanal ağa ve alt ağa dağıtabilirsiniz. Esnek sunucu bir sanal ağa ve alt ağa dağıtıldıktan sonra, başka bir sanal ağa, alt ağa veya *ortak erişime (izin VERILEN IP adresleri)* taşıyamazsınız.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell’i başlatma

[Azure Cloud Shell](../../cloud-shell/overview.md) , bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de açabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash) . Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve çalıştırmak için **ENTER** ' u seçin.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2,0 veya üzerini gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="prerequisites"></a>Ön koşullar

[Az Login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanarak hesabınızda oturum açmanız gerekir. Azure hesabınızın **ABONELIK kimliğini** Ifade eden **ID** özelliğine göz önüne alın.

```azurecli-interactive
az login
```

[Az Account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set) komutunu kullanarak hesabınız altındaki belirli bir aboneliği seçin. Komutta **abonelik** bağımsız değişkeninin değeri olarak kullanılacak **az Login** çıktısından **ID** değerini bir yere unutmayın. Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. Aboneliğinizi tamamen almak için [az Account List](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list)kullanın.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-azure-database-for-postgresql---flexible-server-using-cli"></a>CLı kullanarak PostgreSQL için Azure veritabanı oluşturma-esnek sunucu
`az postgres flexible-server` *Özel erişim (VNET tümleştirmesi)* ile esnek sunucu oluşturmak için komutunu kullanabilirsiniz. Bu komut, varsayılan bağlantı yöntemi olarak özel erişim (VNet tümleştirmesi) kullanır. Hiçbiri sağlanmazsa, sizin için bir sanal ağ ve alt ağ oluşturulur. Zaten var olan sanal ağı ve alt ağı, alt ağ kimliğini kullanarak da sağlayabilirsiniz. <!-- You can provide the **vnet**,**subnet**,**vnet-address-prefix** or**subnet-address-prefix** to customize the virtual network and subnet.--> Aşağıdaki örneklerde gösterildiği gibi CLı kullanarak esnek sunucu oluşturmaya yönelik çeşitli seçenekler vardır.

>[!Important]
> Bu komutun kullanılması, alt ağı **Microsoft. DBforPostgreSQL/Flexibtaservers**'a devredebilir. Bu temsili, yalnızca PostgreSQL için Azure veritabanı 'nın esnek sunucularının bu alt ağı kullanabileceği anlamına gelir. Temsilci alt ağında diğer türdeki Azure kaynakları bulunamaz.
>
Azure CLı başvuru belgelerine başvurun <!--FIXME --> yapılandırılabilir CLı parametrelerinin tüm listesi için. Örneğin, aşağıdaki komutlarda kaynak grubunu isteğe bağlı olarak belirtebilirsiniz.

- Varsayılan sanal ağı kullanarak esnek sunucu oluşturma, varsayılan adres ön eki olan alt ağ
    ```azurecli-interactive
    az postgres flexible-server create
    ```
<!--- Create a flexible server using already existing virtual network and subnet
    ```azurecli-interactive
    az postgres flexible-server create --vnet myVnet --subnet mySubnet
    ```-->
- Zaten var olan bir sanal ağ, alt ağ ve alt ağ KIMLIĞINI kullanarak esnek bir sunucu oluşturun. Belirtilen alt ağda başka bir kaynak dağıtılmamalıdır ve bu alt ağ, henüz temsilci yoksa **Microsoft. DBforPostgreSQL/Flexibtaservers**için temsilci olarak alınacaktır.
    ```azurecli-interactive
    az postgres flexible-server create --subnet /subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/Microsoft.Network/virtualNetworks/{VNetName}/subnets/{SubnetName}
    ```
    > [!Note]
    > Sanal ağ ve alt ağ, esnek sunucunuz ile aynı bölgede ve abonelikte olmalıdır.
<!--  
- Create a flexible server using new virtual network, subnet with non-default address prefix
    ```azurecli-interactive
    az postgres flexible-server create --vnet myVnet --vnet-address-prefix 10.0.0.0/24 --subnet mySubnet --subnet-address-prefix 10.0.0.0/24
    ```-->
Azure CLı başvuru belgelerine başvurun <!--FIXME --> yapılandırılabilir CLı parametrelerinin tüm listesi için.

## <a name="next-steps"></a>Sonraki adımlar
- [PostgreSQL Için Azure veritabanı 'nda ağ iletişimi](./concepts-networking.md)hakkında daha fazla bilgi edinin-esnek sunucu.
- [Azure Portal kullanarak PostgreSQL Için Azure veritabanı-esnek sunucu sanal ağı oluşturun ve yönetin](./how-to-manage-virtual-network-portal.md).
- [PostgreSQL Için Azure veritabanı-esnek sunucu sanal ağı](./concepts-networking.md#private-access-vnet-integration)hakkında daha fazla bilgi edinin.