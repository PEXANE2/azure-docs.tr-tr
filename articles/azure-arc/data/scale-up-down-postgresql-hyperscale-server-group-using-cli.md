---
title: CLı (azdata veya kubectl) kullanarak PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini artırma ve azaltma
description: CLı (azdata veya kubectl) kullanarak PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini artırma ve azaltma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: b96f38d04fe3e3cb59fa75424ae588fe0e38f510
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941199"
---
# <a name="scale-up-and-down-an-azure-database-for-postgresql-hyperscale-server-group-using-cli-azdata-or-kubectl"></a>CLı (azdata veya kubectl) kullanarak PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini artırma ve azaltma



Bir sunucu grubunun özelliklerini veya tanımını değiştirmeniz gerekebilecek durumlar vardır. Örnek:

- Düzenleyicinin veya çalışan düğümlerinin kullandığı sanal çekirdekler sayısının ölçeğini artırma veya azaltma
- Düzenleyicinin veya çalışan düğümlerinin kullandığı belleğin ölçeğini artırma veya azaltma

Bu kılavuzda, sanal çekirdek ve/veya belleğin nasıl ölçekleneceği açıklanmaktadır.

Sunucu grubunuzun sanal çekirdek veya bellek ayarlarının ölçeği yukarı veya aşağı ölçeği, her bir sanal çekirdek ve bellek ayarı için en düşük ve/veya en büyük değeri ayarlama olasılığa sahip olduğunuz anlamına gelir. Sunucu grubunuzu belirli sayıda sanal çekirdek veya belirli bir bellek miktarı kullanacak şekilde yapılandırmak istiyorsanız en düşük ayarları en yüksek ayarlara eşit olarak ayarlarsınız.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="show-the-current-definition-of-the-server-group"></a>Sunucu grubunun geçerli tanımını göster

Sunucu grubunuzun geçerli tanımını göstermek ve geçerli sanal çekirdek ve bellek ayarlarını görmek için aşağıdaki komuttan birini çalıştırın:

### <a name="cli-with-azdata"></a>Azdata ile CLı

```console
azdata arc postgres server show -n <server group name>
```
### <a name="cli-with-kubectl"></a>Kubectl ile CLı

```console
kubectl describe postgresql-12/<server group name> [-n <namespace name>]
```
> [!NOTE]
> Bir PostgreSQL sürüm 11 sunucu grubu oluşturduysanız `kubectl describe postgresql-11/<server group name>` bunun yerine çalıştırın.

Sunucu grubunuzun yapılandırmasını döndürür. Sunucu grubunu varsayılan ayarlarla oluşturduysanız, tanımı şöyle görmeniz gerekir:

```console
"scheduling": {
      "default": {
        "resources": {
          "requests": {
            "memory": "256Mi"
          }
        }
      }
    },
```

## <a name="interpret-the-definition-of-the-server-group"></a>Sunucu grubunun tanımını yorumlama

Bir sunucu grubunun tanımında, düğüm başına Min/Max vCore ayarlarını ve düğüm başına en az/en fazla bellek değerini taşıyan bölüm, **"zamanlama"** bölümüdür. Bu bölümde, en fazla ayarlar **"sınırlar"** adlı bir alt bölümde kalıcı olacak ve en düşük ayarlar **"istekler"** adlı alt bölümde kalıcı olacak.

En büyük ayarlardan farklı olan en düşük ayarları ayarlarsanız, yapılandırma, sunucu grubunuza ihtiyaç duyduğunda istenen kaynakları tahsis eder. Ayarladığınız limitlerin aşmayacak.

Gerçekten sunucu grubunuz tarafından kullanılacak kaynaklar (sanal çekirdekler ve bellek) en fazla ayarlara kadar olur ve kümedeki iş yüklerine ve kaynaklara bağlıdır. Ayarların en fazla bir üst sınırı yoksa, sunucu grubunuz, Kubernetes kümesinin sunucu grubunuzun zamanlandığı Kubernetes düğümlerine ayırdığı tüm kaynakları kullanabilir.

Bu sanal çekirdek ve bellek ayarları, PostgreSQL hiper ölçek düğümlerinin (düzenleyici düğümü ve çalışan düğümleri) her biri için geçerlidir. Henüz düzenleyici düğümünün ve çalışan düğümlerinin tanımlarını ayrı olarak ayarlamak desteklenmez.

Varsayılan yapılandırmada, PostgreSQL hiper ölçeğini çalıştırmak için önerilen minimum bellek miktarı olduğundan, yalnızca minimum bellek 256 GB olarak ayarlanır.

> [!NOTE]
> Minimum ayar, sunucu grubunun bu en küçük değeri kullanması anlamına gelmez. Bu, sunucu grubuna ihtiyaç duyması durumunda en az bu minimum değer tahsis edilmesi garanti edilir. Örneğin, belirlediğimiz göz atalım `--minCpu 2` . Sunucu grubunun her zaman en az 2 sanal çekirdek kullanacağı anlamına gelmez. Bunun yerine, sunucu grubunun bu çok gerekmiyorsa 2 sanal çekirdeği kullanmaya başlayabileceği ve daha sonra ihtiyacı olan en az 2 sanal çekirdeğe ayrılması garanti edildiği anlamına gelir. Bu, Kubernetes kümesinin kaynakları diğer iş yüklerine, ihtiyaç duyması durumunda sunucu grubuna 2 sanal çekirdek ayırabilecek şekilde ayırmasını gerektirir.

