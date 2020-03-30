---
title: Azure CLI ile Linux VM görüntülerini seçin
description: Market VM görüntüleri için yayımcı, teklif, SKU ve sürümü belirlemek için Azure CLI'yi nasıl kullanacağınızı öğrenin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 01/25/2019
ms.author: cynthn
ms.openlocfilehash: 0026c70a3a1a6b5e635e6b43e74b557d4218e6d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250315"
---
# <a name="find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Azure CLI ile Azure Market'te Linux VM görüntülerini bulma

Bu konu, Azure Marketi'nde VM görüntülerini bulmak için Azure CLI'nin nasıl kullanılacağını açıklar. CLI, Kaynak Yöneticisi şablonları veya diğer araçlarla programlı bir VM oluştururken bir Market görüntüsünü belirtmek için bu bilgileri kullanın.

Ayrıca [Azure Marketi](https://azuremarketplace.microsoft.com/) mağazası, [Azure portalı](https://portal.azure.com)veya [Azure PowerShell'i](../windows/cli-ps-findimage.md)kullanarak kullanılabilir resimlere ve tekliflere göz atın. 

En son [Azure CLI'sini](/cli/azure/install-azure-cli) yüklediğinizden ve bir Azure`az login`hesabında oturum açtığınızı unutmayın ( ).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="list-popular-images"></a>Popüler resimleri listele

Azure Marketi'nde popüler VM `--all` görüntülerinin listesini görmek için seçenek olmadan [az vm resim listesi](/cli/azure/vm/image) komutunu çalıştırın. Örneğin, önbelleğe alınmış popüler görüntülerin listesini tablo biçiminde görüntülemek için aşağıdaki komutu çalıştırın:

```azurecli
az vm image list --output table
```

Çıktı, URN *(Urn* sütunundaki değer) görüntüiçerir. Bu popüler Marketplace görüntülerinden biriyle bir VM oluştururken, alternatif olarak *UrnAlias*belirtebilirsiniz , *UbuntuLTS*gibi kısaltılmış bir form .

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.5                 OpenLogic:CentOS:7.5:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.3                SUSE:openSUSE-Leap:42.3:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7-RAW               RedHat:RHEL:7-RAW:latest                                        RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Belirli görüntüleri bulma

Markette belirli bir VM görüntüsünü bulmak `az vm image list` için `--all` seçeneği olan komutu kullanın. Komutun bu sürümünün tamamlanması biraz zaman alır ve uzun çıktı döndürebilir, bu nedenle genellikle listeyi `--publisher` başka bir parametreye göre filtrelersiniz. 

Örneğin, aşağıdaki komut Tüm Debian tekliflerini görüntüler `--all` (anahtar olmadan yalnızca ortak görüntülerin yerel önbelleğinde arama yaptığını unutmayın):

```azurecli
az vm image list --offer Debian --all --output table 

```

Kısmi çıkış: 

```
Offer              Publisher    Sku                  Urn                                                    Version
-----------------  -----------  -------------------  -----------------------------------------------------  --------------
Debian             credativ     7                    credativ:Debian:7:7.0.201602010                        7.0.201602010
Debian             credativ     7                    credativ:Debian:7:7.0.201603020                        7.0.201603020
Debian             credativ     7                    credativ:Debian:7:7.0.201604050                        7.0.201604050
Debian             credativ     7                    credativ:Debian:7:7.0.201604200                        7.0.201604200
Debian             credativ     7                    credativ:Debian:7:7.0.201606280                        7.0.201606280
Debian             credativ     7                    credativ:Debian:7:7.0.201609120                        7.0.201609120
Debian             credativ     7                    credativ:Debian:7:7.0.201611020                        7.0.201611020
Debian             credativ     7                    credativ:Debian:7:7.0.201701180                        7.0.201701180
Debian             credativ     8                    credativ:Debian:8:8.0.201602010                        8.0.201602010
Debian             credativ     8                    credativ:Debian:8:8.0.201603020                        8.0.201603020
Debian             credativ     8                    credativ:Debian:8:8.0.201604050                        8.0.201604050
Debian             credativ     8                    credativ:Debian:8:8.0.201604200                        8.0.201604200
Debian             credativ     8                    credativ:Debian:8:8.0.201606280                        8.0.201606280
Debian             credativ     8                    credativ:Debian:8:8.0.201609120                        8.0.201609120
Debian             credativ     8                    credativ:Debian:8:8.0.201611020                        8.0.201611020
Debian             credativ     8                    credativ:Debian:8:8.0.201701180                        8.0.201701180
Debian             credativ     8                    credativ:Debian:8:8.0.201703150                        8.0.201703150
Debian             credativ     8                    credativ:Debian:8:8.0.201704110                        8.0.201704110
Debian             credativ     8                    credativ:Debian:8:8.0.201704180                        8.0.201704180
Debian             credativ     8                    credativ:Debian:8:8.0.201706190                        8.0.201706190
Debian             credativ     8                    credativ:Debian:8:8.0.201706210                        8.0.201706210
Debian             credativ     8                    credativ:Debian:8:8.0.201708040                        8.0.201708040
Debian             credativ     8                    credativ:Debian:8:8.0.201710090                        8.0.201710090
Debian             credativ     8                    credativ:Debian:8:8.0.201712040                        8.0.201712040
Debian             credativ     8                    credativ:Debian:8:8.0.201801170                        8.0.201801170
Debian             credativ     8                    credativ:Debian:8:8.0.201803130                        8.0.201803130
Debian             credativ     8                    credativ:Debian:8:8.0.201803260                        8.0.201803260
Debian             credativ     8                    credativ:Debian:8:8.0.201804020                        8.0.201804020
Debian             credativ     8                    credativ:Debian:8:8.0.201804150                        8.0.201804150
Debian             credativ     8                    credativ:Debian:8:8.0.201805160                        8.0.201805160
Debian             credativ     8                    credativ:Debian:8:8.0.201807160                        8.0.201807160
Debian             credativ     8                    credativ:Debian:8:8.0.201901221                        8.0.201901221
...
```

Benzer filtreleri , `--location` `--publisher`, `--sku` ve seçenekleri uygulayın. Tüm Debian görüntülerini bulmak için `--offer Deb` arama yapmak gibi bir filtrede kısmi eşleşmeler gerçekleştirebilirsiniz.

`--location` Seçeneği olan belirli bir konum belirtmezseniz, varsayılan konum için değerler döndürülür. (Çalıştırarak farklı bir `az configure --defaults location=<location>`varsayılan konum ayarlayın.)

Örneğin, aşağıdaki komut, Batı Avrupa'daki tüm Debian 8 SUS'ları listeler:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Kısmi çıkış:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
Debian   credativ     8                  credativ:Debian:8:8.0.201710090                  8.0.201710090
Debian   credativ     8                  credativ:Debian:8:8.0.201712040                  8.0.201712040
Debian   credativ     8                  credativ:Debian:8:8.0.201801170                  8.0.201801170
Debian   credativ     8                  credativ:Debian:8:8.0.201803130                  8.0.201803130
Debian   credativ     8                  credativ:Debian:8:8.0.201803260                  8.0.201803260
Debian   credativ     8                  credativ:Debian:8:8.0.201804020                  8.0.201804020
Debian   credativ     8                  credativ:Debian:8:8.0.201804150                  8.0.201804150
Debian   credativ     8                  credativ:Debian:8:8.0.201805160                  8.0.201805160
Debian   credativ     8                  credativ:Debian:8:8.0.201807160                  8.0.201807160
Debian   credativ     8                  credativ:Debian:8:8.0.201901221                  8.0.201901221
...
```

## <a name="navigate-the-images"></a>Resimlerde gezinme
 
Bir yerde bir görüntü bulmak için başka bir yolu [az vm görüntü listesi-yayıncılar](/cli/azure/vm/image)çalıştırmak için , [az vm görüntü listesi-teklifler](/cli/azure/vm/image), ve [az vm görüntü listesi-skus](/cli/azure/vm/image) komutları sırayla. Bu komutlarla, şu değerleri belirlersiniz:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

Ardından, seçili bir SKU için dağıtmak için bir sürüm seçebilirsiniz.

Örneğin, aşağıdaki komut, Batı ABD'deki görüntü yayımcılarını listeler:

```azurecli
az vm image list-publishers --location westus --output table
```

Kısmi çıkış:

```
Location    Name
----------  ----------------------------------------------------
westus      128technology
westus      1e
westus      4psa
westus      5nine-software-inc
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      accessdata-group
westus      accops
westus      Acronis
westus      Acronis.Backup
westus      actian-corp
westus      actian_matrix
westus      actifio
westus      activeeon
westus      advantech-webaccess
westus      aerospike
westus      affinio
westus      aiscaler-cache-control-ddos-and-url-rewriting-
westus      akamai-technologies
westus      akumina
...
```

Belirli bir yayımcıdan gelen teklifleri bulmak için bu bilgileri kullanın. Örneğin, Batı ABD'deki *Kanonik* yayımcı için, `azure vm image list-offers`çalıştırarak teklifleri bulun. Aşağıdaki örnekte olduğu gibi konumu ve yayımcıyı geçirin:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Çıktı:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
```
Batı ABD bölgesinde Canonical'In *Azure'da UbuntuServer* teklifini yayınladığını görüyorsunuz. Peki bunların SKU’su ne? Bu değerleri almak `azure vm image list-skus` için, keşfettiğiniz konumu, yayımcıyı ve teklifi çalıştırın ve ayarlayın:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Çıktı:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-LTS
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      18.04-DAILY-LTS
westus      18.04-LTS
westus      18.10
westus      18.10-DAILY
westus      19.04-DAILY
```

Son olarak, `az vm image list` istediğiniz SKU belirli bir sürümünü bulmak için komutu kullanın, örneğin, *18.04-LTS:*

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 18.04-LTS --all --output table
```

Kısmi çıkış:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201804262  18.04.201804262
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201805170  18.04.201805170
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201805220  18.04.201805220
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201806130  18.04.201806130
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201806170  18.04.201806170
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201807240  18.04.201807240
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808060  18.04.201808060
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808080  18.04.201808080
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808140  18.04.201808140
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201808310  18.04.201808310
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201809110  18.04.201809110
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201810030  18.04.201810030
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201810240  18.04.201810240
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201810290  18.04.201810290
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201811010  18.04.201811010
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201812031  18.04.201812031
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201812040  18.04.201812040
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201812060  18.04.201812060
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201901140  18.04.201901140
UbuntuServer  Canonical    18.04-LTS  Canonical:UbuntuServer:18.04-LTS:18.04.201901220  18.04.201901220
...
```

Artık URN değerini dikkate alarak tam olarak kullanmak istediğiniz görüntüyü seçebilirsiniz. [Az vm](/cli/azure/vm) `--image` oluşturma komutu ile bir VM oluştururken bu değeri parametre ile geçirin. URN'deki sürüm numarasını isteğe bağlı olarak "en son" ile değiştirebileceğinizi unutmayın. Bu sürüm her zaman görüntünün en son sürümüdür. 

Kaynak Yöneticisi şablonu yla bir VM dağıtırsanız, görüntü `imageReference` parametrelerini özelliklerde tek tek ayarlarsınız. Bkz. [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Plan özelliklerini görüntüleme

Bir resmin satın alma planı bilgilerini görüntülemek için [az vm görüntü göster](/cli/azure/image) komutunu çalıştırın. Çıktıdaki `plan` özellik değilse, `null`görüntünün programlı dağıtımdan önce kabul etmesi gereken terimler vardır.

Örneğin, Kanonik Ubuntu Server 18.04 LTS görüntü ek terimleri `plan` yoktur, çünkü `null`bilgi:

```azurecli
az vm image show --location westus --urn Canonical:UbuntuServer:18.04-LTS:latest
```

Çıktı:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/18.04-LTS/Versions/18.04.201901220",
  "location": "westus",
  "name": "18.04.201901220",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

Bitnami görüntü tarafından onaylanan RabbitMQ için benzer bir `plan` komut `name` `product`çalıştırma `publisher`aşağıdaki özellikleri gösterir: , ve . (Bazı görüntülerin de `promotion code` bir özelliği vardır.) Bu resmi dağıtmak için, koşulları kabul etmek ve programatik dağıtımı etkinleştirmek için aşağıdaki bölümlere bakın.

```azurecli
az vm image show --location westus --urn bitnami:rabbitmq:rabbitmq:latest
```
Çıktı:

```
{
  "dataDiskImages": [],
  "id": "/Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/bitnami/ArtifactTypes/VMImage/Offers/rabbitmq/Skus/rabbitmq/Versions/3.7.1901151016",
  "location": "westus",
  "name": "3.7.1901151016",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": {
    "name": "rabbitmq",
    "product": "rabbitmq",
    "publisher": "bitnami"
  },
  "tags": null
}
```

### <a name="accept-the-terms"></a>Koşulları kabul edin

Lisans koşullarını görüntülemek ve kabul etmek için [az vm görüntü kabul koşulları](/cli/azure/vm/image?) komutunu kullanın. Şartları kabul ettiğinizde, aboneliğinizde programatik dağıtımı etkinleştirin. Görüntü için abonelik başına yalnızca bir kez şartları kabul etmeniz gerekir. Örnek:

```azurecli
az vm image accept-terms --urn bitnami:rabbitmq:rabbitmq:latest
``` 

Çıktı, lisans `licenseTextLink` terimlerine a içerir ve aşağıdakilerin `true`değerinin `accepted`

```
{
  "accepted": true,
  "additionalProperties": {},
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.MarketplaceOrdering/offertypes/bitnami/offers/rabbitmq/plans/rabbitmq",
  "licenseTextLink": "https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_BITNAMI%253a24RABBITMQ%253a24RABBITMQ%253a24IGRT7HHPIFOBV3IQYJHEN2O2FGUVXXZ3WUYIMEIVF3KCUNJ7GTVXNNM23I567GBMNDWRFOY4WXJPN5PUYXNKB2QLAKCHP4IE5GO3B2I.txt",
  "name": "rabbitmq",
  "plan": "rabbitmq",
  "privacyPolicyLink": "https://bitnami.com/privacy",
  "product": "rabbitmq",
  "publisher": "bitnami",
  "retrieveDatetime": "2019-01-25T20:37:49.937096Z",
  "signature": "XXXXXXLAZIK7ZL2YRV5JYQXONPV76NQJW3FKMKDZYCRGXZYVDGX6BVY45JO3BXVMNA2COBOEYG2NO76ONORU7ITTRHGZDYNJNXXXXXX",
  "type": "Microsoft.MarketplaceOrdering/offertypes"
}
```

### <a name="deploy-using-purchase-plan-parameters"></a>Satınalma planı parametrelerini kullanarak dağıtma

Görüntünün koşullarını kabul ettikten sonra, abonelikte bir VM dağıtabilirsiniz. Komutu `az vm create` kullanarak görüntüyü dağıtmak için, görüntü için bir URN'ye ek olarak satın alma planı için parametreler sağlayın. Örneğin, Bitnami görüntüsü tarafından onaylanan RabbitMQ ile bir VM dağıtmak için:

```azurecli
az group create --name myResourceGroupVM --location westus

az vm create --resource-group myResourceGroupVM --name myVM --image bitnami:rabbitmq:rabbitmq:latest --plan-name rabbitmq --plan-product rabbitmq --plan-publisher bitnami
```

## <a name="next-steps"></a>Sonraki adımlar
Görüntü bilgilerini kullanarak hızlı bir şekilde sanal bir makine oluşturmak için Azure [CLI ile Linux VM'leri Oluştur ve Yönet'e](tutorial-manage-vm.md)bakın.
