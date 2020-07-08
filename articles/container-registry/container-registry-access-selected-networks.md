---
title: Ortak kayıt defteri erişimini yapılandırma
description: Seçilen ortak IP adreslerinden veya adres aralıklarından bir Azure Container Registry 'ye erişimi etkinleştirmek için IP kurallarını yapılandırın.
ms.topic: article
ms.date: 05/19/2020
ms.openlocfilehash: dc0514fbe7d3e01914965cee5dc547172d4435a4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83702078"
---
# <a name="configure-public-ip-network-rules"></a>Genel IP ağ kurallarını yapılandırma

Azure Container Registry, varsayılan olarak herhangi bir ağdaki konaklardan internet üzerinden bağlantıları kabul eder. Bu makalede, kapsayıcı kayıt defterinizin yalnızca belirli genel IP adreslerinden veya adres aralıklarından erişime izin verecek şekilde nasıl yapılandırılacağı gösterilmektedir. Azure CLı ve Azure portal ile eşdeğer adımlar sağlanır.

IP ağ kuralları genel kayıt defteri uç noktasında yapılandırılır. IP ağ kuralları [özel bağlantı](container-registry-private-link.md) ile yapılandırılmış özel uç noktalara uygulanmıyor

IP erişim kurallarını yapılandırma, **Premium** kapsayıcı kayıt defteri hizmet katmanında kullanılabilir. Kayıt defteri hizmeti katmanları ve limitleri hakkında bilgi için bkz. [Azure Container Registry katmanları](container-registry-skus.md).

## <a name="access-from-selected-public-network---cli"></a>Seçili ortak ağdan erişim-CLı

### <a name="change-default-network-access-to-registry"></a>Kayıt defteri için varsayılan ağ erişimini değiştirme

Seçilen bir ortak ağa erişimi sınırlandırmak için öncelikle erişimi reddetmek üzere varsayılan eylemi değiştirin. Kayıt defterinizin adını şu [az ACR Update][az-acr-update] komutunda değiştirin:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

### <a name="add-network-rule-to-registry"></a>Kayıt defterine ağ kuralı ekle

Kayıt defterinize genel IP adresinden veya aralıktan erişime izin veren bir ağ kuralı eklemek için [az ACR Network-Rule Add][az-acr-network-rule-add] komutunu kullanın. Örneğin, kapsayıcı kayıt defterinin adını ve bir sanal ağ içindeki bir VM 'nin genel IP adresini değiştirin.

```azurecli
az acr network-rule add \
  --name mycontainerregistry \
  --ip-address <public-IP-address>
```

> [!NOTE]
> Kural eklendikten sonra kuralın etkili olması birkaç dakika sürer.

## <a name="access-from-selected-public-network---portal"></a>Seçili ortak ağdan erişim-Portal

1. Portalda kapsayıcı Kayıt defterinize gidin.
1. **Ayarlar**altında **ağ**' ı seçin.
1. **Ortak erişim** sekmesinde, **Seçili ağlardan**ortak erişime izin ver ' i seçin.
1. **Güvenlik duvarı**altında, bir sanal ağdaki VM 'nın genel IP adresi gibi BIR genel IP adresi girin. Ya da VM 'nin IP adresini içeren CıDR gösteriminde bir adres aralığı girin.
1. **Kaydet**'i seçin.

![Kapsayıcı kayıt defteri için güvenlik duvarı kuralını yapılandırma][acr-access-selected-networks]

> [!NOTE]
> Kural eklendikten sonra kuralın etkili olması birkaç dakika sürer.

> [!TIP]
> İsteğe bağlı olarak, yerel istemci bilgisayarından veya IP adresi aralığından kayıt defteri erişimini etkinleştirin. Bu erişime izin vermek için bilgisayarın genel IPv4 adresine sahip olmanız gerekir. Internet tarayıcısında "IP adresim nedir?" öğesini arayarak bu adresi bulabilirsiniz. Portalda **ağ** sayfasında güvenlik duvarı ayarlarını yapılandırdığınızda geçerli istemci IPv4 adresi de otomatik olarak görünür.