>[!NOTE]
>Sisteminizin yapılandırmasını değiştirmeden önce lütfen Kubernetes kaynak modelini [buradan](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities) öğrendiğinizden emin olun

## <a name="scale-up-the-server-group"></a>Sunucu grubunun ölçeğini artırma

Ayarlamak üzere olduğunuz ayarların, Kubernetes kümeniz için ayarladığınız yapılandırma dahilinde değerlendirilmelidir. Kubernetes kümenizin karşılayamayamayacak değerleri ayarlamadığınızdan emin olun. Bu, hatalara veya öngörülemeyen davranışlara yol açabilir. Örnek olarak, yapılandırma değiştirildikten sonra sunucu _grubunuzun durumu uzun_ bir süre içinde kalırsa, Kubernetes kümenizin karşılayamadığı değerlere aşağıdaki parametreleri ayarlamış olduğunuz bir göstergesi olabilir. Bu durumda, değişikliği veya _troubleshooting_section okuyun.

Sunucu grubunuzun tanımını şu şekilde ölçeklendirmek istediğinizi varsayalım:

- En az sanal çekirdek = 2
- Maksimum sanal çekirdek = 4
- En az bellek = 512Mb
- Maksimum bellek = 1Gb

Aşağıdaki yaklaşımlardan birini kullanın:

### <a name="cli-with-azdata"></a>Azdata ile CLı

```console
azdata arc postgres server edit -n <name of your server group> --cores-request 2  --cores-limit 4  --memory-request 512Mi  --memory-limit 1024Mi
```

Komutu gösterdiği zaman başarıyla çalıştırılır:

```console
<name of your server group> is Ready
```

> [!NOTE]
> Bu parametreler hakkında daha fazla bilgi için, öğesini çalıştırın `azdata arc postgres server edit --help` .

### <a name="cli-with-kubectl"></a>Kubectl ile CLı

```console
kubectl edit postgresql-12/<server group name> [-n <namespace name>]
```

Bu, sizi, yapılandırmayı gezinebileceğiniz ve değiştirebileceğiniz VI düzenleyicisinde götürür. İstenen ayarı belirtimde alanın adına eşlemek için aşağıdakileri kullanın:

- Min sanal çekirdek = 2-> scheduling\default\resources\requests\cpu
- Maksimum sanal çekirdek = 4-> scheduling\default\resources\limits\cpu
- En az bellek = 512Mb-> scheduling\default\resources\requests\cpu
- Maksimum bellek = 1Gb-> scheduling\default\resources\limits\cpu

VI Düzenleyicisi hakkında bilgi sahibi değilseniz [, ihtiyacınız olabilecek](https://www.computerhope.com/unix/uvi.htm)komutların açıklamasına bakın:
- düzenleme modu: `i`
- oklarla gezinmek
- _stop düzenlemesi: `esc`
- kaydetmeden _exit: `:qa!`
- kaydettikten sonra _exit: `:qw!`


## <a name="show-the-scaled-up-definition-of-the-server-group"></a>Sunucu grubunun ölçeği genişletilmiş tanımını göster

Sunucu grubunun tanımını göstermek için komutu yeniden çalıştırın ve istediğiniz gibi ayarlandığını doğrulayın:

### <a name="cli-with-azdata"></a>Azdata ile CLı

```console
azdata arc postgres server show -n <the name of your server group>
```
### <a name="cli-with-kubectl"></a>Kubectl ile CLı

```console
kubectl describe postgresql-12/<server group name>  [-n <namespace name>]
```
> [!NOTE]
> Bir PostgreSQL sürüm 11 sunucu grubu oluşturduysanız `kubectl describe postgresql-11/<server group name>` bunun yerine çalıştırın.


Sunucu grubunun yeni tanımını gösterir:

```console
"scheduling": {
      "default": {
        "resources": {
          "limits": {
            "cpu": "4",
            "memory": "1024Mi"
          },
          "requests": {
            "cpu": "2",
            "memory": "512Mi"
          }
        }
      }
    },
```

## <a name="scale-down-the-server-group"></a>Sunucu grubunun ölçeğini azaltma

Sunucu grubunun ölçeğini genişletmek için aynı komutu yürütür, ancak ölçeklendirmek istediğiniz ayarlar için daha az değer ayarlayın. İstekleri ve/veya sınırları kaldırmak için değerini boş dize olarak belirtin.

## <a name="next-steps"></a>Sonraki adımlar

- [PostgreSQL için Azure veritabanı hiper ölçek sunucu grubu ölçeğini genişletme](scale-out-postgresql-hyperscale-server-group.md)
- [Depolama yapılandırması ve Kubernetes depolama kavramları](storage-configuration.md)
- [Kalıcı birim taleplerini genişletme](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#expanding-persistent-volumes-claims)
- [Kubernetes kaynak modeli](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)
