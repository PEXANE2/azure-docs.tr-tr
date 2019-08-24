---
title: Veri bilimi sanal makinesi havuzları - Azure | Microsoft Docs
description: DSVMs havuzlarını bir ekip için paylaşılan bir kaynak olarak dağıtma
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz, team data science Process'i
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: vijetaj
ms.openlocfilehash: 76a715838fa5eb4f806fccc98675ea5acdbe1822
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69992027"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Veri bilimi sanal makineleri paylaşılan havuz oluşturma

Bu makalede, bir ekip için paylaşılan bir veri bilimi sanal makineleri havuzunun (DSVMs) nasıl oluşturulacağı açıklanmaktadır. Paylaşılan havuz kullanmanın avantajları, daha iyi kaynak kullanımı, daha kolay paylaşım ve işbirliği ve DSVM kaynaklarının daha etkili bir şekilde yönetilmesini içerir.

Dsvm'leri havuzu oluşturmak için birçok yöntem ve teknolojileri kullanabilirsiniz. Bu makale etkileşimli sanal makineler (VM 'Ler) için havuzlara odaklanır. Alternatif bir yönetilen işlem altyapısı Azure Machine Learning Işlem olur. Daha fazla bilgi için bkz. [işlem hedeflerini ayarlama](../service/how-to-set-up-training-targets.md#amlcompute).

## <a name="interactive-vm-pool"></a>Etkileşimli VM havuzu

Tüm yapay ZEKA/veri bilimi ekibi tarafından paylaşılan etkileşimli VM'lerin bir havuz kullanılabilir her bir kullanıcı kümesi için ayrılmış bir örnek yerine DSVM örneğini oturum açmasına izin verir. Bu kurulum, kaynakların daha iyi kullanılabilirliğini ve daha etkili kullanımını sunar.

Etkileşimli bir VM havuzu oluşturmak için [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) teknolojisini kullanırsınız. Ölçek kümeleri, bir grup özdeş, yük dengeli ve otomatik ölçeklendirme Vm'leri oluşturmak ve yönetmek için kullanabilirsiniz.

Kullanıcı ana havuzuna ait IP ya da DNS adresine bağlanır. Ölçek oturum yollar ölçek kümesindeki bir kullanılabilir DSVM otomatik olarak ayarlayın. Kullanıcılar, oturum açtığı VM 'den bağımsız olarak tutarlı ve tanıdık bir ortam gerektirdiğinden, ölçek kümesindeki sanal makinenin tüm örnekleri, bir Azure dosya paylaşımında veya bir ağ dosya sistemi (NFS) paylaşımında olduğu gibi paylaşılan bir ağ sürücüsü bağlamalarından oluşur. Kullanıcının paylaşılan çalışma normalde her örnekleri bağlanan paylaşılan dosya depolama tutulur.

Örnek bir Azure Resource Manager şablonu ölçek üzerinde Ubuntu DSVM örnekleriyle kümesi oluşturan bulabilirsiniz [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Azure Resource Manager şablonu için aynı konumda bir [parametre dosyası](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) örneği bulacaksınız.

Azure CLı 'de parametre dosyası değerlerini belirterek Azure Resource Manager şablondan ölçek kümesi oluşturabilirsiniz:

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Önceki komutlarda, sahip olduğunuz varsayılmaktadır:
* Parametre dosyası, Ölçek kümesi örneği için belirtilen değerlerle bir kopyası.
* Sanal makine örneği sayısını.
* Azure dosyaları için işaretçiler paylaşın.
* Her VM'de bağlanacaktır depolama hesabı için kimlik bilgileri.

Parametre dosyası, komutları yerel olarak başvuruluyor. Parametreleri, satır içi veya komut istemi onlar için de geçirebilirsiniz.  

Yukarıdaki şablonu SSH ve Ubuntu Dsvm'leri arka uç havuzuna bir ön uç ölçek JupyterHub bağlantı noktasını etkinleştirir. Bir kullanıcı olarak, bir Secure Shell (SSH) veya jupi üzerinde sanal makinede normal şekilde oturum açın. VM örnekleri dinamik olarak ölçeklendirilebilir veya daha fazla azaltılabilen için, herhangi bir durum bağlı Azure dosyaları paylaşımında kaydedilmelidir. Windows Dsvm'leri havuzu oluşturmak için aynı yaklaşımı kullanabilirsiniz.

[Azure dosyaları paylaşımına bağlandığı betik](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) de GitHub Azure DataScienceVM depoda kullanılabilir. Betik, Azure dosya paylaşımını parametre dosyasında belirtilen bağlama noktası bağlar. Betik, ilk kullanıcının giriş dizininde bağlı sürücü geçici bağlantılar da oluşturur. Azure dosya paylaşımındaki kullanıcıya özgü bir not defteri dizini, kullanıcıların Jupyter not defterlerini `$HOME/notebooks/remote` erişebilmeleri, çalıştırmaları ve kaydedebilmeleri için dizine geçici olarak bağlanır. Ek kullanıcılar her kullanıcının Jupyter çalışma Azure dosyaları paylaşımına işaret edecek şekilde sanal makine oluşturduğunuzda, aynı yöntemi kullanabilirsiniz.

Sanal makine ölçek kümeleri, otomatik ölçeklendirmeyi destekler. Ne zaman ek örnekler oluşturulacağı ve örneklerin ne zaman Ölçeklendirilecek hakkında kurallar belirleyebilirsiniz. Örneğin, sıfır örneklere Vm'leri hiç kullanılmadığı zaman bulut donanım kullanım maliyet tasarrufu için ölçeği azaltabilirsiniz. Ayrıntılı adımlar için sanal makine ölçek kümeleri belgeleri sayfaları sağlamak [otomatik ölçeklendirme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Sonraki adımlar

* [Ortak bir kimlik ayarlayın](dsvm-common-identity.md)
* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde depolayın](dsvm-secure-access-keys.md)















