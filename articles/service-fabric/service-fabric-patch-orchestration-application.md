---
title: Service Fabric kümenizdeki Windows işletim sistemini yama
description: Bu makalede, Patch Orkestrasyon Uygulaması kullanarak Bir Hizmet Kumaş ı kümesiüzerinde işletim sistemi yama otomatikleştirin nasıl anlatılmaktadır.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: atsenthi
ms.openlocfilehash: 857a4da0b24d600ecc572933af578e2e8faf501a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366323"
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Service Fabric kümenizdeki Windows işletim sistemini yama

> 
> [!IMPORTANT]
> 30 Nisan 2019 itibariyle Patch Orchestration Application sürüm 1.2.* artık desteklenmez. En son sürüme yükselttikinden emin olun.

> [!NOTE]
> [Sanal makine ölçeği setinizde otomatik işletim sistemi görüntü yükseltmeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) almak, işletim sisteminizi Azure'da yamalı tutmak için en iyi yöntemdir. Sanal Makine Ölçeği Set tabanlı otomatik işletim sistemi görüntü yükseltmeleri bir ölçek kümesiüzerinde gümüş veya daha fazla dayanıklılık gerektirir.
>

 Yama Düzenleme Uygulama (POA), Azure'da barındırılan olmayan kümeler için yapılandırma tabanlı işletim sistemi yama zamanlaması sağlayan Azure Hizmet Kumaş Onarım Yöneticisi hizmetinin etrafında bir sarmalayıcıdır. Azure'da barındırılan kümeler için POA gerekmez, ancak hizmet dokusu küme ana bilgisayarlarını kapalı kalma süresine maruz kalmadan yama lamak için güncelleştirme etki alanına göre yama yüklemesi zamanlanması gerekir.

POA, bir Servis Kumaşı kümesinde çalışma süresi nezamalı işletim sistemini otomatikleştiren bir Hizmet Kumaşuygulamasıdır.

POA aşağıdaki özellikleri sağlar:

- **Otomatik işletim sistemi güncelleme kurulumu**. İşletim sistemi güncelleştirmeleri otomatik olarak indirilir ve yüklenir. Küme düğümleri küme kapalı kalma süresine maruz kalmadan gerektiği gibi yeniden başlatılır.

- **Küme farkında yama ve sağlık entegrasyonu.** POA güncelleştirmeleri uygularken, küme düğümlerinin durumunu izler. Küme düğümleri aynı anda bir düğüm veya aynı anda bir güncelleştirme etki alanı güncelleştirilir. Kümenin durumu yama işlemi nedeniyle aşağı giderse, sorunu ağırlaştırmak önlemek için yama durdurulur.

## <a name="internal-details-of-poa"></a>POA'nın iç detayları

POA aşağıdaki alt bileşenlerden oluşur:

- **Koordinatör Hizmet**: Bu devlet hizmeti sorumludur:
    - Tüm kümedeki Windows Update işini koordine etme.
    - Tamamlanan Windows Update işlemlerinin sonucunu depolama.

- **Düğüm Aracı Sıcağı Servisi**: Bu hizmet tüm Service Fabric küme düğümlerinde çalışır. Hizmet aşağıdakilerden sorumludur:
    - Düğüm Agent NTService bootstrapping.
    - Düğüm Agent NTService izleme.

- **Düğüm Agent NTService**: Bu Windows NT hizmeti daha üst düzey bir ayrıcalıkta (SYSTEM) çalışır. Buna karşılık, Düğüm Aracısı Hizmeti ve Koordinatör Hizmeti daha düşük düzeyli bir ayrıcalıkta (NETWORK SERVICE) çalışır. Hizmet, tüm küme düğümlerinde aşağıdaki Windows Update işlerini gerçekleştirmekten sorumludur:
    - Düğümdeki otomatik Windows güncelleştirmelerini devre dışı bırakma.
    - Windows güncelleştirmelerini kullanıcının sağladığı ilkeye göre indirme ve yükleme.
    - Makineyi Windows sonrası güncelleştirmeleri yüklemesini yeniden başlatma.
    - Windows güncelleştirmelerinin sonuçlarını Koordinatör Hizmetine yükleme.
    - Bir işlem tüm yeniden denemeleri tükettikten sonra başarısız olduysa, sağlık raporlarını bildirme.

> [!NOTE]
> POA düğümü devre dışı bırakıp etkinleştirmek ve sistem durumu denetimleri gerçekleştirmek için Servis Kumaş Onarım Yöneticisi hizmetini kullanır. POA tarafından oluşturulan onarım görevi, her düğüm için Windows Update ilerleme sini izler.

## <a name="prerequisites"></a>Ön koşullar

> [!NOTE]
> Gerekli minimum .NET Framework sürümü 4.6'dır.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Onarım Yöneticisi hizmetini etkinleştirin (zaten çalışmıyorsa)

POA, kümeüzerinde Onarım Yöneticisi hizmetinin etkinleştirilmesini gerektirir.

#### <a name="azure-clusters"></a>Azure kümeleri

Gümüş dayanıklılık katmanındaki Azure kümeleri varsayılan olarak Onarım Yöneticisi hizmetini etkinleştirir. Altın dayanıklılık katmanındaki Azure kümeleri, bu kümelerin ne zaman oluşturulduğuna bağlı olarak Onarım Yöneticisi hizmetini etkinleştirmiş olabilir veya etkinleştirmeyebilir. Bronz dayanıklılık katmanındaki Azure kümeleri varsayılan olarak Onarım Yöneticisi hizmetini etkinleştirmez. Hizmet zaten etkinse, Hizmet Kumaş Explorer'da sistem hizmetleri bölümünde çalıştığını görebilirsiniz.

