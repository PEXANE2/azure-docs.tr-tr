---
title: Yaygın sorunları giderme
description: Azure Kapsayıcı Örnekleri dağıtırken, çalıştırırken veya yönetirken sık karşılaşılan sorunları nasıl gidereceklerinizi öğrenin
ms.topic: article
ms.date: 09/25/2019
ms.custom: mvc
ms.openlocfilehash: 07cdbfb27aaf9076e726ebda861ed24996e10135
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533387"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Azure Container Instances'taki sık karşılaşılan sorunları giderme

Bu makalede, kapsayıcıları azure kapsayıcı örneklerine yönetmek veya dağıtmak için sık karşılaşılan sorunları nasıl giderileceği gösterilmektedir. Ayrıca bakınız [sık sorulan sorular](container-instances-faq.md).

Ek desteğe ihtiyacınız varsa, [Azure portalında](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)kullanılabilir **Yardım + destek** seçeneklerine bakın.

## <a name="issues-during-container-group-deployment"></a>Kapsayıcı Grubu dağıtımı sırasındaki sorunlar
### <a name="naming-conventions"></a>Adlandırma kuralları

Kapsayıcı belirtiminizi tanımlarken, belirli parametreler adlandırma kısıtlamalarına uyulmasını gerektirir. Aşağıda kapsayıcı grup özellikleri için özel gereksinimleri olan bir tablodur. Azure adlandırma kuralları hakkında daha fazla bilgi için Azure Mimari Merkezi'ndeki [Adlandırma kurallarına][azure-name-restrictions] bakın.

| Kapsam | Uzunluk | Büyük/Küçük Harf Kullanımı | Geçerli karakterler | Önerilen düzen | Örnek |
| --- | --- | --- | --- | --- | --- |
| Kapsayıcı grup adı | 1-64 |Büyük/Küçük harfe duyarsız |Alfasayısal ve ilk veya son karakter dışında herhangi bir yerde tire |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Kapsayıcı adı | 1-64 |Büyük/Küçük harfe duyarsız |Alfasayısal ve ilk veya son karakter dışında herhangi bir yerde tire |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Konteyner bağlantı noktaları | 1 ile 65535 arası |Tamsayı |1 ile 65535 arası bir sonsayı |`<port-number>` |`443` |
| DNS ad etiketi | 5-63 |Büyük/Küçük harfe duyarsız |Alfasayısal ve ilk veya son karakter dışında herhangi bir yerde tire |`<name>` |`frontend-site1` |
| Ortam değişkeni | 1-63 |Büyük/Küçük harfe duyarsız |Alfasayısal ve ilk veya son karakter dışında herhangi bir yerde (_) altını çizin |`<name>` |`MY_VARIABLE` |
| Birim adı | 5-63 |Büyük/Küçük harfe duyarsız |Küçük harfler ve sayılar ve tireler ilk veya son karakter dışında herhangi bir yerde. Ardışık iki tire içeremez. |`<name>` |`batch-output-volume` |

### <a name="os-version-of-image-not-supported"></a>Görüntünün işletim sistemi sürümü desteklenmiyor

Azure Kapsayıcı Örnekleri'nin desteklemediği bir resim belirtirseniz, bir `OsVersionNotSupported` hata döndürülür. Hata, dağıtmaya denediğiniz görüntünün adı aşağıdakilere `{0}` benzer:

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

