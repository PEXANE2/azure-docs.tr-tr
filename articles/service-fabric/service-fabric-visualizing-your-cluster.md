---
title: Azure Hizmet Kumaş Explorer'ı kullanarak kümenizi görselleştirme
description: Service Fabric Explorer, Microsoft Azure Hizmet Kumaşı kümesindeki bulut uygulamalarını ve düğümlerini incelemek ve yönetmek için kullanılan bir uygulamadır.
author: mikkelhegn
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: mikhegn
ms.openlocfilehash: 80e3d990b6e8026c57ffff0048d0447a95529564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258193"
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Service Fabric Explorer ile kümenizi görselleştirme

Service Fabric Explorer (SFX), Azure Hizmet Kumaş kümelerini denetlemek ve yönetmek için açık kaynaklı bir araçtır. Service Fabric Explorer, Windows, macOS ve Linux için bir masaüstü uygulamasıdır.

## <a name="service-fabric-explorer-download"></a>Hizmet Kumaş Explorer indir

Service Fabric Explorer'ı masaüstü uygulaması olarak indirmek için aşağıdaki bağlantıları kullanın:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

- macOS
  - https://aka.ms/sfx-macos

> [!NOTE]
> Service Fabric Explorer'ın masaüstü sürümü küme desteğinden daha fazla veya daha az özelliğe sahip olabilir. Tam özellik uyumluluğunu sağlamak için kümeye dağıtılan Service Fabric Explorer sürümüne geri dönebilirsiniz.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Kümeden Hizmet Kumaş Explorer'ı Çalıştırma

Service Fabric Explorer, Service Fabric cluster'ın HTTP yönetim bitiş noktasında da barındırılır. SFX'i bir web tarayıcısında başlatmak için, herhangi bir tarayıcıdan kümenin HTTP\/yönetim bitiş noktasına göz atın - örneğin https: /clusterFQDN:19080.

Geliştirici iş istasyonu kurulumu için, yerel kümenizde Service Fabric https://localhost:19080/ExplorerExplorer'ı başlatarak . [Geliştirme ortamınızı hazırlamak](service-fabric-get-started.md)için bu makaleye bakın.

> [!NOTE]
> Kümeniz kendi imzasını taşıyan bir sertifika yla korunuyorsa, web tarayıcısından "Bu site güvenli değil" bir hata iletisi alırsınız. Uyarıyı geçersiz kılarak en modern web tarayıcılarında devam edebilirsiniz. Üretim ortamında kümeniz ortak ad ve sertifika yetkilisi sertifika sıfatıyla güvence altına alınmalıdır. 
>
>

## <a name="connect-to-a-service-fabric-cluster"></a>Service Fabric kümesine bağlanma
Service Fabric kümesine bağlanmak için kümeyönetimi bitiş noktası (FQDN/IP) ve HTTP yönetimi bitiş noktası bağlantı noktası (varsayılan olarak 19080) gerekir. Örneğin https\://mysfcluster.westus.cloudapp.azure.com:19080. İş istasyonunuzdaki yerel kümeye bağlanmak için "Localhost'a bağlan" onay kutusunu kullanın.

### <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma
İstemci erişimini Hizmet Kumaşı kümenize sertifikalarınızla veya Azure Etkin Dizini (AAD) kullanarak denetleyebilirsiniz.

Güvenli bir kümeye bağlanmaya çalışırsanız, kümenin yapılandırmasına bağlı olarak bir istemci sertifikası sunmanız veya AAD kullanarak oturum açmanız gerekir.

## <a name="understand-the-service-fabric-explorer-layout"></a>Hizmet Kumaş Explorer düzenini anlama
Soldaki ağacı kullanarak Service Fabric Explorer'da gezinebilirsiniz. Ağacın kökünde, küme panosu, uygulama ve düğüm durumunun özeti de dahil olmak üzere kümenize genel bir bakış sağlar.

