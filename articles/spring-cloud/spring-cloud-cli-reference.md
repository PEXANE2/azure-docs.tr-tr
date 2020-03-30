---
title: az bahar bulutu
description: Azure CLI'yi kullanarak Azure İlke Bulutu'nu yönetme
author: bmitchell287
ms.service: spring-cloud
ms.topic: reference
ms.date: 10/03/2019
ms.author: brendm
ms.openlocfilehash: 33d13d2d4fa9003ef041c4c96be83a69ac595a78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298788"
---
# <a name="az-spring-cloud"></a>az bahar bulutu

## <a name="manage-azure-spring-cloud-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure İlke Bulutu'nu yönetme

>[!Note]
> Azure İlkbahar Bulutu şu anda önizlemede.  Bu komutlar gelecekteki bir sürümde değiştirilebilir veya kaldırılabilir.

| az bahar bulutu |  |
|------|------:|
| [az bahar-bulut oluşturmak](#az-spring-cloud-create) | Bir Azure İlkbahar Bulutu örneği oluşturun. |
| [az bahar-bulut silme](#az-spring-cloud-delete) | Azure İlkbahar Bulutörneğini silin. |
| [az bahar bulutu listesi](#az-spring-cloud-list) | Verilen kaynak grubundaki tüm Azure İlkbahar Bulutörneklerini listele, aksi takdirde abonelik adlarını listele. |
| [az bahar bulutgösterisi](#az-spring-cloud-show) | Azure İlkbahar Bulutu'nun ayrıntılarını gösterin. |

| az bahar bulutu uygulaması | Azure İlkbahar Bulutu'ndaki uygulamaları yönetme komutları.  |
| ---- | ----: |
| [az bahar-bulut uygulaması oluşturmak](#az-spring-cloud-app-create) | Azure İlkbahar Bulutu'nda varsayılan dağıtımla yeni bir uygulama oluşturun. |
| [az bahar-bulut uygulaması silme](#az-spring-cloud-app-delete) | Azure İlkbahar Bulutu'ndaki bir uygulamayı silin. |
| [az bahar-bulut uygulaması dağıtmak](#az-spring-cloud-app-deploy) | Kaynak kodundan veya önceden oluşturulmuş bir ikiliden bir uygulamaya dağıtın ve ilgili yapılandırmaları güncelleyin. |
| [az bahar-bulut uygulama listesi](#az-spring-cloud-app-list) | Azure Bahar Bulutu'ndaki tüm uygulamaları listeleyin. |
| [az bahar-bulut uygulaması yeniden başlat](#az-spring-cloud-app-restart) | Üretim dağıtım varsayılanlarını kullanarak uygulamanın örneklerini yeniden başlatın. |
| [az bahar-bulut uygulama ölçeği](#az-spring-cloud-app-scale) | Bir uygulamayı veya dağıtımlarını el ile ölçeklendirin. |
| [az yay-bulut uygulaması set-dağıtım](#az-spring-cloud-app-set-deployment) | Bir uygulamanın üretim dağıtımını ayarlayın. |
| [az bahar-bulut uygulaması gösterisi](#az-spring-cloud-app-show) | Azure İlkbahar Bulutu'nda bir uygulamanın ayrıntılarını gösterin. |
| [az spring-cloud uygulaması show-deploy-log](#az-spring-cloud-app-show-deploy-log) | Kaynaktan en son dağıtım için yapı günlüklerini göster. Varsayılan olarak üretim dağıtımı için. |
| [az bahar-bulut uygulaması başlangıç](#az-spring-cloud-app-start) | Üretim dağıtım varsayılanlarını kullanarak uygulamanın örneklerini başlatın. |
| [az bahar-bulut uygulaması durdurmak](#az-spring-cloud-app-stop) | Üretim dağıtım varsayılanlarını kullanarak uygulama örneklerini durdurun. |
| [az spring-cloud uygulaması güncellemesi](#az-spring-cloud-app-update) | Belirtilen uygulamanın yapılandırmasını güncelleştirin. |

| az bahar-bulut uygulaması bağlama | Azure Veri Hizmetleri ile bağlamaları yönetme komutları.  Bu ayarlar yürürlüğe girmeden önce uygulamanın yeniden başlatılması gerekir. |
| --- | ---: |
| [az spring-cloud uygulaması bağlama listesi](#az-spring-cloud-app-binding-list) | Bir uygulamadaki tüm hizmet bağlamalarını listele. |
| [az spring-cloud uygulaması bağlama kaldırmak](#az-spring-cloud-app-binding-remove) | Uygulamadan bir hizmet bağlama kaldırın. |
| [az bahar-bulut uygulaması bağlama gösterisi](#az-spring-cloud-app-binding-show) | Hizmet bağlama ayrıntılarını göster. |
| [az bahar-bulut uygulaması bağlama cosmos eklemek](#az-spring-cloud-app-binding-cosmos-add) | Azure CosmosDB'yi uygulamayla bağdın. |
| [az spring-cloud uygulaması bağlama kozmos güncelleme](#az-spring-cloud-app-binding-cosmos-update) | Azure CosmosDB hizmet bağlamayı güncelleştirin. |
| [az spring-cloud uygulaması bağlama mysql ekle](#az-spring-cloud-app-binding-mysql-add) | MySQL için bir Azure Veritabanını uygulamayla bağdın. |
| [az spring-cloud uygulaması bağlama mysql güncellemesi](#az-spring-cloud-app-binding-mysql-update) | MySQL hizmet bağlama için bir Azure Veritabanı güncelleştirme. |
| [az bahar-bulut uygulaması bağlayıcı redis eklemek](#az-spring-cloud-app-binding-redis-add) | Redis için bir Azure Önbelleği'ni uygulamayla birlikte bağdatın. |
| [az spring-cloud uygulaması bağlayıcı redis güncelleme](#az-spring-cloud-app-binding-redis-update) | Redis hizmet bağlama için bir Azure Önbelleği güncelleştirme. |

| az yay-bulut uygulaması dağıtımı | Azure Bahar Bulutu'nda bir uygulamanın dağıtım yaşam döngüsünü yönetme komutları. |
| --- | ---: |
| [az yay-bulut uygulaması dağıtım oluşturmak](#az-spring-cloud-app-deployment-create) | Uygulama için bir hazırlama dağıtımı oluşturun. |
| [az yay-bulut uygulaması dağıtım silme](#az-spring-cloud-app-deployment-delete) | Uygulamanın dağıtımını silin. |
| [az yay-bulut uygulaması dağıtım listesi](#az-spring-cloud-app-deployment-list) | Bir uygulamadaki tüm dağıtımları listele. |
| [az bahar-bulut uygulaması dağıtım gösterisi](#az-spring-cloud-app-deployment-show) | Dağıtımın ayrıntılarını göster. |

| az yay-bulut config-server | Azure Yay BulutConfig Server'ı yönetme komutları. |
| --- | ---: |
| [az yay-bulut config-server net](#az-spring-cloud-config-server-clear) | Config Server'daki tüm ayarları silin. |
| [az yay-bulut config-sunucu seti](#az-spring-cloud-config-server-set) | Config Server'ı bir YAML dosyasından tanımlayın. |
| [az yay-bulut config-server göstermek](#az-spring-cloud-config-server-show) | Config Server yapılandırmayı göster. |
| [az yay-bulut config sunucu git seti](#az-spring-cloud-config-server-git-set) | Config Server için git özelliklerini tanımlayın.  Önceki değerler üzerine yazılır. |
| [az spring-cloud config server git repo ekle](#az-spring-cloud-config-server-git-repo-add) | Config Server'a yeni bir git deposu config ekleyin. |
| [az spring-cloud config server git repo listesi](#az-spring-cloud-config-server-git-repo-list) | Config Server için tüm git deposu configs listeleyin. |
| [az spring-cloud config server git repo kaldır](#az-spring-cloud-config-server-git-repo-remove) | Belirtilen git deposunu Config Server'dan kaldırın. |

| az yay-bulut test-endpoint | Azure İlkbahar Bulutu'nda uç nokta testini yönetme komutları |
| --- | ---: |
| [az yay-bulut test-endpoint devre dışı](#az-spring-cloud-test-endpoint-disable) | Test bitiş noktasını devre dışı. |
| [az yay-bulut test-endpoint etkinleştirmek](#az-spring-cloud-test-endpoint-enable) | Test bitiş noktasını etkinleştirin. |
| [az yay-bulut test-endpoint listesi](#az-spring-cloud-test-endpoint-list) | Test bitiş noktası anahtarlarını listele. |
| [az yay-bulut test-endpoint yenileme-anahtar](#az-spring-cloud-test-endpoint-renew-key) | Bir test bitiş noktası anahtarını yeniden oluşturun. |

## <a name="az-spring-cloud-create"></a>az bahar-bulut oluşturmak

Azure İlkbahar Bulutu'nda varsayılan dağıtımla yeni bir uygulama oluşturun.

```azurecli
az spring-cloud create  --name -n
                        --resource-group -g
                        --location -l
                        --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Bu Azure İlkbahar Bulutu örneğinin adı. |
| --kaynak grubu -g | Bu uygulama için kaynak grubunu belirtir.  Varsayılan grubu kullanarak yapılandırma`az configure --defaults group=<name>` |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --yer -l | Bu uygulama için sunucu konumunu belirtir.  Kullanarak geçerli konumları bulma`az account list-locations` |
| --bekleme | Uzun süren işlemlerin tamamlanması için yapmayın.

### <a name="examples"></a>Örnekler

WestUS'ta yeni bir Azure Bahar Bulutu oluşturun

```azurecli
az spring-cloud create -n MyService -g MyResourceGroup -l westus
```

## <a name="az-spring-cloud-delete"></a>az bahar-bulut silme

Azure İlkbahar Bulutörneğini silin.

```azurecli
az spring cloud --name -n
                --resource-group -g
                --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Silinecek Azure İlkbahar Bulutu örneğinin adı. |
| --kaynak grubu -g | Azure İlkbahar Bulutu'nun ait olduğu kaynak grubunun adı. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| -beklememe | Uzun süren işlemlerin tamamlanmasını beklemeyin. |

### <a name="example"></a>Örnek

'MyResourceGroup'tan 'MyService' adlı bir Azure İlkbahar Bulutu örneğini silin.

```azurecli
az spring-cloud delete -n MyService -g MyResourceGroup
```

## <a name="az-spring-cloud-list"></a>az bahar bulutu listesi

Verilen kaynak grubuyla ilişkili tüm Azure İlkbahar Bulut örneklerini listele. Kaynak grubu belirtilmemişse, abonelik tanımlarını listele.

```azurecli
az spring-cloud list --resource-group -g
```

| Gerekli Parametreler | |
| --- | ---: |
| --kaynak grubu -g | Kaynak grubunun adı. |

## <a name="az-spring-cloud-show"></a>az bahar bulutgösterisi

Belirtilen Azure İlkbahar Bulutu örneğinin ayrıntılarını gösterin.

```azurecli
az spring-cloud show --name -n
                     -- resource-group -g
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Azure İlkbahar Bulutu örneğinin adı. |
| --kaynak grubu -g | Azure İlkbahar Bulutu örneğinin ait olduğu Kaynak Grubu'nun adı.

## <a name="az-spring-cloud-app-create"></a>az bahar-bulut uygulaması oluşturmak

Azure Yay Bulutu'nda yeni bir uygulama oluşturun.

```azurecli
az spring-cloud app create --name -n
                            --resource-group -g
                            --service -s
                            --cpu
                            --enable-persistent-storage
                            --instance-count
                            --is-public
                            --memory
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --cpu | Örnek başına sanal çekirdek sayısı.  Varsayılan: 1. |
| --enable-persistent-depolama | Boolean değeri.  Doğruysa, varsayılan yola sahip bir 50GB disk bağlar. |
| --örnek sayısı | Örnek sayısı.  Varsayılan: 1. |
| --is-kamu | Boolean değeri.  Doğruysa, bir kamu malı atar. |
| --bellek | Örnek başına GB bellek sayısı.  Varsayılan: 1. |

### <a name="examples"></a>Örnekler

Varsayılan yapılandırmaya sahip bir uygulama oluşturun.

```azurecli
az spring-cloud app create -n MyApp -s MyService
```

3 örnekle herkese açık bir uygulama oluşturun.  Her örnekte 3 GB bellek ve 2 CPU çekirdeği vardır.

```azurecli
az spring-cloud app create -n MyApp -s MyService --is-public true --cpu 2 --memory 3
```

## <a name="az-spring-cloud-app-delete"></a>az bahar-bulut uygulaması silme

Azure İlkbahar Bulutu'ndaki bir uygulamayı siler.

```azurecli
az spring cloud app delete  --name -n
                            --resource-group -g
                            --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-deploy"></a>az bahar-bulut uygulaması dağıtmak

Kaynak kodundan veya önceden oluşturulmuş bir ikiliden Bir uygulamayı Azure Bahar Bulutu'na dağıtın ve ilgili yapılandırmaları güncelleyin.

```azurecli
az spring cloud app deploy  --name -n
                            --resource-group -g
                            --service -s
                            --cpu
                            --deployment -d
                            --env
                            --instance-count
                            --jar-path
                            --jvm-options
                            --memory
                            --no-wait
                            --runtime-version
                            --target-module
                            --version
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --cpu | Örnek başına sanal CPI çekirdeği sayısı. |
| --dağıtım -d | Varolan bir uygulama dağıtımının adı.  Belirtilmemişse üretim dağıtımı varsayılan. |
| --env | 'Key[=value]' biçiminde boşluk ayrılmış ortam değişkenleri. |
| --örnek sayısı | Örnek sayısı. |
| --jar-yolu | Sağlanırsa, kavanozu verilen yoldan dağıtın. Aksi takdirde, geçerli klasörü katran olarak dağıtın. |
| --jvm seçenekleri | JVM seçenekleri içeren bir dize.  Kabuk ayrıştma hatalarını önlemek için ' ' yerine '=' kullanın. Örneğin, `--jvm-options='-Xms1024m -Xmx2048m`. |
| --bellek | Örnek başına GB bellek sayısı. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |
| --runtime-version | Uygulamada kullanılan dilin çalışma zamanı sürümü.  İzin verilen `Java_11` `Java_8`değerler: , . |
| --hedef modülü | Çocuk modülü dağıtılacak.  Kaynak kodundan birden çok kavanoz paketi oluşturulduğunda gereklidir. |
| --sürüm | Dağıtım sürümü.  Ayarlanmamışsa değişmeden. |

### <a name="examples"></a>Örnekler

Kaynak kodu bir uygulamaya dağıtın. Bu, geçerli dizini paketleyecek, Pivotal Build Service'i kullanarak bir ikili oluşturacak ve ardından uygulamaya dağıtılacaktır.

```azurecli
az spring-cloud app deploy -n MyApp -s MyService
```

JVM seçeneklerini ve ortam değişkenlerini kullanarak önceden oluşturulmuş bir kavanozu bir uygulamaya dağıtın.

```azurecli
az spring-cloud app deploy -n MyApp -s MyService --jar-path app.jar --jvm-options="-XX:+UseG1GC -XX:+UseStringDeduplication" --env foo=bar
```

Kaynak kodu bir uygulamanın belirli bir dağıtımına dağıtın.

```azurecli
az spring-cloud app deploy -n MyApp -s Myspring-cloud -d green-deployment
```

## <a name="az-spring-cloud-app-list"></a>az bahar-bulut uygulama listesi

Azure Bahar Bulutu örneğindeki tüm uygulamaları listeleyin.

```azurecli
az spring-cloud app list --resource-group -g
                         --service -s
```

|Gerekli Parametreler | |
| --- | ---: |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-restart"></a>az bahar-bulut uygulaması yeniden başlat

Uygulamanın örneklerini yeniden başlatın.  Varsayılan olarak üretim dağıtımı için.

```azurecli
az spring-cloud app restart --name -n
                            --resource-group -g
                            --service -s
                            --deployment -d
                            --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --dağıtım -d | Uygulamanın varolan dağıtımının adı.  Belirtilmemişse üretim dağıtımı varsayılan. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |

## <a name="az-spring-cloud-app-scale"></a>az bahar-bulut uygulama ölçeği

Bir uygulamayı veya dağıtımlarını el ile ölçeklendirin.

```azurecli
az spring-cloud app scale --name -n
                          --resource-group -g
                          --service -s
                          --cpu
                          --deployment -d
                          --instance-count
                          --memory
                          --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --cpu | Uygulama örneği başına sanal CPU çekirdeği sayısı. |
| --dağıtım -d | Uygulamanın varolan dağıtımının adı.  Belirtilmemişse üretim dağıtımı varsayılan. |
| --örnek sayısı | Bu uygulamanın örneklerinin sayısı. |
| --bellek | Uygulama örneği başına GB bellek sayısı. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |

### <a name="examples"></a>Örnekler

Bir uygulamayı örnek başına 4 CPU çekirdeği ve 8 GB bellek olarak ölçeklendirin.

```azurecli
az spring-cloud app scale -n MyApp -s MyService --cpu 3 --memory 8
```

Uygulamanın dağıtımını 5 örneğe ölçeklendirin.

```azurecli
az spring-cloud app scale -n MyApp -s MyService -d green-deployment --instance-count 5
```

## <a name="az-spring-cloud-app-set-deployment"></a>az yay-bulut uygulaması set-dağıtım

Uygulamanın üretim dağıtımı için yapılandırma seçeneklerini ayarlayın.

```azurecli
az spring-cloud app set-deployment --deployment -d
                                   --name -n
                                   --resource-group -g
                                   --service -s
                                   --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --dağıtım -d | Uygulamanın varolan bir dağıtımının adı. |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |

### <a name="examples"></a>Örnekler

Uygulamanın bir hazırlama dağıtımını üretime değiştirin.

```azurecli
az spring-cloud app set-deployment -d green-deployment -n MyApp -s MyService
```

## <a name="az-spring-cloud-app-show"></a>az bahar-bulut uygulaması gösterisi

Azure İlkbahar Bulutu'nda bir uygulamanın ayrıntılarını gösterin.

```azurecli
az spring-cloud app show --name -n
                         --resource-group -g
                         --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-show-deploy-log"></a>az spring-cloud uygulaması show-deploy-log

Kaynak koddan son dağıtımın yapı günlüğünü gösterin.  Varsayılan olarak üretim ortamına.

```azurecli
az spring-cloud app show-deploy-log --name -n
                                    --resource-group -g
                                    --service -s
                                    --deployment -d
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --dağıtım -d | Uygulamanın varolan bir dağıtımının adı.  Varsayılan olarak üretim ortamına. |

## <a name="az-spring-cloud-app-start"></a>az bahar-bulut uygulaması başlangıç

Uygulamanın örneklerini başlatır.  Varsayılan olarak üretim ortamına.

```azurecli
az spring-cloud app start --name -n
                          --resource-group -g
                          --service -s
                          --deployment -d
                          --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --dağıtım -d | Uygulamanın varolan bir dağıtımının adı.  Varsayılan olarak üretim ortamına. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |

## <a name="az-spring-cloud-app-stop"></a>az bahar-bulut uygulaması durdurmak

Uygulamaörneklerini durdurun.  Varsayılan olarak üretim ortamına.

```azurecli
az spring-cloud app stop --name -n
                         --resource-group -g
                         --service -s
                         --deployment -d
                         --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --dağıtım -d | Uygulamanın varolan bir dağıtımının adı.  Varsayılan olarak üretim ortamına. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |

## <a name="az-spring-cloud-app-update"></a>az spring-cloud uygulaması güncellemesi

Bir uygulamanın depolanan yapılandırmasını güncelleştirin.

```azurecli
az spring-cloud app update --name -n
                           --resource-group -g
                           --service -s
                           --deployment -d
                           --enable-persistent-storage
                           --env
                           --is-public
                           --jvm-options
                           --no-wait
                           --runtime-version
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim -n | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --dağıtım -d | Uygulamanın varolan bir dağıtımının adı.  Varsayılan olarak üretim ortamına. |
| --enable-persistent-depolama | Boolean.  Doğruysa, varsayılan yola sahip bir 50GB disk takın. |
| --env | 'Key[=value]' biçiminde boşluk ayrılmış ortam değişkenleri. |
| --is-kamu | Boolean.  Doğruysa, uygulamaya bir kamu malı atayın. |
| --jvm seçenekleri | JVM seçenekleri içeren bir dize.  Kabuk ayrıştma hatalarını önlemek için ' ' yerine '=' kullanın. Örneğin, `--jvm-options='-Xms1024m -Xmx2048m`. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |
| --runtime-version | Uygulamada kullanılan dilin çalışma zamanı sürümü.  İzin verilen `Java_11` `Java_8`değerler: , . |

### <a name="example"></a>Örnek

Uygulama için bir ortam değişkeni ekleyin.

```azurecli
az spring-cloud app update --env foo=bar
```

## <a name="az-spring-cloud-app-binding-list"></a>az spring-cloud uygulaması bağlama listesi

Bir uygulamadaki tüm hizmet bağlamalarını listele.

```azurecli
az spring-cloud app binding list --app
                                 --resource-group -g
                                 --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-binding-remove"></a>az spring-cloud uygulaması bağlama kaldırmak

Uygulamadan bir hizmet bağlama kaldırın.

```azurecli
az spring-cloud app binding list --app
                                 --name -n
                                 --resource-group -g
                                 --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Kaldırılacak hizmet in adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-binding-show"></a>az bahar-bulut uygulaması bağlama gösterisi

Hizmet bağlama ayrıntılarını göster.

```azurecli
az spring-cloud app binding show --app
                                 --name -n
                                 --resource-group -g
                                 --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-binding-cosmos-add"></a>az bahar-bulut uygulaması bağlama cosmos eklemek

Azure Cosmos DB'yi uygulamayla bağdın.

```azurecli
az spring-cloud app binding cosmos add --api-type
                                       --app
                                       --name -n
                                       --resource-group -g
                                       --resource-id
                                       --service -s
                                       --collection-name
                                       --database-name
                                       --key-space
```

```azurecli
az spring-cloud app binding list --app
                                 --name -n
                                 --resource-group -g
                                 --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --api tipi | Aşağıdaki değerlerden birini kullanarak API türünü belirtin: cassandra, gremlin, mongo, sql, tablo. |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

|İsteğe Bağlı Parametreler | |
| --- | ---: |
| --koleksiyon adı | Koleksiyonun adı.  Gremlin kullanırken gereklidir. |
| --veritabanı adı | Veritabanının adı.  Mongo, SQL ve Gremlin kullanırken gereklidir. |
| --anahtar alanı | Cassandra anahtar alanı.  Cassandra kullanırken gereklidir. |

## <a name="az-spring-cloud-app-binding-cosmos-update"></a>az spring-cloud uygulaması bağlama kozmos güncelleme

```azurecli
az spring-cloud app binding cosmos update --app
                                          --name -n
                                          --resource-group -g
                                          --service -s
                                          --collection-name
                                          --database-name
                                          --key-space
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

|İsteğe Bağlı Parametreler | |
| --- | ---: |
| --koleksiyon adı | Koleksiyonun adı.  Gremlin kullanırken gereklidir. |
| --veritabanı adı | Veritabanının adı.  Mongo, SQL ve Gremlin kullanırken gereklidir. |
| --anahtar alanı | Cassandra anahtar alanı.  Cassandra kullanırken gereklidir. |

## <a name="az-spring-cloud-app-binding-mysql-add"></a>az spring-cloud uygulaması bağlama mysql ekle

```azurecli
az spring-cloud app binding mysql add --app
                                      --database-name
                                      --key
                                      --name -n
                                      --resource-group -g
                                      --service -s
                                      --username
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --veritabanı adı | Veritabanının adı. |
| --anahtar | Hizmetin API anahtarı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --kaynak-id | Bağlatılabilmek için hizmetin Azure kaynak kimliği. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |
| --username | Veritabanı erişimi için kullanıcı adı. |

## <a name="az-spring-cloud-app-binding-mysql-update"></a>az spring-cloud uygulaması bağlama mysql güncellemesi

Hizmet bağlama bağlantısını uygulamanın MySQL için bir Azure Veritabanına güncelleştirin.

```azurecli
az spring-cloud app binding mysql update --add
                                         --name -n
                                         --resource-group -g
                                         --service -s
                                         --database-name
                                         --key
                                         --username
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --veritabanı adı | Veritabanının adı. |
| --anahtar | Hizmetin API anahtarı. |
| --username | Veritabanı erişimi için kullanıcı adı. |

## <a name="az-spring-cloud-app-binding-redis-add"></a>az bahar-bulut uygulaması bağlayıcı redis eklemek

Redis için bir Azure Önbelleği'ni uygulamayla birlikte bağdatın.

```azurecli
az spring-cloud app binding redis add --app
                                      --name -n
                                      --resource-group -g
                                      --resource-id
                                      --service -s
                                      --disable-ssl
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --kaynak-id | Bağlamak istediğiniz hizmetin Azure kaynak kimliği. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --devre dışı-ssl | TLS'yi devre dışı kındırın. |

## <a name="az-spring-cloud-app-binding-redis-update"></a>az spring-cloud uygulaması bağlayıcı redis güncelleme

Redis için Azure Önbelleği için bir hizmet bağlamayı güncelleştirin.

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --devre dışı-ssl | TLS'yi devre dışı kındırın. |

## <a name="az-spring-cloud-app-deployment-create"></a>az yay-bulut uygulaması dağıtım oluşturmak

Uygulama için bir hazırlama dağıtımı oluşturun.

Kodu dağıtmak veya ayarları varolan bir `az spring-cloud app deploy --deployment <staging-deployment>` dağıtımda güncellemek için, 'az yay bulutu uygulaması güncelleştirmesi --dağıtım. <staging deployment>

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Hizmet bağlama adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --cpu | Örnek başına sanal CPU çekirdeği sayısı.  Varsayılan değer: 1 |
| --env | 'Key[=value]' biçiminde boşluk ayrılmış ortam değişkenleri. |
| --örnek sayısı | Örnek sayısı. Varsayılan: 1. |
| --jar-yolu | Eğer sağlanırsa, kavanozu dağıtın.  Aksi takdirde, geçerli klasörü katran olarak dağıtın. |
| --jvm seçenekleri | JVM seçenekleri içeren bir dize.  Kabuk ayrıştma hatalarını önlemek için ' ' yerine '=' kullanın. Örneğin, `--jvm-options='-Xms1024m -Xmx2048m`. |
| --bellek | Örnek başına GB bellek sayısı. |
| --bekleme | Uzun süren işlemlerin tamamlanmasını beklemeyin. |
| --runtime-version | Uygulamada kullanılan dilin çalışma zamanı sürümü.  İzin verilen `Java_11` `Java_8`değerler: , . |
| --atlama-klon ayarları | Geçerli üretim dağıtım ayarlarını klonlayarak bir hazırlama dağıtımı oluşturun. |
| --hedef modülü | Çocuk modülü dağıtılacak.  Kaynak kodundan birden çok kavanoz paketi oluşturulduğunda gereklidir. |
| --sürüm | Dağıtım sürümü.  Ayarlanmamışsa değişmeden. |

### <a name="examples"></a>Örnekler

Uygulamanın yeni dağıtımına kaynak kodu dağıtın.  Bu, geçerli dizini paketleyecek, Pivotal Build System'i kullanarak bir ikili oluşturacak ve sonra dağıtacaktır.

```azurecli
az spring-cloud app deployment create -n green-deployment --app MyApp -s MyService
```

JVM seçenekleri ve ortam değişkenleri içeren bir uygulamaya önceden oluşturulmuş bir kavanoz dağıtın.

```azurecli
az spring-cloud app deployment create -n green-deployment --app MyApp -s MyService --jar-path app.jar --jvm-options="-XX:+UseStringDeDuplication" --env foo=bar
```

## <a name="az-spring-cloud-app-deployment-delete"></a>az yay-bulut uygulaması dağıtım silme

Uygulamanın dağıtımını silin.

```azurecli
az spring-cloud app deployment delete --app
                                      --name -n
                                      --resource-group -g
                                      --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Dağıtımın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-deployment-list"></a>az yay-bulut uygulaması dağıtım listesi

Bir uygulamadaki tüm dağıtımları listele.

```azurecli
az spring-cloud app deployment list --app
                                    --resource-group -g
                                    --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-app-deployment-show"></a>az bahar-bulut uygulaması dağıtım gösterisi

Dağıtımın ayrıntılarını göster.

```azurecli
az spring-cloud app deployment show --app
                                    --name -n
                                    --resource-group -g
                                    --service -s
```

| Gerekli Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --isim | Dağıtımın adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --hizmet -s | Azure Bahar Bulutu'nun adı.  Varsayılan hizmeti kullanarak `az configure --defaults spring-cloud=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-config-server-clear"></a>az yay-bulut config-server net

Config Server'daki tüm yapılandırma ayarlarını silin.

```azurecli
az spring-cloud config-server clear --name
                                        --resource-group -g
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-config-server-set"></a>az yay-bulut config-sunucu seti

Yaml dosyasını kullanarak Config Server'daki yapılandırma ayarlarını ayarlayın.

```azurecli
az spring-cloud config-server set --config-file
                                  --name -n
                                  --resource-group -g
                                  --no-wait
```

| Gerekli Parametreler | |
| --- | ---: |
| --config-dosya | Config Server yapılandırması için bir YAML bildirimine dosya yolu. |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --bekleme | Uzun süren işlemlerin tamamlanması için yapmayın.

## <a name="az-spring-cloud-config-server-show"></a>az yay-bulut config-server göstermek

Config Server ayarlarını göster.

```azurecli
az spring-cloud config-server show --name -n
                                   --resource-group -g
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-config-server-git-set"></a>az yay-bulut config-server git seti

Config Server için git özelliklerini ayarlayın.  Bu, varolan tüm git özelliklerinin üzerine yazılır.

```azurecli
az spring-cloud config-server git set --name -n
                                      --resource-group g
                                      --uri
                                      --defer
                                      --host-key
                                      --host-key-algorithm
                                      --label
                                      --password
                                      --private-key
                                      --search-paths
                                      --strict-host-key-checking
                                      --username
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --uri | Uri ekconfig. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --erteleme | Nesneyi Azure'a göndermek yerine geçici olarak yerel önbellekte saklayın.  Görüntülemek `az cache` için kullanın / temizleyin. |
| --ana bilgisayar anahtarı | Eklenen config için ana bilgisayar anahtarı. |
| --ana bilgisayar-anahtar algoritması | Eklenen config için ana bilgisayar anahtar algoritması. |
| --etiket | Eklenen config etiketi. |
| --parola | Eklenen config'in şifresi. |
| --özel anahtar | Eklenen config özel anahtarı. |
| --arama yolları | Eklenen config arama yolları.  Birden çok yol için virgül sınırlayıcıları kullanın. |
| --sıkı-host-anahtar denetimi | Eklenen config'in sıkı ana bilgisayar anahtarı denetimini sağlar. |
| --username | Eklenen config kullanıcı adı. |

## <a name="az-spring-cloud-config-server-git-repo-add"></a>az yay-bulut config-server git repo eklemek

```azurecli
az spring-cloud config-server git repo add --name -n
                                           --repo-name
                                           --resource-group -g
                                           --uri
                                           --defer
                                           --host-key
                                           --host-key-algorithm
                                           --label
                                           --password
                                           --pattern
                                           --private-key
                                           --search-paths
                                           --strict-host-key-checking
                                           --username
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --repo adı | Repo'nun URI'si. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --uri | Uri ekconfig. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --erteleme | Nesneyi Azure'a göndermek yerine geçici olarak yerel önbellekte saklayın.  Görüntülemek `az cache` için kullanın / temizleyin. |
| --ana bilgisayar anahtarı | Eklenen config için ana bilgisayar anahtarı. |
| --ana bilgisayar-anahtar algoritması | Eklenen config için ana bilgisayar anahtar algoritması. |
| --etiket | Eklenen config etiketi. |
| --parola | Eklenen config'in şifresi. |
| --desen | Repo için desen.  Birden çok yol için virgül sınırlayıcıları kullanın.|
| --özel anahtar | Eklenen config özel anahtarı. |
| --arama yolları | Eklenen config arama yolları.  Birden çok yol için virgül sınırlayıcıları kullanın. |
| --sıkı-host-anahtar denetimi | Eklenen config'in sıkı ana bilgisayar anahtarı denetimini sağlar. |
| --username | Eklenen config kullanıcı adı. |

## <a name="az-spring-cloud-config-server-git-repo-list"></a>az yay-bulut config-server git repo listesi

Config Server'da tanımlanan tüm git depolarını listele

```azurecli
az spring-cloud config-server git repo list --name -n
                                       --resource-group -g
                                       --defer
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --erteleme | Nesneyi Azure'a göndermek yerine geçici olarak yerel önbellekte saklayın.  Görüntülemek `az cache` için kullanın / temizleyin. |

## <a name="az-spring-cloud-config-server-git-repo-remove"></a>az yay-bulut config-server git repo kaldırmak

Varolan bir git repo yapılandırmasını Config Server'dan kaldırın.

```azurecli
az spring-cloud config-server git repo remove --name -n
                                         --repo-name
                                         --resource-group -g
                                         --defer
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --repo adı | Repo'nun URI'si. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --erteleme | Nesneyi Azure'a göndermek yerine geçici olarak yerel önbellekte saklayın.  Görüntülemek `az cache` için kullanın / temizleyin. |

## <a name="az-spring-cloud-test-endpoint-disable"></a>az yay-bulut test-endpoint devre dışı

Azure İlkbahar Bulutu'nun test bitiş noktasını devre dışı

```azurecli
az spring-cloud test-endpoint disable --name -n
                                      --resource-group -g
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-test-endpoint-enable"></a>az yay-bulut test-endpoint etkinleştirmek

Azure İlkbahar Bulutu için test bitiş noktasını etkinleştirin. 

```azurecli
az spring-cloud test-endpoint enable --name -n
                                     --resource-group -g
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

## <a name="az-spring-cloud-test-endpoint-list"></a>az yay-bulut test-endpoint listesi 

Azure İlkbahar Bulutu için kullanılabilir test bitiş noktası anahtarlarını listele.

```azurecli
az spring-cloud test-endpoint list --name -n
                                   --resource-group -g
                                   --app
                                   --deployment -d
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |

| İsteğe Bağlı Parametreler | |
| --- | ---: |
| --uygulama | Uygulamanın adı. |
| --dağıtım -d | Uygulamanın varolan bir dağıtımının adı.  Belirtilmemişse varsayılan üretim. |

## <a name="az-spring-cloud-test-endpoint-renew-key"></a>az yay-bulut test-endpoint yenileme-anahtar

Azure Bahar Bulutu için bir test bitiş noktası anahtarını yeniden oluşturun.

```azurecli
az spring-cloud test-endpoint renew-key --name -n
                                        --resource-group -g
                                        --type
```

| Gerekli Parametreler | |
| --- | ---: |
| --isim | Azure Bahar Bulutu'nun adı. |
| --kaynak grubu -g | Kaynak grubunun adı.  Varsayılan grubu kullanarak `az configure --defaults group=<name>`yapılandırabilirsiniz. |
| --türü | Test bitiş noktası anahtarı türü.  İzin verilen değerler: Birincil, İkincil. |
