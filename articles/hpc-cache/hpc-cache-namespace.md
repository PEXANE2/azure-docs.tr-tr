---
title: Azure HPC önbelleği toplanmış ad alanını anlayın
description: Azure HPC önbelleğiniz için sanal ad alanını planlayın
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/30/2020
ms.author: v-erkel
ms.openlocfilehash: 1c28f549cf93d77f6aef6bcde6a2225345a79cc9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91612957"
---
# <a name="plan-the-aggregated-namespace"></a>Toplanan ad alanını planlama

Azure HPC önbelleği, istemcilerin, arka uç depolama sisteminin ayrıntılarını gizleyen bir sanal ad alanı aracılığıyla çeşitli depolama sistemlerine erişmesini sağlar.

Bir depolama hedefi ekledikten sonra, depolama hedefi için bir veya daha fazla istemciye yönelik ad alanı yolu ayarlarsınız. İstemci makineler bu dosya yolunu bağlayabilir ve depolama sistemini doğrudan bağlamak yerine önbelleğe dosya okuma istekleri yapabilir.

Azure HPC Cache bu sanal dosya sistemini yönettiği için, istemciye yönelik yolu değiştirmeden depolama hedefini değiştirebilirsiniz. Örneğin, istemci tarafı yordamlarını yeniden yazmak zorunda kalmadan bir donanım depolama sistemini bulut depolaması ile değiştirebilirsiniz.

## <a name="aggregated-namespace-example"></a>Toplanmış ad alanı örneği

İstemci makinelerin ihtiyaç duydukları bilgilere rahat bir şekilde ulaşabilmesi için toplanan ad alanınızı planlayın ve yöneticiler ile iş akışı mühendislerinin yolları kolayca ayırabilmesini sağlayın.

Örneğin, Azure Blob 'da depolanan verileri işlemek için bir Azure HPC önbellek örneğinin kullanıldığı bir sistemi düşünün. Analiz, şirket içi veri merkezinde depolanan şablon dosyalarını gerektirir.

Şablon verileri bir veri merkezinde depolanır ve bu iş için gereken bilgiler şu alt dizinlerde saklanır:

* */Goldline/Templates/acme2017/sku798*
* */Goldline/Templates/acme2017/sku980*

Veri merkezi depolama sistemi bu dışarı aktarmaları kullanıma sunar:

* */*
* */Goldline*
* */Goldline/Templates*

Çözümlenecek veriler, [Clfsload yardımcı programı](hpc-cache-ingest.md#pre-load-data-in-blob-storage-with-clfsload)kullanılarak "SourceCollection" adlı bir Azure Blob depolama kapsayıcısına kopyalanmış.

Önbellek üzerinden kolay erişime izin vermek için bu sanal ad alanı yollarıyla depolama hedefleri oluşturmayı göz önünde bulundurun:

| Arka uç depolama sistemi <br/> (NFS dosya yolu veya blob kapsayıcısı) | Sanal ad alanı yolu |
|-----------------------------------------|------------------------|
| /Goldline/Templates/acme2017/sku798     | /Templates/sku798      |
| /Goldline/Templates/acme2017/sku980     | /Templates/sku980      |
| SourceCollection                        | /Source               |

Bir NFS depolama hedefi, her biri benzersiz bir dışarı aktarma yoluna başvurduğu sürece birden fazla sanal ad alanı yoluna sahip olabilir. (NFS depolama hedefi başına önerilen en fazla ad alanı yolu sayısını öğrenmek için [NFS ad alanı yollarını](add-namespace-paths.md#nfs-namespace-paths) okuyun.)

NFS kaynak yolları aynı dışarı aktarmanın alt dizinleri olduğundan, aynı depolama hedefinden birden çok ad alanı yolu tanımlamanız gerekir.

| Depolama hedef konak adı  | NFS dışarı aktarma yolu     | Alt dizin yolu | Ad alanı yolu    |
|--------------------------|---------------------|-------------------|-------------------|
| *IP adresi veya ana bilgisayar adı* | /Goldline/Templates | acme2017/sku798   | /Templates/sku798 |
| *IP adresi veya ana bilgisayar adı* | /Goldline/Templates | acme2017/sku980   | /Templates/sku980 |

Bir istemci uygulaması, önbelleği bağlayabilir ve toplanmış ad alanı dosya yollarına, ve ' ye kolayca erişebilir ``/source`` ``/templates/sku798`` ``/templates/sku980`` .

Diğer bir yaklaşım, üst dizine bağlantılar gibi bir sanal yol oluşturmak `/templates` `acme2017` ve ardından istemcilerin `sku798` `sku980` önbelleği bağladıktan sonra ayrı ve dizinlere gitmesini sağlamak olabilir. Ancak, başka bir ad alanı yolunun alt dizini olan bir ad alanı yolu oluşturamazsınız. Bu nedenle, dizine bir yol oluşturursanız, `acme2017` alt dizinlerine doğrudan erişmek için herhangi bir ad alanı yolu da oluşturamazsınız.

Azure HPC cache **ad alanı** ayarları sayfasında, istemciye yönelik dosya sistemi gösterilir ve yol eklemenize veya düzenlemenize izin verir. Ayrıntılar için [toplanan ad alanını ayarla](add-namespace-paths.md) makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

Sanal dosya sisteminizi ayarlamaya karar verdikten sonra, bu adımları oluşturmak için aşağıdaki adımları uygulayın:

* Arka uç depolama sistemlerinizi Azure HPC önbelleğinize eklemek için [depolama hedefleri oluşturun](hpc-cache-add-storage.md)
* İstemci makinelerin dosyalara erişmek için kullandığı toplanmış ad alanını oluşturmak için [ad alanı yolları ekleyin](add-namespace-paths.md)
