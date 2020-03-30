---
title: Öğretici - ACR görevi zamanlama
description: Bu öğreticide, bir veya daha fazla zamanlayıcı tetikleyicisi ayarlayarak belirli bir zamanlamada Azure Kapsayıcı Kayıt Defteri Görevi'ni nasıl çalıştırılayarılabilirsiniz
ms.topic: article
ms.date: 06/27/2019
ms.openlocfilehash: 3202b5d8c426165d81129f1affa69b3a3d515ce9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402870"
---
# <a name="run-an-acr-task-on-a-defined-schedule"></a>Tanımlı bir zamanlamada bir ACR görevi çalıştırma

Bu öğretici, bir Zamanlama'da Bir [ACR Görevi'ni](container-registry-tasks-overview.md) nasıl çalıştırabileceğinizi gösterir. Bir veya daha fazla *zamanlayıcı tetikleyicisini*ayarlayarak bir görev zamanlayın. Zamanlayıcı tetikleyicileri tek başına veya diğer görev tetikleyicileriyle birlikte kullanılabilir.

Bu eğitimde, görevleri zamanlama ve:

> [!div class="checklist"]
> * Zamanlayıcı tetikleyicisi ile görev oluşturma
> * Zamanlayıcı tetikleyicilerini yönetme

Görev zamanlama aşağıdaki gibi senaryolar için yararlıdır:

* Zamanlanmış bakım işlemleri için bir kapsayıcı iş yükü çalıştırın. Örneğin, kayıt defterinizden gereksiz görüntüleri kaldırmak için kapsayıcıbir uygulama çalıştırın.
* Canlı site izlemenizin bir parçası olarak iş günü boyunca bir üretim görüntüsü üzerinde bir dizi test çalıştırın.

Bu makaledeki örnekleri çalıştırmak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.68 veya daha sonra gereklidir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].


## <a name="about-scheduling-a-task"></a>Görev zamanlama hakkında

* **Cron ifadesi ile tetikleme** - Görev için zamanlayıcı tetikleyici bir *cron ifadesi*kullanır. İfade, görevi tetiklemek için dakikayı, saati, günü, ayı ve haftanın gününü belirten beş alana sahip bir dizedir. Dakikada bir kereye kadar frekanslar desteklenir.

  Örneğin, ifade `"0 12 * * Mon-Fri"` hafta içi her gün UTC'de öğlen bir görevi tetikler. Bu makalenin [ilerleyen](#cron-expressions) bilgilerine bakın.
* **Birden çok zamanlayıcı tetikleyicisi** - Zamanlamalar farklı olduğu sürece göreve birden çok zamanlayıcı eklenmesine izin verilir.
    * Görevi oluştururken birden çok zamanlayıcı tetikleyicisi belirtin veya daha sonra ekleyin.
    * İsteğe bağlı olarak daha kolay yönetim için tetikleyicileri adlandırır veya ACR Görevleri varsayılan tetikleyici adları sağlar.
    * Zamanlayıcı zamanlamaları aynı anda çakışıyorsa, ACR Görevleri görevi her zamanlayıcı için zamanlanan zamanda tetikler.
* **Diğer görev tetikleyicileri** - Zamanlayıcı tarafından tetiklenen bir görevde, [kaynak kodu işleme](container-registry-tutorial-build-task.md) veya [temel görüntü güncelleştirmelerini](container-registry-tutorial-base-image-update.md)temel alan tetikleyicileri de etkinleştirebilirsiniz. Diğer ACR görevleri gibi, zamanlanmış bir görevi [de el ile tetikleyebilirsiniz.][az-acr-task-run]

## <a name="create-a-task-with-a-timer-trigger"></a>Zamanlayıcı tetikleyicisi ile görev oluşturma

[Az acr görev oluşturma][az-acr-task-create] komutu ile bir görev oluşturduğunuzda, isteğe bağlı olarak bir zamanlayıcı tetikleyici sekleyebilirsiniz. Parametre `--schedule` ekleyin ve zamanlayıcı için cron ifadesini geçirin.

Basit bir örnek olarak, aşağıdaki komut, `hello-world` görüntüyü her gün saat 21:00 UTC'de Docker Hub'dan çalıştırmayı tetikler. Görev kaynak kodu bağlamı olmadan çalışır.

```azurecli
az acr task create \
  --name mytask \
  --registry myregistry \
  --cmd hello-world \
  --schedule "0 21 * * *" \
  --context /dev/null
```