##### <a name="the-azure-portal"></a>Azure portal
Kümeyi ayarlarken Azure portalından Onarım Yöneticisi'ni etkinleştirebilirsiniz. Kümeyi yapılandırırken, Eklenti özellikleri altında **Onarım Yöneticisi Ekle** seçeneğini **belirleyin.**

![Azure portalından Onarım Yöneticisi'ni etkinleştirme görüntüsü](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="the-azure-resource-manager-deployment-model"></a>Azure Kaynak Yöneticisi dağıtım modeli
Alternatif olarak, yeni ve varolan Hizmet Dokusu kümelerinde Onarım Yöneticisi hizmetini etkinleştirmek için [Azure Kaynak Yöneticisi dağıtım modelini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) kullanabilirsiniz. Dağıtmak istediğiniz küme için şablonu alın. Örnek şablonları kullanabilir veya özel bir Azure Kaynak Yöneticisi dağıtım modeli şablonu oluşturabilirsiniz. 

[Azure Kaynak Yöneticisi dağıtım modeli şablonu](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm)kullanarak Onarım Yöneticisi hizmetini etkinleştirmek için aşağıdakileri yapın:

1. `apiVersion` *Microsoft.ServiceFabric/clusters* kaynağı için *2017-07-01 önizlemeolarak* ayarlandığından emin olun. Farklıysa, `apiVersion` *2017-07-01 önizlemeveya* daha sonrasına güncelleştirmeniz gerekir:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

1. Bölümden sonra aşağıdaki `addonFeatures` bölümü ekleyerek Onarım `fabricSettings` Yöneticisi hizmetini etkinleştirin:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Küme şablonunuzu bu değişikliklerle güncelledikten sonra uygulayın ve güncelleştirmenin bitmesine izin verin. Artık kümenizde çalışan Onarım Yöneticisi hizmetini görebilirsiniz. Service Fabric Explorer'daki sistem hizmetleri bölümünde *kumaş:/System/RepairManagerService* olarak adlandırılır. 

### <a name="standalone-on-premises-clusters"></a>Bağımsız şirket içi kümeler

Yeni veya varolan Service Fabric kümesinde Onarım Yöneticisi hizmetini etkinleştirmek [için, bağımsız Windows kümesi için Yapılandırma ayarlarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest)kullanabilirsiniz.

Onarım Yöneticisi hizmetini etkinleştirmek için:

1. Genel küme `apiVersion` [yapılandırmalarında](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) burada gösterildiği gibi *04-2017* veya daha sonra olarak ayarlandığından emin olun:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

1. Aşağıda gösterildiği gibi, `addonFeatures` `fabricSettings` bölümden sonra aşağıdaki bölümü ekleyerek Onarım Yöneticisi hizmetini etkinleştirin:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

1. Güncelleştirilmiş küme bildirimini kullanarak küme bildiriminizi bu değişikliklerle güncelleştirin [yeni bir küme oluşturun](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) veya küme [yapılandırmasını yükseltin.](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server) 

   Küme güncelleştirilmiş bir küme bildirimiyle çalışmaya devam ettikten sonra, kümenizde çalışan Onarım Yöneticisi hizmetini görebilirsiniz. Buna kumaş *adı verilir:/System/RepairManagerService*, ve Service Fabric Explorer'daki sistem hizmetleri bölümünde.

### <a name="configure-windows-updates-for-all-nodes"></a>Tüm düğümler için Windows güncelleştirmelerini yapılandırma

Birden çok küme düğümü aynı anda yeniden başlatılabildiği için otomatik Windows güncelleştirmeleri kullanılabilirlik kaybına neden olabilir. POA, varsayılan olarak, her küme düğümündeki otomatik Windows güncelleştirmelerini devre dışı düşürmeye çalışır. Ancak, ayarlar bir yönetici veya Grup İlkesi tarafından yönetiliyorsa, Windows Update ilkesini açıkça "İndirmeden Önce Bildir" olarak ayarlamanızı öneririz.

## <a name="download-the-application-package"></a>Uygulama paketini indirin

Uygulama paketini indirmek için GitHub'daki [Patch Orchestration Application sürüm sayfasına](https://github.com/microsoft/Service-Fabric-POA/releases/latest/) gidin.

## <a name="configure-poa-behavior"></a>POA davranışını yapılandırma

POA davranışını gereksinimlerinizi karşılayacak şekilde yapılandırabilirsiniz. Uygulamayı oluştururken veya güncellerken uygulama parametresini geçirerek varsayılan değerleri geçersiz kılın. Uygulama parametrelerini `ApplicationParameter` cmdlets'e `New-ServiceFabricApplication` `Start-ServiceFabricApplicationUpgrade` belirterek sağlayabilirsiniz.

| Parametre        | Tür                          | Ayrıntılar |
|:-|-|-|
|MaxResultsToÖnbellek    |Uzun                              | Önbelleğe alınması gereken en fazla Windows Update sonucu sayısı. <br><br>Varsayılan değer 3000'dir, aşağıdakileri varsayarsak: <br> &nbsp;&nbsp;- Düğüm sayısı 20'dir. <br> &nbsp;&nbsp;- Bir düğüme aylık güncelleştirme sayısı 5'tir. <br> &nbsp;&nbsp;- Operasyon başına sonuç sayısı 10 olabilir. <br> &nbsp;&nbsp;- Son üç ayın sonuçları saklanmalıdır. |
|Görev Onayı Politikası   |Sabit Listesi <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy, Windows güncelleştirmelerini Hizmet Dokusu küme düğümlerine yüklemek için Koordinatör Hizmeti tarafından kullanılacak ilkeyi gösterir.<br><br>İzin verilen değerler şunlardır: <br>*NodeWise*: Windows güncelleştirmeleri bir seferde bir düğüm yüklenir. <br> *UpgradeDomainWise*: Windows güncelleştirmeleri bir seferde bir güncelleştirme etki alanı yüklenir. (En fazla, bir güncelleştirme etki alanına ait tüm düğümler Windows güncelleştirmesi için gidebilir.)<br><br> Kümeniz için hangi iidi nin en uygun olduğuna karar vermenize yardımcı olmak için [SSS](#frequently-asked-questions) bölümüne bakın.
|GünlüklerDiskQuotaInMB   |Uzun  <br> (Varsayılan: *1024*)               | Yama orkestrasyonu uygulamasının maksimum boyutu MB'de günlüklenir ve düğümlerde yerel olarak kalıcı olarak kalıcı olabilir.
| WUQuery               | string<br>(Varsayılan: *IsInstalled=0*)                | Windows güncelleştirmelerini almak için sorgula. Daha fazla bilgi için [WuQuery'ye bakın.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| WindowsOSOnlyUpdates'i Yükleme | *Boole* <br> (varsayılan: yanlış)                 | Hangi güncelleştirmenin indirilip yüklenmesi gerektiğini denetlemek için bu bayrağı kullanın. Aşağıdaki değerlere izin verilir <br>true - Yalnızca Windows işletim sistemi güncelleştirmelerini yükler.<br>false - Mevcut tüm güncellemeleri makineye yükler.          |
| WUOperationTimeOutInMinutes | int <br>(Varsayılan: *90*)                   | Herhangi bir Windows Update işlemi (arama veya indirme veya yükleme) için zaman arasını belirtir. İşlem belirtilen zaman anına kadar tamamlanmazsa, iptal edilir.       |
| WURescheduleSayısı     | int <br> (Varsayılan: *5*)                  | Bir işlem kalıcı olarak başarısız olursa, hizmetin Windows güncelleştirmesini en fazla zamanlaması sayısı.          |
| WURescheduleTimeInMinutes | int <br>(Varsayılan: *30*) | Hata devam ederse, hizmetin Windows güncelleştirmelerini yeniden zamanladığı aralık. |
| WUFrekans           | Virgülle ayrılmış dize (Varsayılan: *Haftalık, Çarşamba, 07:00:00*)     | Windows güncelleştirmelerini yükleme sıklığı. Biçimi ve olası değerleri şunlardır: <br>&nbsp;&nbsp;- Aylık: DD, HH:MM:SS (örneğin, *Aylık, 5,12:22:32*)<br>DD (gün) alanı için izin verilen değerler 1 ile 28 ve "son" sayılardır. <br> &nbsp;&nbsp;- Haftalık, GÜN, HH:MM:SS (örneğin, *Haftalık, Salı, 12:22:32*)  <br> &nbsp;&nbsp;- Günlük, HH:MM:SS (örneğin, *Günlük, 12:22:32*)  <br> &nbsp;&nbsp;-  *Hiçbiri* Windows güncelleştirmeleri yapılmaması gerektiğini gösterir.  <br><br> Zaman UTC bulunmaktadır.|
| KabulWindowsUpdateEula | Boole <br>(Varsayılan: *true*) | Bu bayrağı ayarlayarak, uygulama makine sahibi adına Windows Update için Son Kullanıcı Lisans Sözleşmesi'ni kabul eder.              |

> [!TIP]
> Windows güncelleştirmelerin hemen gerçekleşmesini `WUFrequency` istiyorsanız, uygulama dağıtım süresine göre ayarlayın. Örneğin, beş düğümlü bir test kümeniz olduğunu ve uygulamayı 17:00 civarında UTC'de dağıtmayı planladığınızı varsayalım. Uygulama yükseltme veya dağıtımın en fazla 30 dakika sürdüğünü varsayarsanız, WUFrequency'yi *Günlük saat 17:30:00*olarak ayarlayın.

## <a name="deploy-poa"></a>POA'yı dağıt

1. Kümeyi hazırlamak için tüm ön koşul adımlarını tamamlayın.
1. PoA'yı diğer Service Fabric uygulaması gibi dağıtın. PowerShell'i kullanarak dağıtmak için [PowerShell kullanarak dağıt'a](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications)bakın ve uygulamaları kaldırın.
1. Dağıtımı sırasında uygulamayı yapılandırmak için cmdlet'e `ApplicationParameter` `New-ServiceFabricApplication` geçirin. Sizin rahatınız için, uygulama ile birlikte komut deploy.ps1 sağladık. Komut dosyasını kullanmak için:

    - Kullanarak Service Fabric kümesine `Connect-ServiceFabricCluster`bağlanın.
    - PowerShell komut dosyası Deploy.ps1'i uygun `ApplicationParameter` değerle çalıştırın.

> [!NOTE]
> Komut dosyasını ve uygulama klasörünü *PatchOrchestrationApplication'ı* aynı dizinde tutun.

## <a name="upgrade-poa"></a>POA'yı yükselt

PowerShell'i kullanarak POA sürümünüzü yükseltmek için [PowerShell'i kullanarak Service Fabric uygulama](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell)yükseltmesindeki talimatları izleyin.

## <a name="remove-poa"></a>POA'yı kaldırın

Uygulamayı kaldırmak için, Dağıt'taki yönergeleri izleyin [ve PowerShell'i kullanarak uygulamaları kaldırın.](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications)

Rahatınız için, uygulamayla birlikte Undeploy.ps1 komut dosyasını sağladık. Komut dosyasını kullanmak için:

  - Kullanarak Service Fabric kümesine ```Connect-ServiceFabricCluster```bağlanın.
  - PowerShell komut dosyası Undeploy.ps1 çalıştırın.

> [!NOTE]
> Komut dosyasını ve uygulama klasörünü *PatchOrchestrationApplication'ı* aynı dizinde tutun.

## <a name="view-the-windows-update-results"></a>Windows Update sonuçlarını görüntüleme

POA, geçmiş sonuçları kullanıcılara görüntülemek için REST API'lerini ortaya çıkarır. Burada sonuç JSON bir örnek:

```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2019-05-13T08:44:56.4836889Z",
        "OperationStartTime": "2019-05-13T08:44:33.5285601Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "ResultCode": 0,
            "HResult": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```

JSON alanları aşağıdaki tabloda açıklanmıştır:

Alan | Değerler | Ayrıntılar
-- | -- | --
İşlem Sonucu | 0 - Başarılı<br> 1 - Hatalarla Başarılı<br> 2 - Başarısız<br> 3 - İptal edildi<br> 4 - Zaman Dilimi ile İptal | Normalde bir veya daha fazla güncelleştirmenin yüklenmesini içeren genel işlemin sonucunu gösterir.
Resultcode | İşlem Sonucu ile Aynı | Bu alan, tek bir güncelleştirme için yükleme işleminin sonucunu gösterir.
Operationtype | 1 - Kurulum<br> 0 - Arama ve İndirme| Varsayılan olarak, Yükleme sonuçlarda gösterilen tek İşlem Türüdür.
WindowsUpdateQuery | Varsayılan "IsInstalled=0" | Güncelleştirmeleri aramak için kullanılan Windows Update sorgusu. Daha fazla bilgi için [WuQuery'ye](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)bakın.
Yeniden BaşlatmaGerekli | true - yeniden başlatma gerekli ydi<br> false - yeniden başlatma gerekli değildi | Güncelleştirmelerin yüklenmesini tamamlamak için yeniden başlatma gerekip gerekmediğini gösterir.
İşlemBaşlangıç Zamanı | DateTime | İşlemin (İndirme/Yükleme) başlatıldıği zamanı gösterir.
Çalışma Süresi | DateTime | İşlemin (İndirme/Yükleme) tamamlandığı zamanı gösterir.
Hresult | 0 - Başarılı<br> diğer - başarısızlık| UpdateID "7392acaf-6a85-427c-8a8d-058c25beb0d6" ile Windows güncelleştirmesinin başarısızlığının nedenini gösterir.

Henüz zamanlanmamışsa, sonuç JSON boştur.

Windows Update sonuçlarını sorgulamak için kümede oturum açın. Koordinatör Hizmetin birincil adresinin çoğaltma IP adresini öğrenin ve tarayıcıdan aşağıdaki&lt;URL'yi&gt;&lt;açın:&gt;http:// REPLICA-IP : ApplicationPort /PatchOrchestrationApplication/v1/GetWindowsUpdateResults.

Koordinatör Hizmet için REST bitiş noktası dinamik bir bağlantı noktasına sahiptir. Tam URL'yi kontrol etmek için Service Fabric Explorer'a bakın. Örneğin, sonuçlar *http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults*.

![REST bitiş noktasının görüntüsü](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

Kümede ters proxy etkinleştirilmişse, URL'ye kümenin dışından da erişebilirsiniz.

Vurmak için gereken bitiş noktası *&lt;http://&gt;SERVERURL:&lt;&gt;REVERSEPROXYPORT / PatchOrchestrationApplication / CoordinatorService/v1/GetWindowsUpdateResults*.

Kümedeki ters proxy'yi etkinleştirmek için [Azure Hizmet Kumaşı'ndaki Ters proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy)yönergelerini izleyin. 

> 
> [!WARNING]
> Ters proxy yapılandırıldıktan sonra, bir HTTP bitiş noktasını ortaya çıkaran kümedeki tüm mikro hizmetler kümenin dışından giderilebilir.

## <a name="diagnostics-and-health-events"></a>Teşhis ve sağlık olayları

Bu bölümde, Hizmet Kumaş kümelerinde POA aracılığıyla yama güncelleştirmeleriyle ilgili sorunları nasıl ayıklama veya tanılama konuları açıklanmıştır.

> [!NOTE]
> Aşağıdaki çağrı, kendi kendine tanılama iyileştirmeleri çok almak için, POA sürüm 1.4.0 veya daha sonra yüklü olmalıdır.

Düğüm Aracısı NTService düğümleri güncellemeleri yüklemek için [onarım görevleri](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtask?view=azure-dotnet) oluşturur. Her görev daha sonra Koordinatör Servis tarafından görev onay ilkesine göre hazırlanır. Son olarak, hazırlanan görevler Onarım Yöneticisi tarafından onaylanır ve küme sağlıksız durumdaysa herhangi bir görevi onaylamaz. 

Güncelleştirmelerin bir düğümüzerinde nasıl devam ettiğinizi anlamanıza yardımcı olmak için adım adım gidelim:

1. Her düğümde çalışan NodeAgentNTService, zamanlanan zamanda kullanılabilir Windows güncelleştirmelerini arar. Güncelleştirmeler varsa, bunları düğüme indirir.

1. Güncelleştirmeler indirildikten sonra, Düğüm Aracısı NTService>unique_id adı *POS___\< *düğüm için karşılık gelen bir onarım görevi oluşturur. Bu onarım görevlerini [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps) cmdlet'i kullanarak veya düğüm ayrıntıları bölümünde SFX kullanarak görüntüleyebilirsiniz. Onarım görevi oluşturulduktan sonra, hızla [ *Claimed* durumuna](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtaskstate?view=azure-dotnet)taşınır.

1. Koordinatör Hizmet, *Talep edilen* durumdaki onarım görevlerini düzenli olarak arar ve bunları TaskApprovalPolicy'ye dayalı olarak durumu *hazırlama* olarak güncelleştirir. TaskApprovalPolicy NodeWise olarak yapılandırılırsa, düğüme karşılık gelen bir onarım görevi yalnızca şu anda Başka bir onarım görevi *Hazırlanma,* *Onaylanan,* *Yürütme*veya *Geri Verme* durumunda yoksa hazırlanır. 

   Benzer şekilde, UpgradeWise TaskApprovalPolicy durumunda, önceki eyaletlerde yalnızca aynı güncelleştirme etki alanına ait düğümler için görevler vardır. Bir onarım görevi *Hazırlama* durumuna taşındıktan sonra, ilgili Hizmet Kumaş ı yeniden *başlatma*amacıyla [devre dışı](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) bırakılır.

   POA sürümleri 1.4.0 ve daha sonra yamalı düğümleri görüntülemek için CoordinatorService'te ClusterPatchingStatus özelliği ile olayları gönderin. Güncelleştirmeler, aşağıdaki resimde gösterildiği gibi _poanode_0 yüklenir:

    [![Küme düzeltme durumu görüntüsü](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png)](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png#lightbox)

1. Düğüm devre dışı bırakıldıktan sonra, onarım görevi *Yürütme* durumuna taşınır. 
   
   > [!NOTE]
   > *Devre Dışı bırakılmış* durumda sıkışmış bir düğüm kümedeki yama işlemini durduran yeni bir onarım görevini engelleyebilir.

1. Onarım görevi *Yürütme* durumundayken, bu düğümdeki yama yüklemesi başlar. Yama yüklendikten sonra, düğüm yama bağlı olarak yeniden başlatılabilir veya başlatılamayabilir. Daha sonra, onarım görevi düğümü yeniden etkinleştiren *durumu geri getirmek* için taşınır. Onarım görevi daha sonra tamamlanmış olarak işaretlenir.

   POA sürümleri 1.4.0 ve sonraki sürümlerinde, WUOperationStatus-NodeName\<> özelliği ile NodeAgentService'deki sistem durumu olaylarını görüntüleyerek güncelleştirmenin durumunu bulabilirsiniz. Aşağıdaki resimlerde vurgulanan bölümler, düğüm *poanode_0* ve *poanode_2*Windows güncelleştirmelerinin durumunu gösterir:

   [![Windows Update çalışma durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png#lightbox)

   [![Windows Update çalışma durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png#lightbox)

   PowerShell'i kullanarak da detayları alabilirsiniz. Bunu yapmak için kümeye bağlanır ve [Get-ServiceFabricRepairTask'ı](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps)kullanarak onarım görevinin durumunu alırsınız. 
   
   Aşağıdaki örnekte, "POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15" görevi *DownloadComplete* durumundadır. Bu, güncelleştirmelerin *poanode_2* düğümünde karşıdan yüklenmiş olduğu ve görev *Yürütme* durumuna geçtiğinde yüklemenin deneneceği anlamına gelir.

   ``` powershell
    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k = Get-ServiceFabricRepairTask -TaskId "POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15"

    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k.ExecutorData
    {"ExecutorSubState":2,"ExecutorTimeoutInMinutes":90,"RestartRequestedTime":"0001-01-01T00:00:00"}
    ```

   Daha fazla sorun varsa, sanal makinenizde (VM) veya VM'lerde oturum açın ve Windows olay günlüklerini kullanarak bunlar hakkında bilgi edinin. Daha önce bahsedilen onarım görevi yalnızca aşağıdaki uygulayıcı alt devletlerde bulunabilir:

      ExecutorSubState | Açıklama
    -- | -- 
      Yok=1 |  Düğüm üzerinde devam eden bir işlem olmadığını gösterir. Eyalet geçiş döneminde olabilir.
      İndirme Tamamlandı=2 | İndirme işleminin başarı, kısmi hata veya başarısızlıkla tamamlandığını gösterir.
      Kurulum Onaylandı=3 | İndirme işleminin daha önce tamamlandığını ve Onarım Yöneticisi'nin yüklemeyi onayladığını ima eder.
      Kurulum Devam=4 | Onarım görevinin yürütme durumuna karşılık gelir.
      KurulumTamamlandı=5 | Yüklemenin başarı, kısmi başarı veya başarısızlıkla tamamlandığını gösterir.
      Yeniden Başlatmaİsten=6 | Yama yüklemesinin tamamlandığını ve düğümüzerinde bekleyen bir yeniden başlatma eylemi olduğunu ima eder.
      Yeniden BaşlatNotGerekli=7 |  Yama yüklemesi tamamlandıktan sonra yeniden başlatmaya gerek olmadığını ima eder.
      Yeniden BaşlatmaTamamlandı=8 | Yeniden başlatmanın başarıyla tamamlandığını gösterir.
      İşlem Tamamlandı=9 | Windows Update işlemi başarıyla tamamlandı.
      İşlemAborted=10 | Windows Update işleminin iptal edildiğini ima eder.

1. POA sürümleri1.4.0 ve sonraki sürümlerde, bir düğüm güncelleştirme denemesi sona erdiğinde, Windows güncelleştirmelerini indirme ve yükleme girişiminin ne zaman başlayacağını size bildirmek için NodeAgentService'de "WUOperationStatus-[NodeName]" özelliğine sahip bir olay yayınlanır. Bu, aşağıdaki resimde görüntülenir:

     [![Windows Update çalışma durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png#lightbox)

### <a name="diagnostics-logs"></a>Tanılama günlükleri

Yama orkestrasyon uygulama günlükleri Servis Kumaş çalışma zamanı günlükleri bir parçası olarak toplanır.

Seçtiğiniz tanılama aracını veya ardışık hattını kullanarak günlükleri yakalayabilirsiniz. POA [olay kaynağı](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netframework-4.5.1)üzerinden olayları günlüğe kaydetmek için aşağıdaki sabit sağlayıcı adlarını kullanır:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Sağlık raporları

POA ayrıca düğüm aracısı hizmetine veya Koordinatör Hizmete karşı aşağıdaki senaryolarda sağlık raporları yayınlar:

* Düğüm Agent NTService düştü

   Düğüm Aracısı NTService bir düğüm üzerinde yse, Düğüm Aracısı Hizmeti'ne karşı bir uyarı düzeyi sağlık raporu oluşturulur.

* Onarım Yöneticisi hizmeti etkinleştirildi

   Onarım Yöneticisi hizmeti kümede bulunmazsa, Koordinatör Servis için bir uyarı düzeyi sağlık raporu oluşturulur.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: POA çalışırken kümemi neden bir hata durumunda görüyorum?**

C: Yükleme işlemi sırasında POA, sağlıksız bir kümeyle geçici olarak sonuçlanabilecek düğümleri devre dışı alır veya yeniden başlatır.

Uygulamanın ilkesine bağlı olarak, bir düğüm bir yama işlemi sırasında aşağı gidebilir *veya* tüm güncelleştirme etki alanı aynı anda aşağı gidebilir.

Windows güncelleştirmeleri yüklemesinin sonunda, düğümler yeniden başlatılmaya sonra yeniden etkinleştirilir.

Aşağıdaki örnekte, iki düğüm aşağı ve MaxPercentageUnhealthyNodes ilkesi ihlal edildi, çünkü küme geçici olarak bir hata durumuna gitti. Düzeltme işlemi başlayana kadar hata geçicidir.

![Sağlıksız küme görüntüsü](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sorun devam ederse, Sorun Giderme bölümüne bakın.

**S: POA uyarı durumundaysa ne yapabilirim?**

C: Uygulamaya karşı gönderilen bir sağlık raporunun temel nedeni belirtip belirtmediğini denetleyin. Genellikle, uyarı sorunun ayrıntılarını içerir. Sorun geçiciyse, uygulamanın otomatik olarak kurtarılması beklenir.

**S: Kümem sağlıksızsa ve acil bir işletim sistemi güncelleştirmesi yapmam gerekiyorsa ne yapabilirim?**

C: Küme sağlıksız ken POA güncelleştirmeleri yüklemez. POA iş akışının engelini kaldırmak için kümenizi sağlıklı bir duruma getirmeye çalışın.

**S: TaskApprovalPolicy'yi kümem için "NodeWise" veya "UpgradeDomainWise" olarak mı ayarlamalıyım?**

C: "UpgradeDomainWise" ayarı, bir güncelleştirme etki alanına ait tüm düğümleri paralel olarak yamalayarak genel küme onarımını hızlandırAr. İşlem sırasında, tüm güncelleştirme etki alanına ait düğümler kullanılamaz [ *(Devre Dışı bırakılmış* durumda).](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled)

Buna karşılık, "NodeWise" ayarı, genel küme düzeltmenin daha uzun sürebileceğini ima eden bir seferde yalnızca bir düğüm ayarlar. Ancak, yama işlemi sırasında en fazla bir düğüm kullanılamaz *(Devre Dışı durumda).*

Kümeniz yama döngüsü sırasında N-1 sayıda güncelleştirme etki alanında çalışmaya tahammül edemiyorsa (Kümenizdeki toplam güncelleştirme etki alanı sayısı N'dir) ilkeyi "UpgradeDomainWise" olarak ayarlayabilirsiniz. Aksi takdirde, "NodeWise" olarak ayarlayın.

**S: Bir düğümü niçin yamalamak ne kadar sürer?**

C: Düğümü yama maksama dakika (örneğin, [Windows Defender tanım güncelleştirmeleri)](https://www.microsoft.com/en-us/wdsi/definitions)saate (örneğin, [Windows Kümülatif güncelleştirmeleri)](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%20server%20cumulative%20update)alabilir. Bir düğümü yamalamak için gereken süre çoğunlukla bağlıdır: 
 - Güncelleştirmelerin boyutu.
 - Yama penceresinde uygulanması gereken güncelleştirme sayısı.
 - Güncelleştirmeleri yüklemek, düğümü yeniden başlatmak (gerekirse) ve yeniden başlatma sonrası yükleme adımlarını tamamlamak için gereken süre.
 - VM veya makinenin performansı ve ağ koşulları.

**S: Tüm bir kümenin yama için ne kadar sürer?**

C: Tüm kümeyi yamalamak için gereken süre:

- Bir düğümü yamalamak için gereken zaman.

- Koordinatör Hizmet politikası. Varsayılan ilke olan "NodeWise", aynı anda yalnızca bir düğümün yamalanmasıyla sonuçlanır ve bu yaklaşım "UpgradeDomainWise" kullanarak daha yavaştır. 

   Örneğin: Bir düğümün yamalanması ~1 saat sürüyorsa, her biri 4 düğüm içeren 5 güncelleştirme etki alanı içeren 20 düğümlü (aynı tür de düğüm) kümeyi yamalamak gerekir:
    - "NodeWise" için: ~20 saat.
    - "UpgradeDomainWise" için: ~5 saat.

- Küme yükü. Her yama işlemi, müşteri iş yükünü kümedeki diğer kullanılabilir düğümlere değiştirmeyi gerektirir. Yamalı olan bir düğüm bu süre zarfında [ *devre dışı bırakma* durumunda](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabling) olacaktır. Küme en yüksek yüke yakın çalışıyorsa, devre dışı bırakma işlemi daha uzun sürer. Bu nedenle, genel yama işlemi gibi stresli koşullarda yavaş görünebilir.

- Yama sırasında küme sağlık hataları. [Kümenin sağlığındaki](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction) herhangi bir [bozulma](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet#System_Fabric_Health_HealthState_Error) yama işlemini kesintiye uğratabilir. Bu sorun, tüm kümeyi yamalamak için gereken genel süreye eklenir.

**S: Neden WINDOWS Update sonuçlarında REST API üzerinden elde edilen ancak makinedeki Windows Update geçmişi altında olmayan bazı güncelleştirmeler görüyorum?**

C: Bazı ürün güncelleştirmeleri yalnızca kendi güncelleştirme veya yama geçmişinde görünür. Örneğin, Windows Defender güncelleştirmeleri Windows Server 2016'da Windows Update geçmişinde görüntülenebilir veya görüntülenmeyebilir.

**S: POA benim dev küme (tek düğüm küme) yama için kullanılabilir mi?**

C: Hayır, POA tek düğümlü kümeyi yamalamak için kullanılamaz. [Service Fabric sistem hizmetleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) veya diğer müşteri uygulamaları kesintiye maruz kalacağınız için bu sınırlama tasarım gereğidir. Bu nedenle, onarım işleri yama Onarım Yöneticisi tarafından onaylanan asla.

**S: Linux'ta küme düğümlerini nasıl yamam?**

C: Linux'ta güncelleştirmeleri düzenleme hakkında bilgi edinmek için Azure sanal makine ölçeğine göre [otomatik işletim sistemi görüntü yükseltmeleri ayarlayın.](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade)

**S: Güncelleştirme döngüsü neden bu kadar uzun sürüyor?**

C: Sonuç JSON için sorgulayın, tüm düğümler için güncelleştirme döngüsünü girin ve sonra OperationStartTime ve OperationTime (OperationCompletionTime) kullanarak her düğümde güncelleştirme yüklemetarafından alınan zamanı bulmak için deneyebilirsiniz. 

Güncelleştirme nin gerçekleşmediği büyük bir zaman penceresi varsa, küme bir hata durumunda olabilir ve sonuç olarak Onarım Yöneticisi herhangi bir POA onarım görevini onaylayamaz. Güncelleştirme yüklemesi herhangi bir düğüm üzerinde uzun zaman alıyorsa, bu düğüm bir süre içinde güncelleştirilemeyebilir. Birçok güncelleştirme yükleme yi bekliyor olabilir ve bu da gecikmelere neden olabilir. 

Düğüm yamalarının engellenmiş olması da mümkündür, çünkü devre *dışı bırakma* durumunda sıkışıp kalmış. Bu genellikle düğüm devre dışı çoğunluk veya veri kaybı durumlarına yol açabilir çünkü olur.

**S: POA yama yaparken düğüm neden devre dışı bırakılmalı?**

C: POA düğümü *yeniden başlatma* amacıyla devre dışı bırakır ve düğümüzerinde çalışan tüm Service Fabric hizmetlerini durdurur veya yeniden tahsis eder. POA, uygulamaların yeni ve eski DL'lerin bir karışımını kullanmamasını sağlamak için bunu yapar, bu nedenle düğümü devre dışı bırakmadan düzeltmemenizi öneririz.

**S: POA kullanılarak güncelleştirilebilen maksimum düğüm sayısı nedir?**

C: POA, güncelleştirmeler için düğümler için onarım görevleri oluşturmak için Service Fabric Repair Manager kullanır. Ancak, aynı anda en fazla 250 onarım görevi bulunabilir. Şu anda, POA her düğüm için aynı anda onarım görevleri oluşturur, böylece POA kümedeki en fazla 250 düğümü güncelleştirebilir. 

## <a name="disclaimers"></a>Bildirimler

- POA, kullanıcı adına Windows Update için Son Kullanıcı Lisans Sözleşmesini kabul eder. İsteğe bağlı olarak, uygulamayapılandırmasında ayar kapatılabilir.

- POA kullanımı ve performansı izlemek için telemetri toplar. Uygulamanın telemetrisi, Hizmet Kumaşı runtime'ın telemetri ayarını (varsayılan olarak açıktır) izler.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, düğümleri yama sorunları için olası sorun giderme çözümleri sağlar.

### <a name="a-node-is-not-coming-back-to-up-state"></a>Bir düğüm yukarı durumuna geri gelmiyor

* Düğüm *devre dışı bırakma* durumunda sıkışmış olabilir, çünkü:

  - Güvenlik kontrolü beklemede. Bu durumu gidermek için, yeterli düğümleri sağlıklı bir durumda kullanılabilir olduğundan emin olun.

* Düğüm *Devre Dışı bırakıldığında* sıkışmış olabilir, çünkü:

  - Elle devre dışı bırakılmış.
  - Devam eden bir Azure altyapı işi nedeniyle devre dışı bırakıldı.
  - Düğümü yamalamak için POA tarafından geçici olarak devre dışı bırakıldı.

* Düğüm aşağı durumda sıkışmış olabilir, çünkü:

  - El ile aşağı bir duruma yerleştirildi.
  - Bir yeniden başlatma (POA tarafından tetiklenebilir) geçiyor.
  - Hatalı bir VM veya makine var veya ağ bağlantısı sorunları yaşıyor.

### <a name="updates-were-skipped-on-some-nodes"></a>Bazı düğümlerde güncelleştirmeler atlandı

POA, yeniden zamanlama ilkesine göre bir Windows güncelleştirmesi yüklemeye çalışır. Hizmet düğümü kurtarmak ve uygulama ilkesine göre güncelleştirmeyi atlamak çalışır.

Böyle bir durumda, Düğüm Aracısı Hizmeti'ne karşı bir uyarı düzeyi sağlık raporu oluşturulur. Windows Update sonucu da hata için olası nedeni içerir.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-is-being-installed"></a>Güncelleştirme yüklenirken kümenin durumu hataya gider

Hatalı bir Windows güncelleştirmesi, belirli bir düğümveya güncelleştirme etki alanında bir uygulamanın veya kümenin durumunu aşağı getirebilir. POA, küme yeniden sağlıklı olana kadar sonraki Windows Update işlemini durdurdu.

Bir yönetici müdahale etmeli ve Windows Update nedeniyle uygulamanın veya kümenin neden sağlıksız hale geldiğine karar vermelidir.

## <a name="poa-release-notes"></a>POA sürüm notları

>[!NOTE]
> POA sürümleri 1.4.0 ve sonrası için, GitHub'daki [Patch Orchestration Application sürüm sayfasında](https://github.com/microsoft/Service-Fabric-POA/releases/) sürüm notları ve sürümler bulabilirsiniz.

### <a name="version-110"></a>Sürüm 1.1.0
- Genel sürüm

### <a name="version-111"></a>Sürüm 1.1.1
- NodeAgentNTService'in yüklenmesini engelleyen NodeAgentService'in SetupEntryPoint'inde bir hata düzeltildi.

### <a name="version-120"></a>Sürüm 1.2.0

- Hata sistem yeniden iş akışı etrafında giderir.
- Onarım görevlerini hazırlarken sağlık denetimi nin beklendiği gibi gerçekleşmediği Nedeniyle RM görevlerinin oluşturulmasında hata düzeltmesi.
- Windows hizmeti POANodeSvc için başlangıç modunu otomatikten gecikmeli otomatike değiştirdi.

### <a name="version-121"></a>Sürüm 1.2.1

- Küme ölçeğiaşağı iş akışında hata düzeltmesi. Var olmayan düğümlere ait POA onarım görevleri için çöp toplama mantığı tanıtıldı.

### <a name="version-122"></a>Sürüm 1.2.2

- Çeşitli hata düzeltmeleri.
- İkili ler imzalandı.
- Uygulama için sfpkg bağlantısı eklendi.

### <a name="version-130"></a>Sürüm 1.3.0

- InstallWindowsOSOnlyUpdates'i false olarak ayarlamak artık tüm kullanılabilir güncelleştirmeleri yükler.
- Otomatik güncelleştirmeleri devre dışı bırakma mantığını değiştirdi. Bu, Otomatik güncelleştirmelerin Server 2016 ve üzeri devre dışı bırakılmadığı bir hatayı giderir.
- Gelişmiş kullanım durumları için POA'nın her iki mikro hizmeti için parametreli yerleşim kısıtlaması.

### <a name="version-131"></a>Sürüm 1.3.1
- POA 1.3.0'ın otomatik güncelleştirmeleri devre dışı bırakmadaki bir hata nedeniyle Windows Server 2012 R2'de veya daha önce çalışmadığı regresyon sabitleme. 
- InstallWindowsOSOnlyUpdates yapılandırması her zaman True olarak seçilir hata sabitleme.
- InstallWindowsOSOnlyUpdates'in varsayılan değerini False olarak değiştirme.

### <a name="version-132"></a>Sürüm 1.3.2
- Geçerli düğüm adının bir alt kümesi olan bir adla düğüm varsa, düğüm üzerindeki yama yaşam döngüsünü etkileyen bir sorunu giderme. Bu tür düğümler için, yama nın kaçırılmış olması veya yeniden başlatmabeklemede olması mümkündür.