## <a name="disable-public-network-access"></a>Ortak ağ erişimini devre dışı bırak

İsteğe bağlı olarak, kayıt defterindeki genel uç noktasını devre dışı bırakın. Genel uç noktayı devre dışı bırakmak tüm güvenlik duvarı yapılandırmalarının üzerine yazar. Örneğin, [özel bağlantı](container-registry-private-link.md)kullanarak bir sanal ağda güvenli hale getirilmiş bir kayıt defterine genel erişimi devre dışı bırakmak isteyebilirsiniz.

### <a name="disable-public-access---cli"></a>Genel erişimi devre dışı bırak-CLı

Azure CLı kullanarak genel erişimi devre dışı bırakmak için [az ACR Update][az-acr-update] çalıştırın ve `--public-network-enabled` olarak ayarlayın `false` . 

> [!NOTE]
> `public-network-enabled`Bağımsız değişken Azure CLI 2.6.0 veya üstünü gerektirir. 

```azurecli
az acr update --name myContainerRegistry --public-network-enabled false
```

### <a name="disable-public-access---portal"></a>Genel erişimi devre dışı bırak-Portal

1. Portalda kapsayıcı Kayıt defterinize gidin ve **ayarlar > ağ**' ı seçin.
1. **Genel erişim** sekmesinde, **genel ağ erişimine izin ver**' in altında **devre dışı**' yı seçin. Sonra **Kaydet**'i seçin.

![Genel erişimi devre dışı bırak][acr-access-disabled]


## <a name="restore-public-network-access"></a>Ortak ağ erişimini geri yükleme

Genel uç noktayı yeniden etkinleştirmek için ağ ayarlarını ortak erişime izin verecek şekilde güncelleştirin. Ortak uç noktanın etkinleştirilmesi tüm güvenlik duvarı yapılandırmalarının üzerine yazar. 

### <a name="restore-public-access---cli"></a>Ortak erişimi geri yükleme-CLı

[Az ACR Update][az-acr-update] ' i çalıştırın ve `--public-network-enabled` olarak ayarlayın `true` . 

> [!NOTE]
> `public-network-enabled`Bağımsız değişken Azure CLI 2.6.0 veya üstünü gerektirir. 

```azurecli
az acr update --name myContainerRegistry --public-network-enabled true
```

### <a name="restore-public-access---portal"></a>Ortak erişimi geri yükleme-Portal

1. Portalda kapsayıcı Kayıt defterinize gidin ve **ayarlar > ağ**' ı seçin.
1. **Genel erişim** sekmesinde, **ortak ağ erişimine izin ver**' in altında **tüm ağlar**' ı seçin. Sonra **Kaydet**'i seçin.

![Tüm ağlardan ortak erişim][acr-access-all-networks]

## <a name="next-steps"></a>Sonraki adımlar

* Bir sanal ağda özel bir uç nokta kullanarak bir kayıt defterine erişimi kısıtlamak için bkz. Azure [Container kayıt defteri Için Azure özel bağlantısını yapılandırma](container-registry-private-link.md).
* Bir istemci güvenlik duvarının ardında kayıt defteri erişim kuralları ayarlamanız gerekiyorsa, bkz. [güvenlik duvarı arkasındaki bir Azure Container Registry 'ye erişmek için kuralları yapılandırma](container-registry-firewall-access-rules.md).

[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com

[acr-access-selected-networks]: ./media/container-registry-access-selected-networks/acr-access-selected-networks.png
[acr-access-disabled]: ./media/container-registry-access-selected-networks/acr-access-disabled.png
[acr-access-all-networks]: ./media/container-registry-access-selected-networks/acr-access-all-networks.png