Bu hata, desteklenmeyen Yarı Yıllık Kanal sürümü 1709 veya 1803'e dayanan Windows görüntülerini dağıtırken en sık karşılaşılan hatadır. Azure Kapsayıcı Örnekleri'nde desteklenen Windows görüntüleri için [sık sorulan sorulara](container-instances-faq.md#what-windows-base-os-images-are-supported)bakın.

### <a name="unable-to-pull-image"></a>Görüntü çekilemiyor

Azure Kapsayıcı Örnekleri başlangıçta resminizi çekemiyorsa, bir süre için yeniden çalışır. Görüntü çekme işlemi başarısız olmaya devam ederse, ACI sonunda dağıtımbaşarısız `Failed to pull image` olur ve bir hata görebilirsiniz.

Bu sorunu gidermek için kapsayıcı örneğini silin ve dağıtımınızı yeniden deneyin. Resmin kayıt defterinde olduğundan ve resim adını doğru yazdığından emin olun.

Görüntü çekilemezse, aşağıdaki gibi olaylar [az kapsayıcı gösterisinin][az-container-show]çıktısında gösterilir:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"mcr.microsoft.com/azuredocs/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```
### <a name="resource-not-available-error"></a>Kaynak yok hatası

Azure'daki çeşitli bölgesel kaynak yükü nedeniyle, bir kapsayıcı örneğini dağıtmaya çalışırken aşağıdaki hatayı alabilirsiniz:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Bu hata, dağıtmaya çalıştığınız bölgedeki ağır yük nedeniyle, kapsayıcınız için belirtilen kaynakların o anda tahsis edilemediğini gösterir. Sorununuzu çözmeye yardımcı olmak için aşağıdaki azaltma adımlarından birini veya birkaçını kullanın.

* Kapsayıcı dağıtım ayarlarınızın Azure Kapsayıcı [Örnekleri için Bölge kullanılabilirliği'nde](container-instances-region-availability.md) tanımlanan parametrelere uygun olduğunu doğrulayın
* Kapsayıcı için daha düşük CPU ve bellek ayarlarını belirtin
* Farklı bir Azure bölgesine dağıtma
* Daha sonraki bir zamanda dağıtma

## <a name="issues-during-container-group-runtime"></a>Kapsayıcı Grubu çalışma süresi sırasında sorunlar
### <a name="container-continually-exits-and-restarts-no-long-running-process"></a>Konteyner sürekli çıkar ve yeniden başlatır (uzun süren bir işlem yok)

Kapsayıcı grupları **her zaman**yeniden [başlatma ilkesi](container-instances-restart-policy.md) varsayılan, bu nedenle kapsayıcı grubunda kapsayıcı kapsayıcılar her zaman tamamlanmak için çalıştırdıktan sonra yeniden başlatın. Görev tabanlı kapsayıcıları çalıştırmak istiyorsanız bunu **OnFailure** veya **Never** olarak değiştirmeniz gerekebilir. **OnFailure'ı** belirtir ve hala sürekli yeniden başlatmaları görürseniz, kapsayıcınızda yürütülen uygulama veya komut dosyasıyla ilgili bir sorun olabilir.

Uzun süren işlemler olmadan konteyner gruplarını çalıştırırken tekrarlanan çıkışları görebilir ve Ubuntu veya Alpine gibi görüntülerle yeniden başlatabilirsiniz. Konteynerin onu canlı tutma işlemi olmadığı için [EXEC](container-instances-exec.md) üzerinden bağlantı çalışmaz. Bu sorunu gidermek için, kapsayıcıyı çalışır durumda tutmak için kapsayıcı grubu dağıtımınızla aşağıdaki gibi bir başlangıç komutu ekleyin.

```azurecli-interactive
## Deploying a Linux container
az container create -g MyResourceGroup --name myapp --image ubuntu --command-line "tail -f /dev/null"
```

```azurecli-interactive 
## Deploying a Windows container
az container create -g myResourceGroup --name mywindowsapp --os-type Windows --image mcr.microsoft.com/windows/servercore:ltsc2019
 --command-line "ping -t localhost"
```

Kapsayıcı Örnekleri API ve Azure `restartCount` portalı bir özellik içerir. Bir kapsayıcının yeniden başlatma sayısını denetlemek için Azure CLI'deki [az kapsayıcı göster][az-container-show] komutunu kullanabilirsiniz. Aşağıdaki örnek çıktıda (kısalık için kesilen) çıktının sonundaki `restartCount` özelliği görebilirsiniz.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Linux dağıtımları için çoğu kapsayıcı görüntüsü varsayılan komut olarak bash gibi bir kabuk ayarlar. Kendi başına bir kabuk uzun süren bir hizmet olmadığından, bu kapsayıcılar hemen çıkar ve varsayılan **Her zaman** yeniden başlatma ilkesi ile yapılandırıldığında yeniden başlatma döngüsüne girer.

### <a name="container-takes-a-long-time-to-start"></a>Konteynerin başlaması uzun zaman alır

Azure Kapsayıcı Örnekleri'nde kapsayıcı başlatma süresine katkıda bulunan üç temel etken şunlardır:

* [Görüntü boyutu](#image-size)
* [Görüntü konumu](#image-location)
* [Önbelleğe alınmış görüntüler](#cached-images)

Windows [görüntülerinin ek değerlendirmeleri](#cached-images)vardır.

#### <a name="image-size"></a>Görüntü boyutu

Kapsayıcınızın başlaması uzun sürüyorsa, ancak sonunda başarılı olacaksa, kapsayıcı görüntünüzün boyutuna bakarak başlayın. Azure Kapsayıcı Örnekleri kapsayıcı resminizi isteğe bağlı çektiğinden, gördüğünüz başlangıç süresi boyutuyla doğrudan ilişkilidir.

Docker CLI'deki komutu `docker images` kullanarak konteyner görüntünüzün boyutunu görüntüleyebilirsiniz:

```console
$ docker images
REPOSITORY                                    TAG       IMAGE ID        CREATED          SIZE
mcr.microsoft.com/azuredocs/aci-helloworld    latest    7367f3256b41    15 months ago    67.6MB
```

Görüntü boyutlarını küçük tutmanın anahtarı, son görüntünüzün çalışma zamanında gerekli olmayan hiçbir şey içermemesini sağlamaktır. Bunu yapmanın bir yolu [çok aşamalı yapılar][docker-multi-stage-builds]ile. Çok aşamalı yapılar, son görüntünün yalnızca uygulamanız için gereken yapıları ve oluşturma zamanında gereken ekstra içeriği içermemesini kolaylaştırır.

#### <a name="image-location"></a>Görüntü konumu

Görüntü çekmenin kapsayıcınızın başlangıç süresi üzerindeki etkisini azaltmanın bir diğer yolu da, kapsayıcı örneklerini dağıtmayı planladığınız aynı bölgedeki [Azure Konteyner Kayıt Defteri'nde](/azure/container-registry/) kapsayıcı görüntüsünü barındırmaktır. Bu, kapsayıcı görüntüsünün seyahat etmesi gereken ağ yolunu kısaltarak indirme süresini önemli ölçüde kısaltır.

#### <a name="cached-images"></a>Önbelleğe alınmış görüntüler

Azure Kapsayıcı Örnekleri, ortak [Windows temel görüntüleri](container-instances-faq.md#what-windows-base-os-images-are-supported)(, `nanoserver:1809`ve `servercore:ltsc2019`. `servercore:1809` Yaygın olarak kullanılan Linux `ubuntu:1604` `alpine:3.6` görüntüleri gibi ve aynı zamanda önbelleğe alınır. Önbelleğe alınmış görüntülerin ve etiketlerin güncel bir listesi için [Önbelleğe Alınmış Görüntüler Listesi][list-cached-images] API'sini kullanın.

> [!NOTE]
> Azure Kapsayıcı Örnekleri'nde Windows Server 2019 tabanlı görüntülerin kullanımı önizlemededir.

#### <a name="windows-containers-slow-network-readiness"></a>Windows kapsayıcıları ağ hazırlık durumunu yavaşlatıyor

İlk oluşturmada, Windows kapsayıcılarında 30 saniyeye kadar (veya nadir durumlarda daha uzun) gelen veya giden bağlantı olmayabilir. Kapsayıcı uygulamanızın Internet bağlantısına ihtiyacı varsa, Internet bağlantısını oluşturmak için 30 saniye izin vermek için gecikme ve yeniden deneme mantığı ekleyin. İlk kurulumdan sonra, kapsayıcı ağ uygun şekilde devam etmelidir.

### <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>Temel Docker API'sine bağlanamıyor veya ayrıcalıklı kapsayıcılar çalıştıramıyor

Azure Kapsayıcı Örnekleri, kapsayıcı gruplarını barındıran temel altyapıya doğrudan erişimi göstermez. Buna, konteynerin ana bilgisayarında çalışan ve ayrıcalıklı kapsayıcıları çalıştıran Docker API'sine erişim de dahildir. Docker etkileşimine ihtiyacınız varsa, ACI API'nin neyi desteklediğini görmek için [REST başvuru belgelerini](https://aka.ms/aci/rest) kontrol edin. Eksik bir şey varsa, [ACI geri bildirim forumlarında](https://aka.ms/aci/feedback)bir istek gönderin.

### <a name="container-group-ip-address-may-not-be-accessible-due-to-mismatched-ports"></a>Konteyner grubu IP adresine eşleşmeyan bağlantı noktaları nedeniyle erişilemeyebilir

Azure Kapsayıcı Örnekleri henüz normal docker yapılandırması gibi bağlantı noktası eşlemesi desteklemez. Bir kapsayıcı grubunun IP adresine olması gerektiğine inandığınız zaman erişilemediğini düşünüyorsanız, kapsayıcı `ports` resminizi, imle birlikte kapsayıcı grubunuzda ortaya çıkardığınız aynı bağlantı noktalarını dinleyecek şekilde yapılandırdığınızdan emin olun.

Azure Kapsayıcı Örnekleri'nin kapsayıcı resminizde yapılandırdığınız bağlantı noktasını dinleyebileceğine onay `aci-helloworld` vermek istiyorsanız, görüntünün bağlantı noktasını ortaya çıkaran dağıtımını test edin. `aci-helloworld` Ayrıca, uygulamayı bağlantı noktasında dinleyecek şekilde çalıştırın. `aci-helloworld`dinlediği varsayılan bağlantı `PORT` noktası 80'i geçersiz kılmak için isteğe bağlı bir ortam değişkeni kabul eder. Örneğin, 9000 no'l'u test etmek için kapsayıcı grubunu oluştururken [ortam değişkenini](container-instances-environment-variables.md) ayarlayın:

1. Bağlantı noktası 9000'i ortaya çıkarmak için kapsayıcı grubunu ayarlayın ve bağlantı noktası numarasını ortam değişkeninin değeri olarak geçirin. Örnek Bash kabuğu için biçimlendirilmiştir. PowerShell veya Komut İstemi gibi başka bir kabuk tercih ederseniz, değişken atamayı buna göre ayarlamanız gerekir.
    ```azurecli
    az container create --resource-group myResourceGroup \
    --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --ip-address Public --ports 9000 \
    --environment-variables 'PORT'='9000'
    ```
1. 'nin komut çıkışında konteyner grubunun IP `az container create`adresini bulun. **ip**değerini arayın . 
1. Kapsayıcı başarıyla sağlandıktan sonra, örneğin tarayıcınızdaki kapsayıcı uygulamasının IP adresine ve `192.0.2.0:9000`bağlantı noktasına göz atın: . 

    "Azure Kapsayıcı Örneklerine Hoş Geldiniz!" web uygulaması tarafından görüntülenen mesaj.
1. Kapsayıcıile işimiz bittiğinde, komutu `az container delete` kullanarak kaldırın:

    ```azurecli
    az container delete --resource-group myResourceGroup --name mycontainer
    ```

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcılarınızı ayıklamanıza yardımcı olmak için [kapsayıcı günlüklerini ve olayları](container-instances-get-logs.md) nasıl alsüreceğinizi öğrenin.

<!-- LINKS - External -->
[azure-name-restrictions]: https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#naming-and-tagging-resources
[windows-sac-overview]: https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/_/microsoft-windows-servercore
[docker-hub-windows-nano]: https://hub.docker.com/_/microsoft-windows-nanoserver

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az-container-show
[list-cached-images]: /rest/api/container-instances/listcachedimages