Zamanlayıcı tetikleyicisinin yapılandırıldığını görmek için [az acr görev göster][az-acr-task-show] komutunu çalıştırın. Varsayılan olarak, temel görüntü güncelleştirme tetikleyicisi de etkinleştirilir.

```azurecli
az acr task show --name mytask --registry registry --output table
```

```output
NAME      PLATFORM    STATUS    SOURCE REPOSITORY       TRIGGERS
--------  ----------  --------  -------------------     -----------------
mytask    linux       Enabled                           BASE_IMAGE, TIMER
```

Düzgün ayarlandığından emin olmak için [az acr görev çalıştırılayla][az-acr-task-run] görevi el ile tetikle:

```azurecli
az acr task run --name mytask --registry myregistry
```

Kapsayıcı başarılı bir şekilde çalışıyorsa, çıktı aşağıdakilere benzer:

```output
Queued a run with ID: cf2a
Waiting for an agent...
2019/06/28 21:03:36 Using acb_vol_2ca23c46-a9ac-4224-b0c6-9fde44eb42d2 as the home volume
2019/06/28 21:03:36 Creating Docker network: acb_default_network, driver: 'bridge'
[...]
2019/06/28 21:03:38 Launching container with name: acb_step_0

Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

Zamanlanan süreden sonra, zamanlayıcının görevi beklendiği gibi tetiklediğini doğrulamak için [az acr görev listesi çalıştır Komutunu][az-acr-task-list-runs] çalıştırın:

```azurecli
az acr task list-runs --name mytask --registry myregistry --output table
```

Zamanlayıcı başarılı olduğunda, çıktı aşağıdakilere benzer:

```output
RUN ID    TASK     PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  -------- ----------  ---------  ---------  --------------------  ----------
[...]
cf2b      mytask   linux       Succeeded  Timer      2019-06-28T21:00:23Z  00:00:06
cf2a      mytask   linux       Succeeded  Manual     2019-06-28T20:53:23Z  00:00:06
```

## <a name="manage-timer-triggers"></a>Zamanlayıcı tetikleyicilerini yönetme

ACR görevi için zamanlayıcı tetikleyicilerini yönetmek için [az acr görev zamanlayıcısı][az-acr-task-timer] komutlarını kullanın.

### <a name="add-or-update-a-timer-trigger"></a>Zamanlayıcı tetikleyicisi ekleme veya güncelleme

Görev oluşturulduktan sonra, [az acr görev zamanlayıcısı ekle][az-acr-task-timer-add] komutunu kullanarak isteğe bağlı olarak bir zamanlayıcı tetikleyicisi ekleyin. Aşağıdaki örnek, daha önce oluşturulan *mytask'a* bir zamanlayıcı tetikleyici adı *timer2* ekler. Bu zamanlayıcı görevi her gün 10:30 UTC'de tetikler.

```azurecli
az acr task timer add \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 10 * * *"
```

[Az acr görev zamanlayıcısı güncelleştirme][az-acr-task-timer-update] komutunu kullanarak varolan bir tetikleyicinin zamanlamasını güncelleştirin veya durumunu değiştirin. Örneğin, görevi 11:30 UTC'de tetiklemek için *timer2* adlı tetikleyiciyi güncelleştirin:

```azurecli
az acr task timer update \
  --name mytask \
  --registry myregistry \
  --timer-name timer2 \
  --schedule "30 11 * * *"
```

### <a name="list-timer-triggers"></a>Zamanlayıcı tetikleyicilerini listeleme

[az acr görev zamanlayıcı listesi][az-acr-task-timer-list] komutu, görev için ayarlanan zamanlayıcı tetikleyicilerini gösterir:

```azurecli
az acr task timer list --name mytask --registry myregistry
```

Örnek çıktı:

```JSON
[
  {
    "name": "timer2",
    "schedule": "30 11 * * *",
    "status": "Enabled"
  },
  {
    "name": "t1",
    "schedule": "0 21 * * *",
    "status": "Enabled"
  }
]
```

### <a name="remove-a-timer-trigger"></a>Zamanlayıcı tetikleyicisi kaldırma

Görevden bir zamanlayıcı tetikleyicisini kaldırmak için [az acr görev zamanlayıcısı kaldır][az-acr-task-timer-remove] komutunu kullanın. Aşağıdaki örnek, *zamanlayıcı2* tetikleyicisini *mytask'tan*kaldırır:

```azurecli
az acr task timer remove \
  --name mytask \
  --registry myregistry \
  --timer-name timer2
