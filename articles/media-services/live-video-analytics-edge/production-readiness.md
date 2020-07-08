---
title: Üretim hazırlığı ve en iyi uygulamalar-Azure
description: Bu makalede, canlı video analizinin üretim ortamlarında IoT Edge modülünde nasıl yapılandırılacağı ve dağıtılacağı hakkında rehberlik sunulmaktadır.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: a5a2ff78d456d4423facdf5f3533ee94bc25bfc4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84261075"
---
# <a name="production-readiness-and-best-practices"></a>Üretim hazırlığı ve en iyi uygulamalar

Bu makalede, canlı video analizinin üretim ortamlarında IoT Edge modülünde nasıl yapılandırılacağı ve dağıtılacağı hakkında rehberlik sunulmaktadır. Ayrıca, IoT Edge çözümünüzü hazırlarken [IoT Edge çözümünüzü üretim makalesinde dağıtmaya yönelik hazırlanın](https://docs.microsoft.com/azure/iot-edge/production-checklist) ' i gözden geçirmeniz gerekir. 

> [!NOTE]
> Kuruluşların BT departmanlarına güvenlik ile ilgili yönlere başvurmalısınız.

## <a name="running-the-module-as-a-local-user"></a>Modülü yerel kullanıcı olarak çalıştırma

Canlı video analizlerini IoT Edge modülünde bir uç cihaza dağıttığınızda, varsayılan olarak yükseltilmiş ayrıcalıklarla çalışır. Bunu yaptığınızda, modüldeki () günlükleri kontrol ederseniz `sudo iotedge logs {name-of-module}` aşağıdakileri görürsünüz:

```
!! production readiness: user accounts – Warning
       LOCAL_USER_ID and LOCAL_GROUP_ID environment variables are not set. The program will run as root!
       For optimum security, make sure to set LOCAL_USER_ID and LOCAL_GROUP_ID environment variables to a non-root user and group.
       See https://aka.ms/lva-iot-edge-prod-checklists-user-accounts for more information.
```

Aşağıdaki bölümlerde yukarıdaki uyarının nasıl ele alındığı ele alınmaktadır.

### <a name="creating-and-using-a-local-user-account"></a>Yerel bir kullanıcı hesabı oluşturma ve kullanma

Canlı video analizlerini, olabildiğince az ayrıcalığa sahip bir hesabı kullanarak üretimde IoT Edge modülünde çalıştırmanız gerekir. Aşağıdaki komutlar, örneğin bir Linux sanal makinesinde nasıl Yerel Kullanıcı hesabı oluşturabileceğiniz gösterilmektedir:

```
sudo groupadd -g 1010 localuser
sudo adduser --home /home/edgeuser --uid 1010 -gid 1010 edgeuser
```

Ardından, dağıtım bildiriminde LOCAL_USER_ID ve LOCAL_GROUP_ID ortam değişkenlerini, kök olmayan kullanıcı ve grup olarak ayarlayabilirsiniz:

```
"lvaEdge": {
"version": "1.0",
…
"env": {
    "LOCAL_USER_ID": 
    {
        "value": "1010"
    },
    "LOCAL_GROUP_ID": {
        "value": "1010"
    }
}
},
…
```

### <a name="granting-permissions-to-device-storage"></a>Cihaz depolamaya izinler veriliyor

IoT Edge modüldeki canlı video analizi, şu durumlarda yerel dosya sistemine dosya yazabilmesini gerektirir:

* Yapılandırma verilerini depolamak için yerel dosya sisteminde bir dizin belirtmeniz gereken modül ikizi özelliği [[ApplicationDataDirectory](module-twin-configuration-schema.md#module-twin-properties)] kullanılıyor.
* Videoyu buluta kaydetmek için bir medya grafiği kullanarak, modül uç cihazında bir dizin kullanılmasını gerektirir (daha fazla bilgi için bkz. [sürekli video kaydı](continuous-video-recording-concept.md) makalesi).
* Kayıtlı video için bir dosya yolu belirtmeniz gereken [yerel bir dosyaya kaydetme](event-based-video-recording-concept.md#video-recording-based-on-events-from-other-sources).

Yukarıdakilerden herhangi birini kullanmak istiyorsanız, yukarıdaki Kullanıcı hesabının ilgili dizine erişiminin olduğundan emin olmanız gerekir. Örneğin, applicationDataDirectory 'yi düşünün. Sınır cihazında bir dizin oluşturabilir ve cihaz depolama alanını modül depolamaya bağlayabilirsiniz. 

```
sudo mkdir /var/local/mediaservices
sudo chown -R edgeuser /var/local/mediaservices
```

Ardından, dağıtım bildiriminde Edge modülünün oluşturma seçeneklerinde, yukarıdaki dizini ("var/yerel/mediaservices/") modül içindeki bir dizine ("/var/lib/azudüzeltici aservices/" gibi) eşleyerek bağlama ayarı ekleyebilirsiniz. Ve applicationDataDirectory değeri olarak son dizini kullanırsınız.

```
"lvaEdge": {
    "version": "1.0",
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
        "image": "mcr.microsoft.com/media/live-video-analytics:1.0",
        "createOptions": "{\"HostConfig\":{\"LogConfig\":{\"Type\":\"\",\"Config\":{\"max-size\":\"10m\",\"max-file\":\"10\"}},\"Binds\":[\"/var/local/mediaservices/:/var/lib/azuremediaservices/\"]}}"
    },
    "env": {
        "LOCAL_USER_ID": 
        {
            "value": "1010"
        },
        "LOCAL_GROUP_ID": {
            "value": "1010"
        }
    }
    },
    …
    
    "lvaEdge": {
    "properties.desired": {
    "applicationDataDirectory": "/var/lib/azuremediaservices",
    …
    }
}
```

[Sürekli video kaydı](continuous-video-recording-tutorial.md)gibi hızlı başlangıç ve öğreticiler için örnek medya grafiklerine baktığınızda, medya önbellek dizini 'Nin (localMediaCachePath) ApplicationDataDirectory altında bir alt dizin kullandığını görürsünüz. Bu, önbellek geçici veriler içerdiğinden önerilen yaklaşımdır.

### <a name="naming-video-assets-or-files"></a>Video varlıklarını veya dosyalarını adlandırma

Medya grafikleri, buluttaki bulutta veya MP4 dosyalarında varlık oluşturulmasına olanak sağlar. Medya varlıkları, [sürekli video kaydı](continuous-video-recording-tutorial.md) veya [olay tabanlı video kaydı](event-based-video-recording-tutorial.md)tarafından oluşturulabilir. Bu varlıklar ve dosyalar istediğiniz şekilde adlandırılmasına karşın, sürekli video kayıt tabanlı medya varlığı için önerilen adlandırma yapısı " &lt; anytext &gt; -$ {System. Graphtopologyıname}-$ {System. graphınstancename}" dir. Örnek olarak, varlık havuzunda assetNamePattern öğesini şu şekilde ayarlayabilirsiniz:

```
"assetNamePattern": "sampleAsset-${System.GraphTopologyName}-${System.GraphInstanceName}
```

Olay tabanlı video kaydı tarafından oluşturulan varlıklar için önerilen adlandırma deseninin " &lt; anytext &gt; -$ {System. DateTime}" olması gerekir. Sistem değişkeni, olaylar aynı anda gerçekleşiyorsa, varlıkların üzerine yazılmamasını sağlar. Örnek olarak, varlık havuzunda assetNamePattern öğesini şu şekilde ayarlayabilirsiniz:

```
"assetNamePattern": "sampleAssetFromEVR-LVAEdge-${System.DateTime}"
```

Aynı grafiğin birden çok örneğini çalıştırıyorsanız, ayırt etmek için Graf topolojisi adını ve örnek adını kullanabilirsiniz. Örnek olarak, varlık havuzunda assetNamePattern öğesini şu şekilde ayarlayabilirsiniz:

```
"assetNamePattern": "sampleAssetFromEVR-${System.GraphTopologyName}-${System.GraphInstanceName} -${System.DateTime}"
```

Kenarda olay tabanlı video kaydı tarafından oluşturulan MP4 video klipleri için, önerilen adlandırma deseninin tarih saat içermesi ve aynı grafiğin birden çok örneği için Graphtopologyıname ve Graphınstancename sistem değişkenlerinin kullanılması önerilir. Örnek olarak, dosya havuzunda Filepathmodel ' i aşağıdaki gibi ayarlayabilirsiniz: 

```
"filePathPattern": "/var/media/sampleFilesFromEVR-${fileSinkOutputName}-${System.DateTime}"
```

Veya 

```
"filePathPattern": "/var/media/sampleFilesFromEVR-${fileSinkOutputName}--${System.GraphTopologyName}-${System.GraphInstanceName} ${System.DateTime}"
```

### <a name="keeping-your-vm-clean"></a>SANAL makinenizin temiz tutulması

Sınır aygıtı olarak kullandığınız Linux sanal makinesi, düzenli aralıklarla yönetilmiyorsa yanıt vermemeye dönüşebilir. Önbellekler temiz tutulması, gereksiz paketleri ortadan kaldırmak ve kullanılmayan kapsayıcıları VM 'den kaldırmak önemlidir. Bunu yapmak için, uç sanal makinenizde kullanabileceğiniz önerilen komutların bir kümesi verilmiştir.

1. `sudo apt-get clean`

    Apt-get clean komutu,/var/cache' de kalan alınan paket dosyalarının yerel deposunu temizler. Temizlediğinde bulunan dizinler/var/cache/apt/Archives/ve/var/cache/apt/Archives/partial/. /Var/cache/apt/Arşivler içinde yer aldığı tek dosya kilit dosyası ve kısmi alt dizin olur. Apt-get clean komutu genellikle, düzenli olarak zamanlanan bakımın bir parçası olarak genellikle disk alanını gereken şekilde temizlemek için kullanılır. Daha fazla bilgi için [apt-get Ile temizlik](https://www.networkworld.com/article/3453032/cleaning-up-with-apt-get.html)bölümüne bakın.
1. `sudo apt-get autoclean`

    Apt-get temizlemesi, apt-get clean gibi, alınan paket dosyalarının yerel deposunu temizler, ancak yalnızca indirilebilecek ve yararlı olmayan dosyaları kaldırır. Önbelleğinizi çok büyük büyümeye karşı korumanıza yardımcı olur.
1. `sudo apt-get autoremove1`

    Otomatik Kaldır seçeneği, başka bir paket gerektiğinden, ancak diğer paketlerin kaldırılmasının artık gerekli olmadığı için otomatik olarak yüklenen paketleri kaldırır
1. `sudo docker image ls`– Uç sisteminizde Docker görüntülerinin listesini sağlar
1. `sudo docker system prune `

    Docker, görüntü, kapsayıcılar, birimler ve ağlar gibi kullanılmayan nesneleri (genellikle "çöp toplama" olarak adlandırılır) temizlemeye yönelik bir koruyucu yaklaşım sağlar: Bu nesneler, açıkça Docker 'a sorun yapmadığınız sürece genellikle kaldırılmaz. Bu, Docker 'ın fazladan disk alanı kullanmasına neden olabilir. Her nesne türü için Docker bir Ayıkla komutu sağlar. Ayrıca, aynı anda birden çok nesne türünü temizlemek için Docker sistem Ayıkla ' yı kullanabilirsiniz. Daha fazla bilgi için [kullanılmayan Docker nesnelerini ayıklama](https://docs.docker.com/config/pruning/)bölümüne bakın.
1. `sudo docker rmi REPOSITORY:TAG`

    Kenar modülünde güncelleştirmeler gerçekleştiği için, Docker 'ın daha eski Edge modülü sürümleri de vardır. Böyle bir durumda, resim sürümü etiketiyle tanımlanan belirli görüntüleri kaldırmak için Docker rmi komutunu kullanmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: IoT Edge kullanmaya başlama-canlı video analizi](get-started-detect-motion-emit-events-quickstart.md)