![Servis Kumaş Explorer küme panosu][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Kümenin düzenini görüntüleme
Hizmet Kumaşı kümesindeki düğümler, hata etki alanları ve yükseltme etki alanlarının iki boyutlu ızgarasına yerleştirilir. Bu yerleşim, uygulamalarınızın donanım hataları ve uygulama yükseltmeleri varlığında kullanılabilir kalmasını sağlar. Küme eşlemi kullanarak geçerli kümenin nasıl yerleştirilebildiğini görüntüleyebilirsiniz.

![Service Fabric Explorer küme haritası][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Uygulamaları ve hizmetleri görüntüleme
Küme iki alt ağaç içerir: biri uygulamalar için, diğeri düğümler için.

Hizmet Dokusu'nun mantıksal hiyerarşisi arasında gezinmek için uygulama görünümünü kullanabilirsiniz: uygulamalar, hizmetler, bölümler ve yinelemeler.

Aşağıdaki örnekte, uygulama **MyApp** iki hizmetoluşur, **MyStatefulService** ve **WebService.** **MyStatefulService** durumlu olduğundan, bir birincil ve iki ikincil yinelemeleri ile bir bölüm içerir. Buna karşılık, WebSvcService devletsiz ve tek bir örnek içerir.

![Servis Kumaş Explorer uygulama görünümü][sfx-application-tree]

Ağacın her düzeyinde, ana bölme öğe hakkında ilgili bilgileri gösterir. Örneğin, belirli bir hizmetin sistem durumu durumunu ve sürümünü görebilirsiniz.

![Hizmet Kumaş Explorer temel bölme][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Kümenin düğümlerini görüntüleme
Düğüm görünümü, kümenin fiziksel düzenini gösterir. Belirli bir düğümde, hangi uygulamalara kod dağıtıldığını denetleyebilirsiniz. Daha ayrıntılı olarak, hangi yinelemelerin şu anda orada çalıştığını görebilirsiniz.

## <a name="actions"></a>Eylemler
Service Fabric Explorer, kümenizdeki düğümler, uygulamalar ve hizmetlerle ilgili eylemleri çağırmak için hızlı bir yol sunar.

Örneğin, bir uygulama örneğini silmek için, soldaki ağaçtan uygulamayı seçin ve ardından **Eylemleri** > **Sil Uygulamasını**seçin.

![Service Fabric Explorer'da uygulama silme][sfx-delete-application]

> [!TIP]
> Her öğenin yanındaki elipsleri tıklatarak aynı eylemleri gerçekleştirebilirsiniz.
>
> Service Fabric Explorer aracılığıyla gerçekleştirilebilecek her eylem, otomasyonu sağlamak için PowerShell veya REST API aracılığıyla da gerçekleştirilebilir.
>
>

Belirli bir uygulama türü ve sürümü için uygulama örnekleri oluşturmak için Service Fabric Explorer'ı da kullanabilirsiniz. Ağaç görünümünde uygulama türünü seçin ve ardından sağ bölmede istediğiniz sürümün yanındaki **uygulama örneği** oluştur bağlantısını tıklatın.

![Service Fabric Explorer'da uygulama örneği oluşturma][sfx-create-app-instance]

> [!NOTE]
> Hizmet Kumaş Gezgini, uygulama örnekleri oluştururken parametreleri desteklemez. Uygulama örnekleri varsayılan parametre değerlerini kullanır.
>
>

## <a name="event-store"></a>Etkinlik Mağazası
EventStore, Service Fabric Explorer'da ve REST API'de sunulan Service Fabric platform etkinliklerini sağlayan platform tarafından sunulan bir özelliktir. Kümenizde her bir varlık için düğüm, hizmet, uygulama ve sorgu gibi olayın zamanına bağlı olarak neler olup bittiğinin anlık görüntüsünü görebilirsiniz. Ayrıca EventStore Hakkında Daha Fazla Bilgi Için [EventStore'a Genel Bakış'](service-fabric-diagnostics-eventstore.md)ı da edinebilirsiniz.   

![EventStore][sfx-eventstore]

>[!NOTE]
>Hizmet Kumaş sürüm 6.4 itibariyle. EventStore varsayılan olarak etkinleştirilmez ve kaynak yöneticisi şablonunda etkin olmalıdır

>[!NOTE]
>Hizmet Kumaş sürüm 6.4 itibariyle. EventStore API'leri yalnızca Azure'da çalışan Windows kümeleri için kullanılabilir. Bu işlevselliği Linux'a ve Bağımsız kümelerimize taşımaya çalışıyoruz.

## <a name="image-store-viewer"></a>Resim Mağazası Görüntüleyici
Resim deposu görüntüleyici, Resim Deposu'nun geçerli içeriğini görüntülemeye ve dosya ve klasör bilgilerini almaya ve dosyaları/klasörleri kaldırmayı sağlayan Yerel Resim Deposu kullanıyorsa sunulan bir özelliktir.

![Service Fabric Explorer küme haritası][sfx-imagestore]

## <a name="backup-and-restore"></a>Yedekleme ve Geri Yükleme
Service Fabric Explorer [Yedekleme ve Geri Yükleme](./service-fabric-reliable-services-backup-restore.md)ile arayüz yeteneği sunar. Yedekleme ve Geri Yükleme özelliklerini SFX'te görebilmek için gelişmiş modun etkinleştirilmesi gerekir.

![Gelişmiş modu etkinleştirme][0]
 
Aşağıdaki işlemler mümkündür:

* Yedekleme İlkesi oluşturun, düzenleyin ve silin.
* Bir uygulama, hizmet veya bölüm için Yedekleme'yi etkinleştirin ve devre dışı kılabilir.
* Bir uygulama, hizmet veya bölüm için Yedeklemeyi askıya alın ve devam ettirin.
* Bir bölümün Yedeklemesini tetikle ve izleyin.
* Bir bölüm için Geri Yükleme'yi tetikle ve izleyin.

Yedekleme ve Geri Yükleme hizmeti hakkında daha fazla şey için [REST API başvurusuna](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)bakın.
## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulamalarınızı Visual Studio'da yönetme](service-fabric-manage-application-in-visual-studio.md)
* [PowerShell kullanarak Servis Kumaş uygulaması dağıtımı](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/sfx-cluster-dashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/sfx-cluster-map.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/sfx-application-tree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/sfx-service-essentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/sfx-delete-application.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/sfx-create-app-instance.png
[sfx-eventstore]: ./media/service-fabric-diagnostics-eventstore/eventstore.png
[sfx-imagestore]: ./media/service-fabric-visualizing-your-cluster/sfx-image-store.png
[0]: ./media/service-fabric-backuprestoreservice/advanced-mode.png