```

## <a name="cron-expressions"></a>Cron ifadeleri

ACR Görevleri cron ifadelerini yorumlamak için [NCronTab](https://github.com/atifaziz/NCrontab) kitaplığını kullanır. ACR Görevleri'nde desteklenen ifadelerin beyaz alanla ayrılmış beş gerekli alanı vardır:

`{minute} {hour} {day} {month} {day-of-week}`

Cron ifadeleri ile kullanılan saat dilimi Eşgüdümlü Evrensel Saat (UTC) idi. Saatler 24 saat biçimindedir.

> [!NOTE]
> ACR Görevleri cron `{second}` `{year}` ifadelerinde veya alanı desteklemez. Başka bir sistemde kullanılan bir cron ifadesini kopyalarsanız, kullanılıyorsa bu alanları kaldırdığınızdan emin olun.

Her alan aşağıdaki değer türlerinden birine sahip olabilir:

|Tür  |Örnek  |Tetiklendiğinde  |
|---------|---------|---------|
|Belirli bir değer |<nobr>`"5 * * * *"`</nobr>|saat 5 dakika geçerken her saat|
|Tüm değerler`*`( )|<nobr>`"* 5 * * *"`</nobr>|05:00 UTC başlayan saatin her dakikası (günde 60 kez)|
|Bir aralık`-` (operatör)|<nobr>`"0 1-3 * * *"`</nobr>|Günde 3 kez, 1:00, 2:00 ve 3:00 UTC|
|Bir değer kümesi`,` (işleç)|<nobr>`"20,30,40 * * * *"`</nobr>|Saatte 3 kez, 20 dakika, 30 dakika ve saati 40 dakika geçe|
|Bir aralık`/` değeri (işleç)|<nobr>`"*/10 * * * *"`</nobr>|Saatte 6 kez, 10 dakika, 20 dakika, ve saire, saati geçti

[!INCLUDE [functions-cron-expressions-months-days](../../includes/functions-cron-expressions-months-days.md)]

### <a name="cron-examples"></a>Cron örnekleri

|Örnek|Tetiklendiğinde  |
|---------|---------|
|`"*/5 * * * *"`|her beş dakikada bir|
|`"0 * * * *"`|her saatin başında bir kez|
|`"0 */2 * * *"`|her iki saatte bir|
|`"0 9-17 * * *"`|9:00-17:00 UTC arasında her saat bir kez|
|`"30 9 * * *"`|9:30 UTC her gün|
|`"30 9 * * 1-5"`|hafta içi her gün 9:30 UTC'de|
|`"30 9 * Jan Mon"`|9:30 UTC Ocak ayında her Pazartesi|

## <a name="clean-up-resources"></a>Kaynakları temizleme

Konteyner kayıt defteri veya kayıt defterleri, konteyner örneği, anahtar kasası ve hizmet sorumlusu da dahil olmak üzere bu öğretici seride oluşturduğunuz tüm kaynakları kaldırmak için aşağıdaki komutları sorun:

```azurecli
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir zamanlayıcı tarafından otomatik olarak tetiklenen Azure Kapsayıcı Kayıt Defteri görevlerininasıl oluşturabileceğinizi öğrendiniz. 

Kayıt defterindeki depoları temizlemek için zamanlanmış bir görev kullanma örneği için, [bkz.](container-registry-auto-purge.md)

Kaynak kodu taahhütleri veya temel görüntü güncelleştirmeleri tarafından tetiklenen görevlerin örnekleri için, [ACR Görevler öğretici serisindeki](container-registry-tutorial-quick-task.md)diğer makalelere bakın.



<!-- LINKS - External -->
[task-examples]: https://github.com/Azure-Samples/acr-tasks


<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-timer]: /cli/azure/acr/task/timer
[az-acr-task-timer-add]: /cli/azure/acr/task/timer#az-acr-task-timer-add
[az-acr-task-timer-remove]: /cli/azure/acr/task/timer#az-acr-task-timer-remove
[az-acr-task-timer-list]: /cli/azure/acr/task/timer#az-acr-task-timer-list
[az-acr-task-timer-update]: /cli/azure/acr/task/timer#az-acr-task-timer-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task]: /cli/azure/acr/task
[azure-cli-install]: /cli/azure/install-azure-cli
