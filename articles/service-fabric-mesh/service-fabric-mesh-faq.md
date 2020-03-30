---
title: Azure Hizmet Kumaş Örgü için sık sorulan sorular
description: Azure Service Fabric Mesh için sık sorulan sorular ve yanıtlar hakkında bilgi edinin.
ms.author: pepogors
ms.date: 4/23/2019
ms.topic: troubleshooting
ms.openlocfilehash: 2a5c2ea63d162eb6fb78ab702e0519f8ac25dcc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252504"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Sık sorulan Servis Kumaş Örgü soruları

Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Bu makalede, sık sorulan soruların yanıtları vardır.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Bir sorunu nasıl bildirebilirim veya soru sorarım?

Sorular sorun, Microsoft mühendislerinden yanıtlar alın ve [hizmet-kumaş-örgü-önizleme GitHub repo'sundaki](https://aka.ms/sfmeshissues)sorunları bildirin.

## <a name="quota-and-cost"></a>Kota ve Maliyet

### <a name="what-is-the-cost-of-participating-in-the-preview"></a>Önizlemeye katılmanın maliyeti nedir?

Şu anda Mesh önizlemesine uygulamaları veya kapsayıcıları dağıtmak için herhangi bir ücret yok. Faturalandırma için etkinleştirme için Mayıs ayındaki güncellemeleri izleyin. Ancak, dağıttığınız kaynakları silmenizi ve bunları etkin olarak test etmediğiniz sürece çalışır durumda bırakmamanızı öneririz.

### <a name="is-there-a-quota-limit-of-the-number-of-cores-and-ram"></a>Çekirdek ve RAM sayısının kota sınırı var mı?

Evet. Her abonelik için kotalar şunlardır:

- Başvuru sayısı: 5
- Uygulama başına çekirdek sayısı: 12
- Uygulama başına toplam RAM: 48 GB
- Ağ ve Giriş bitiş noktaları: 5
- Ekebileceğiniz Azure Birimleri: 10
- Hizmet yineleme sayısı: 3
- Dağıtabileceğiniz en büyük konteyner 4 çekirdek ve 16 GB RAM ile sınırlıdır.
- Konteynerlerinize en fazla 6 çekirdek lik 0,5 çekirdeklik artışlarla kısmi çekirdek ayırabilirsiniz.

### <a name="how-long-can-i-leave-my-application-deployed"></a>Başvurumu daha ne kadar süreyle dağıtabilirim?

Şu anda bir uygulamanın kullanım ömrünü iki günle sınırlandırdık. Bu, önizlemeye ayrılan boş çekirdeklerin kullanımını en üst düzeye çıkarmak içindir. Sonuç olarak, belirli bir dağıtımı yalnızca 48 saat boyunca sürekli olarak çalıştırabilirsiniz ve bu süre sonra kapatılacaktır.

Bunun gerçekleştiğini görürseniz, Azure CLI'deki komutu `az mesh app show` çalıştırarak sistemin kapatıldığını doğrulayabilirsiniz. İade edip etmeyişeyecek lerini denetle`"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."` 

Örnek: 

```azurecli
az mesh app show --resource-group myResourceGroup --name helloWorldApp
```

```output
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Kaynak grubunu silmek için `az group delete <nameOfResourceGroup>` komutu kullanın.

## <a name="deployments"></a>Dağıtımlar

### <a name="what-container-images-are-supported"></a>Hangi kapsayıcı görüntüleri desteklenir?

Windows Fall Creators Update (sürüm 1709) makinesinde geliştiriyorsanız, yalnızca Windows sürüm 1709 docker görüntülerini kullanabilirsiniz.

Windows 10 Nisan 2018 güncelleştirmesi (sürüm 1803) makinesinde geliştiriyorsanız, Windows sürüm 1709 veya Windows sürüm 1803 docker görüntülerini kullanabilirsiniz.

Hizmetleri dağıtmak için aşağıdaki kapsayıcı işletim sistemi görüntüleri kullanılabilir:
- Windows - windowsservercore ve nanoserver
    - Windows Server 1709
    - Windows Server 1803
    - Windows Server 1809
    - Windows Server 2019 LTSC
- Linux
    - Bilinen sınırlama yok

> [!NOTE]
> Mesh için Visual Studio araçlama henüz Windows Server 2019 ve 1809 kapsayıcılarına dağıtım ı desteklemez.

### <a name="what-types-of-applications-can-i-deploy"></a>Ne tür uygulamalar dağıtabilirim? 

Bir uygulama kaynağına yerleştirilen kısıtlamalara uyan kapsayıcılarda çalışan her şeyi dağıtabilirsiniz (kotalar hakkında daha fazla bilgi için yukarıya bakın). Mesh'i yasadışı iş yüklerini çalıştırmak veya sistemi kötüye kullanmak için kullandığınızı tespit edersek (örn. madencilik), dağıtımlarınızı sonlandırma ve aboneliğinizi hizmette çalışmasını engelleme hakkını saklı tutuyoruz. Belirli bir iş yükünü çalıştırmakla ilgili sorularınız varsa lütfen bize ulaşın. 

## <a name="developer-experience-issues"></a>Geliştirici deneyimi sorunları

### <a name="dns-resolution-from-a-container-doesnt-work"></a>Bir kapsayıcıdan Gelen DNS çözünürlüğü çalışmıyor

Bir kapsayıcıdan Hizmet Kumaşı DNS hizmetine giden DNS sorguları belirli koşullar altında başarısız olabilir. Bu soruşturuluyor. Azaltmak için:

- Windows Fall Creators güncelleştirme (sürüm 1709) veya temel kapsayıcı görüntü olarak daha yüksek kullanın.
- Hizmet adı tek başına çalışmıyorsa, tam nitelikli adı deneyin: ServiceName.ApplicationName.
- Hizmetinizin Docker dosyasında, `EXPOSE <port>` hizmetinizi açığa çıkardığınız bağlantı noktasının nerede olduğunu ekleyin. Örnek:

```Dockerfile
EXPOSE 80
```

### <a name="dns-does-not-work-the-same-as-it-does-for-service-fabric-development-clusters-and-in-mesh"></a>DNS, Service Fabric geliştirme kümelerinde ve Mesh'te olduğu gibi çalışmaz

Yerel geliştirme kümenizde hizmetlere Azure Mesh'ten farklı şekilde başvurmanız gerekebilir.

Yerel geliştirme kümenizde `{serviceName}.{applicationName}`. Azure Hizmet Kumaş Örgü'de. `{servicename}` 

Azure Mesh şu anda uygulamalar arasında DNS çözünürlüğünü desteklememektedir.

Windows 10'da Bir Hizmet Kumaşı geliştirme kümesini çalıştırmayla ilgili bilinen diğer DNS sorunları için bkz: [Hata ayıklama Windows kapsayıcıları](/azure/service-fabric/service-fabric-how-to-debug-windows-containers) ve [bilinen DNS sorunları.](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#known-issues)

### <a name="networking"></a>Ağ Oluşturma

ServiceFabric ağı NAT, uygulamanızı yerel makinenizde çalıştırırken kaybolabilir. Bunun olup olmadığını tanılamak için, aşağıdakileri bir komut isteminden çalıştırın:

`docker network ls`ve listelenip listelenmediğine `servicefabric_nat` dikkat edin.  Değilse, aşağıdaki komutu çalıştırın:`docker network create -d=nat --subnet 10.128.0.0/24 --gateway 10.128.0.1 servicefabric_nat`

Bu, uygulama zaten yerel olarak ve sağlıksız bir durumda dağıtılmış olsa bile sorunu giderecektir.

### <a name="issues-running-multiple-apps"></a>Birden çok uygulamanın çalıştırıladaki sorunları

Tüm uygulamalarda CPU kullanılabilirliği ve sınırlarının sabitlendiğini görebilirsiniz. Azaltmak için:
- Beş düğüm kümesi oluşturun.
- Dağıtılan uygulama genelindeki hizmetlerdeki CPU kullanımını azaltın. Örneğin, hizmetinizin hizmetinde.yaml dosyasında, `cpu: 1.0``cpu: 0.5`

Tek düğümlü kümeye birden çok uygulama dağıtılamaz. Azaltmak için:
- Yerel bir kümeye birden çok uygulama dağıtırken beş düğüm kümesi kullanın.
- Şu anda test etmediğiniz uygulamaları kaldırın.

### <a name="vs-tooling-has-limited-support-for-windows-containers"></a>VS Tooling, Windows kapsayıcıları için sınırlı desteğe sahiptir

Visual Studio araç lama, windows containers'ın bugün yalnızca temel işletim sistemi sürümüyle dağıtılmasını destekler. 

## <a name="feature-gaps-and-other-known-issues"></a>Özellik boşlukları ve bilinen diğer sorunlar

### <a name="after-deploying-my-application-the-network-resource-associated-with-it-does-not-have-an-ip-address"></a>Uygulamamı dağıttıktan sonra, uygulamayla ilişkili ağ kaynağının IP adresi yok

IP adresinin hemen kullanılmadığı bilinen bir sorun vardır. İlişkili IP adresini görmek için ağ kaynağının durumunu birkaç dakika içinde denetleyin.

### <a name="my-application-fails-to-access-the-right-networkvolume-resource"></a>Uygulamam doğru ağa/birim kaynağa erişemedi

Uygulama modelinizde, ilişkili kaynağa erişebilmek için ağlar ve birimler için tam kaynak kimliğini kullanın. Aşağıda hızlı başlangıç örneğinden bir örnek verilmiştir:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

### <a name="when-i-scale-out-all-of-my-containers-are-affected-including-running-ones"></a>Ölçeklendirdiğimde, çalışan kaplar da dahil olmak üzere tüm kaplarım etkilenir.

Bu bir hatadır ve bir düzeltme uygulanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh hakkında daha fazla bilgi edinmek için [genel bakışı](service-fabric-mesh-overview.md)okuyun.
