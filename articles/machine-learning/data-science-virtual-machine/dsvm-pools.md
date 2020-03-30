---
title: Paylaşılan havuzlar
titleSuffix: Azure Data Science Virtual Machine
description: Bir ekip için paylaşılan bir kaynak olarak paylaşılan bir Veri Bilimi Sanal Makineleri (DSVM) havuzu oluşturmak & dağıtmayı öğrenin.
keywords: derin öğrenme, Yapay Bilgi, veri bilimi araçları, veri bilimi sanal makine, jeouzamsal analitik, ekip veri bilimi süreci
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 12/10/2018
ms.openlocfilehash: cc0efc0a076ddc3fc9425999f1e38b4a32dec7a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477349"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Veri Bilimi Sanal Makinelerden oluşan ortak bir havuz oluşturun

Bu makalede, bir ekip için paylaşılan bir Veri Bilimi Sanal Makineleri (DSVM) havuzuoluşturmayı öğreneceksiniz. Paylaşılan bir havuz kullanmanın yararları arasında daha iyi kaynak kullanımı, daha kolay paylaşım ve işbirliği ve DSVM kaynaklarının daha etkili yönetimi yer almaktadır.

DSVMs havuzu oluşturmak için birçok yöntem ve teknoloji kullanabilirsiniz. Bu makalede, etkileşimli sanal makineler (VM) havuzları üzerinde duruluyor. Alternatif yönetilen bilgi işlem altyapısı Azure Machine Learning Compute'dur. Daha fazla bilgi için [bkz.](../how-to-set-up-training-targets.md#amlcompute)

## <a name="interactive-vm-pool"></a>İnteraktif VM havuzu

Tüm AI/veri bilimi ekibi tarafından paylaşılan etkileşimli VM havuzu, kullanıcıların her kullanıcı kümesi için özel bir örneğine sahip olmak yerine DSVM'nin kullanılabilir bir örneğinde oturum açmalarına olanak tanır. Bu kurulum, kaynakların daha iyi kullanılabilirolmasını ve daha etkili kullanılmasını sağlar.

Etkileşimli bir VM havuzu oluşturmak için [Azure sanal makine ölçeği kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) teknolojisini kullanırsınız. Bir grup özdeş, yük dengeli ve otomatik ölçeklendirme VM'lerden oluşan bir grup oluşturmak ve yönetmek için ölçek kümelerini kullanabilirsiniz.

Kullanıcı ana havuzun IP veya DNS adresine giriş yaptı. Ölçek kümesi, oturumu otomatik olarak ölçek kümesindeki kullanılabilir bir DSVM'ye yönlendirir. Kullanıcılar oturum açtıkları VM'den bağımsız olarak tutarlı ve tanıdık bir ortam istediklerinden, ölçek kümesindeki VM'nin tüm örnekleri Azure Files paylaşımı veya Ağ Dosya Sistemi (NFS) paylaşımı gibi paylaşılan bir ağ sürücüsüne monte edilir. Kullanıcının paylaşılan çalışma alanı normalde her bir örneğin üzerine monte edilmiş paylaşılan dosya deposunda tutulur.

[GitHub'da](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json)Ubuntu DSVM örnekleriyle bir ölçek kümesi oluşturan örnek bir Azure Kaynak Yöneticisi şablonu bulabilirsiniz. Aynı konumda Azure Kaynak Yöneticisi şablonu için [parametre dosyasının](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) bir örneğini bulabilirsiniz.

Azure CLI'deki parametre dosyasıiçin değerler belirterek Azure Kaynak Yöneticisi şablonundan ölçek kümesini oluşturabilirsiniz:

```azurecli-interactive
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```

Önceki komutlar varsaymak:

* Parametre dosyasının, ölçek kümesiörneğinde belirtilen değerlerle bir kopyası.
* VM örneklerinin sayısı.
* Azure Dosyaları'na işaretçiler paylaşır.
* Her VM'ye monte edilecek depolama hesabının kimlik bilgileri.

Parametre dosyası komutları yerel olarak başvurur. Ayrıca, parametreleri satır satır içinde geçirebilir veya komut dosyanızda bunlar için istem edebilirsiniz.  

Önceki şablon, SSH ve JupyterHub bağlantı noktasını ön uç ölçeğinden Ubuntu DSVM'lerin arka uç havuzuna ayarlamaya olanak tanır. Kullanıcı olarak, VM'de Güvenli Shell (SSH) veya JupyterHub'da normal şekilde oturum açarsınız. VM örnekleri dinamik olarak ölçeklendirilebildiği veya küçültülebileceğinden, herhangi bir durum monte edilmiş Azure Dosyaları paylaşımına kaydedilmelidir. Windows DSVMs havuzu oluşturmak için aynı yaklaşımı kullanabilirsiniz.

[Azure Dosyaları paylaşımını monte eden komut dosyası,](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) GitHub'daki Azure DataScienceVM deposunda da kullanılabilir. Komut dosyası, parametre dosyasında belirtilen montaj noktasına Azure Dosyaları paylaşımını bağlar. Komut dosyası ayrıca ilk kullanıcının ev dizininde monte edilen sürücüye yumuşak bağlantılar oluşturur. Azure Dosyaları paylaşımındaki kullanıcıya özgü bir not defteri `$HOME/notebooks/remote` dizini, kullanıcıların Jupyter not defterlerine erişebilmeleri, çalıştırabilmesi ve kaydedilebilmeleri için dizinle yumuşak bir şekilde bağlantılıdır. VM'de her kullanıcının Jupyter çalışma alanını Azure Dosyaları paylaşımına işaret etmek için ek kullanıcılar oluşturduğunuzda aynı kuralı kullanabilirsiniz.

Sanal makine ölçeği otomatik küçültmeyi destekler. Ek örnekleri ne zaman oluşturup örnekleri ne zaman küçülteceklerine ilişkin kurallar belirleyebilirsiniz. Örneğin, VM'ler hiç kullanılmadığında bulut donanımı kullanım maliyetlerinden tasarruf etmek için sıfıra kadar ölçeklendirebilirsiniz. Sanal makine ölçeği kümelerinin dokümantasyon sayfaları [otomatik ölçeklendirme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview)için ayrıntılı adımlar sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Ortak bir kimlik ayarlama](dsvm-common-identity.md)
* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde depolama](dsvm-secure-access-keys.md)
