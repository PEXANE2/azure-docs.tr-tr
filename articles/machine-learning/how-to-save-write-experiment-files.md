---
title: Deneme dosyalarını yazmak & kaydedilen yer
titleSuffix: Azure Machine Learning
description: Deneme giriş dosyalarınızı nereye kaydacağınızı ve depolama sınırlaması hatalarını ve deneme gecikmesini önlemek için çıktı dosyalarını nereye yazacağınızı öğrenin.
services: machine-learning
author: rastala
ms.author: roastala
manager: danielsc
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 12a38b08fd429280f34b4eb02d4b72187b622261
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79078458"
---
# <a name="where-to-save-and-write-files-for-azure-machine-learning-experiments"></a>Azure Machine Learning denemeleri için dosya ların kaydedildiği ve yazılması gereken yerler
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, depolama sınırı hatalarını ve deneme gecikmesini önlemek için giriş dosyalarını nereye kaydacağınızı ve denemelerinizden çıktı dosyalarının nerede yazacağınızı öğrenirsiniz.

Bir [hesaplama hedefi](how-to-set-up-training-targets.md)üzerinde eğitim çalıştırırken, dış ortamlardan izole edilir. Bu tasarımın amacı, deneyin tekrarlanabilirliğini ve taşınabilirliğini sağlamaktır. Aynı komut dosyasını aynı veya başka bir bilgi işlem hedefinde iki kez çalıştırın, aynı sonuçları alırsınız. Bu tasarımla, işlem hedeflerini, her biri tamamlandıktan sonra çalışan işlere hiçbir yakınlığı olmayan, devletsiz hesaplama kaynakları olarak değerlendirebilirsiniz.

## <a name="where-to-save-input-files"></a>Giriş dosyalarının kaydedildiği yer

Bir bilgi işlem hedefi veya yerel makineüzerinde deneme başlatamadan önce, bağımlılık dosyaları ve kodunuzu çalıştırmak için gereken veri dosyaları gibi gerekli dosyaların bu bilgi işlem hedefi için kullanılabilir olduğundan emin olmalısınız.

Azure Machine Learning, tüm komut dosyası klasörünü hedef bilgi işlem bağlamına kopyalayarak eğitim komut dosyalarını çalıştırır ve ardından anlık görüntü alır. Deneme anlık görüntülerinin depolama sınırı 300 MB ve/veya 2000 dosyadır.

Bu nedenle, şunları öneririz:

* **Dosyalarınızı Azure Machine Learning [veri deposunda](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py)depolama.** Bu, deneme gecikmesi sorunlarını önler ve kimlik doğrulama ve montajın Azure Machine Learning tarafından yönetildiği anlamına gelen uzak bir bilgi işlem hedefinden verilere erişme avantajlarına sahiptir. Kaynak dizininiz olarak bir veri deposu belirtme ve [veri depoları makalenizden Access verilerine](how-to-access-data.md) dosya yükleme hakkında daha fazla bilgi edinin.

* **Yalnızca birkaç veri dosyasına ve bağımlılık komut dosyasına ihtiyacınız varsa ve veri deposu kullanamıyorsanız,** dosyaları eğitim komut dosyanızla aynı klasör dizinine yerleştirin. Bu klasörü `source_directory` doğrudan eğitim komut dosyanızda veya eğitim komut dosyanızı çağıran kodda belirtiniz.

<a name="limits"></a>

### <a name="storage-limits-of-experiment-snapshots"></a>Deneme anlık görüntülerinin depolama sınırları

Azure Machine Learning, denemeler için, çalıştırmayı yapılandırırken önerdiğiniz dizine göre kodunuzu otomatik olarak deneme anlık görüntüsünü yapar. Bu 300 MB ve / veya 2000 dosyaları toplam sınırı vardır. Bu sınırı aşarsanız, aşağıdaki hatayı görürsünüz:

```Python
While attempting to take snapshot of .
Your total snapshot size exceeds the limit of 300.0 MB
```

Bu hatayı gidermek için deneme dosyalarınızı bir veri deposunda depolayın. Bir veri deposu kullanamıyorsanız, aşağıdaki tablo olası alternatif çözümler sunar.

Deney&nbsp;açıklaması|Depolama limiti çözümü
---|---
2000'den az dosya & veri deposu kullanamaz| Anlık görüntü boyut sınırını geçersiz kılma <br> `azureml._restclient.snapshots_client.SNAPSHOT_MAX_SIZE_BYTES = 'insert_desired_size'`<br> Bu, dosyaların sayısına ve boyutuna bağlı olarak birkaç dakika sürebilir.
Belirli komut dosyası dizini kullanmalı| Kaynak `.amlignore` kodun parçası olmayan dosyaları deneme anlık görüntüsünden hariç tutmak için bir dosya yapın. Dosya adlarını dosyaya `.amlignore` ekleyin ve eğitim komut dosyanızla aynı dizine yerleştirin. Dosya, `.amlignore` dosyayla `.gitignore` aynı [sözdizimini ve desenleri](https://git-scm.com/docs/gitignore) kullanır.
İşlem hattı|Her adım için farklı bir alt dizin kullanın
Jupyter notebooks| Bir `.amlignore` dosya oluşturun veya not defterinizi yeni, boş, alt dizine taşıyın ve kodunuzu yeniden çalıştırın.

## <a name="where-to-write-files"></a>Dosya yazma nın yeri

Eğitim denemelerinin yalıtımı nedeniyle, çalıştırmalar sırasında meydana gelen dosyalarda yapılan değişikliklerin ortamınızın dışında kalıcı olması gerekmez. Komut dosyanız dosyaları yerel olarak hesaplatmak üzere değiştirirse, değişiklikler bir sonraki deneme niz için kalıcı değildir ve istemci makinesine otomatik olarak geri yayılmaz. Bu nedenle, ilk deneme çalışması sırasında yapılan değişiklikler ikinci denemedekileri etkilemez ve etkilememelidir.

Değişiklikleri yazarken, bir Azure Machine Learning veri deposuna dosya yazmanızı öneririz. Bkz. [Veri depolarınızdaki verilere erişin.](how-to-access-data.md)

Bir veri deposuna ihtiyacınız yoksa, dosyayı `./outputs` ve/veya `./logs` klasöre yazın.

>[!Important]
> İki klasör, *çıktı* ve *günlük,* Azure Machine Learning tarafından özel muamele alır. Eğitim sırasında, dosya`./outputs` ve`./logs` klasörlere dosya yazdığınızda, dosyalar otomatik olarak çalışma geçmişinize yüklenir, böylece çalışmanız tamamlandıktan sonra bunlara erişebilirsiniz.

* **Durum iletileri veya puanlama sonuçları gibi çıktılar için,** dosyayı `./outputs` klasöre yazın, böylece çalışma geçmişinde yapı olarak kalıcıdırlar. İçeriği niçin geçmişi çalıştırmak için yüklendiğinde gecikme meydana gelebileceğinden, bu klasöre yazılan dosyaların sayısına ve boyutuna dikkat edin. Gecikme bir sorunsa, bir veri deposuna dosya yazma önerilir.

* **Yazılı dosyayı çalışma geçmişinde günlük olarak** `./logs` kaydetmek için dosyaları klasöre yazın. Günlükler gerçek zamanlı olarak yüklendiğinden, bu yöntem uzaktan çalıştırmadan canlı güncelleştirmeler akışı için uygundur.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri depolarınızdan verilere erişim](how-to-access-data.md)hakkında daha fazla bilgi edinin.

* [Eğitim Hedeflerini Ayarlama](how-to-set-up-training-targets.md)hakkında daha fazla bilgi edinin.
