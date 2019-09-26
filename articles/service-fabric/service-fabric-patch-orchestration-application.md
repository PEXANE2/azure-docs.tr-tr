---
title: Azure Service Fabric Patch düzenleme uygulaması | Microsoft Docs
description: Service Fabric kümesinde işletim sistemi düzeltme eki uygulamayı otomatik hale getirmek için uygulama.
services: service-fabric
documentationcenter: .net
author: khandelwalbrijeshiitr
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: brkhande
ms.openlocfilehash: 2aa2dd8373a9568478a02691ca5e6a43e80cd408
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71289420"
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Service Fabric kümenizdeki Windows işletim sistemini düzeltme eki uygulama

> 
> [!IMPORTANT]
> Uygulama sürümü 1,2. *, 30 Nisan 2019 ' de desteden geçiyor. Lütfen en son sürüme yükseltin.


[Azure sanal makine ölçek kümesi otomatik işletim sistemi görüntüsü yükseltmeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) , Işletim sistemlerinizi Azure 'Da düzeltme eki uygulama konusunda en iyi uygulamadır ve düzeltme eki düzenleme uygulaması (POA), Service dokuları Repairmanager sistemleri hizmeti 'nin etrafında bir sarmalayıcı Azure olmayan barındırılan kümeler için yapılandırma tabanlı işletim sistemi düzeltme eki zamanlamasını sunar. Azure dışı barındırılan kümeler için POA gerekli değildir, ancak yükseltme etki alanları tarafından düzeltme eki yüklemeyi zamanlama, kapalı kalma süresi olmadan Service Fabric kümeleri konaklarının yaması için gereklidir.

POA, kapalı kalma süresi olmadan bir Service Fabric kümesinde işletim sistemi düzeltme eki uygulamayı otomatikleştiren bir Azure Service Fabric uygulamasıdır.

Düzeltme Eki düzenleme uygulaması aşağıdaki özellikleri sağlar:

- **Otomatik işletim sistemi güncelleştirme yüklemesi**. İşletim sistemi güncelleştirmeleri otomatik olarak indirilir ve yüklenir. Küme düğümleri, küme kapalı kalma süresi olmadan gerektiği şekilde yeniden başlatılır.

- **Küme durumunu algılayan düzeltme eki uygulama ve sistem durumu tümleştirmesi**. Güncelleştirmeler uygulanırken, düzeltme eki düzenleme uygulaması, küme düğümlerinin sistem durumunu izler. Küme düğümleri tek seferde bir veya bir yükseltme etki alanında bir düğüm yükseltir. Düzeltme eki uygulama işlemi nedeniyle kümenin sistem durumu kapalıysa, sorunu gidermek için düzeltme eki uygulama durdurulur.

## <a name="internal-details-of-the-app"></a>Uygulamanın iç ayrıntıları

Düzeltme Eki düzenleme uygulaması aşağıdaki alt bileşenleri oluşur:

- **Düzenleyici hizmeti**: Bu durum bilgisi olan bu hizmetin sorumluluğundadır:
    - Tüm kümedeki Windows Update işi koordine edin.
    - Tamamlanan Windows Update işlemlerinin sonucunu depolama.
- **Düğüm Aracısı hizmeti**: Bu durum bilgisi olmayan bu hizmet tüm Service Fabric küme düğümlerinde çalışır. Hizmetin sorumluluğundadır:
    - Düğüm Aracısı NTService önyükleniyor.
    - Düğüm Aracısı NTService izleniyor.
- **Düğüm Aracısı NTService**: Bu Windows NT hizmeti, daha üst düzey bir ayrıcalıkla (SISTEM) çalışır. Buna karşılık, düğüm Aracısı hizmeti ve Düzenleyici hizmeti alt düzey bir ayrıcalıkta (ağ HIZMETI) çalışır. Hizmet, tüm küme düğümlerinde aşağıdaki Windows Update işleri gerçekleştirmekten sorumludur:
    - Düğümdeki otomatik Windows Update devre dışı bırakılıyor.
    - Windows Update, Kullanıcı tarafından girilen ilkeye göre indiriliyor ve yükleniyor.
    - Makine gönderi Windows Update yüklemesi yeniden başlatılıyor.
    - Windows güncelleştirmelerinin sonuçları düzenleyici hizmetine yükleniyor.
    - Tüm yeniden denemeler tüketildikten sonra bir işlemin başarısız olması durumunda durum raporlarını raporlama.

> [!NOTE]
> Düzeltme Eki düzenleme uygulaması, düğümü devre dışı bırakmak veya etkinleştirmek ve sistem durumu denetimleri gerçekleştirmek için Service Fabric onarım Yöneticisi sistem hizmetini kullanır. Düzeltme Eki düzenleme uygulaması tarafından oluşturulan onarım görevi her düğüm için Windows Update ilerlemesini izler.

## <a name="prerequisites"></a>Önkoşullar

> [!NOTE]
> Gerekli en düşük .NET Framework sürümü 4,6.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Onarım Yöneticisi hizmetini etkinleştir (zaten çalışmıyorsa)

Düzeltme Eki düzenleme uygulaması, kümede onarım Yöneticisi sistem hizmetinin etkinleştirilmesini gerektirir.

#### <a name="azure-clusters"></a>Azure kümeleri

Gümüş dayanıklılık katmanındaki Azure kümelerinde varsayılan olarak onarım Yöneticisi hizmeti etkinleştirilmiştir. Altın dayanıklılık katmanındaki Azure kümeleri, bu kümelerin ne zaman oluşturulduğuna bağlı olarak onarım Yöneticisi hizmeti etkin olabilir veya olmayabilir. Bronz dayanıklılık katmanındaki Azure kümelerine, varsayılan olarak, onarım Yöneticisi hizmeti etkin değildir. Hizmet zaten etkinleştirilmişse, Service Fabric Explorer sistem hizmetleri bölümünde çalıştığını görebilirsiniz.

##### <a name="azure-portal"></a>Azure portal
Kümeyi ayarlama sırasında Azure portal onarım yöneticisini etkinleştirebilirsiniz. Küme yapılandırması sırasında **eklenti özellikleri** altında **onarım Yöneticisi** Ekle seçeneğini belirleyin.
![Azure portal Onarım Yöneticisi etkinleştirme görüntüsü](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager dağıtım modeli
Alternatif olarak, yeni ve mevcut Service Fabric kümelerinde onarım Yöneticisi hizmetini etkinleştirmek için [Azure Resource Manager dağıtım modelini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) kullanabilirsiniz. Dağıtmak istediğiniz kümenin şablonunu alın. Örnek şablonları kullanabilir ya da özel bir Azure Resource Manager dağıtım modeli şablonu oluşturabilirsiniz. 

[Azure Resource Manager dağıtım modeli şablonunu](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm)kullanarak onarım Yöneticisi hizmetini etkinleştirmek için:

1. İlk olarak, `apiversion` `Microsoft.ServiceFabric/clusters` kaynak için olarak `2017-07-01-preview` ayarlanmış olduğunu denetleyin. Farklıysa, değerini değere `apiVersion` `2017-07-01-preview` veya daha yükseğe güncelleştirmeniz gerekir:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Şimdi, `addonFeatures` `fabricSettings` bölümünden sonra aşağıdaki bölümü ekleyerek onarım Yöneticisi hizmetini etkinleştirin:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Küme şablonunuzu bu değişikliklerle güncelleştirdikten sonra, bu değişiklikleri uygulayın ve yükseltmenin bitmesini sağlayın. Artık kümenizde çalışan onarım Yöneticisi sistem hizmetini görebilirsiniz. Bu, Service Fabric Explorer `fabric:/System/RepairManagerService` sistem hizmetleri bölümünde çağırılır. 

### <a name="standalone-on-premises-clusters"></a>Tek başına şirket içi kümeler

Yeni ve mevcut Service Fabric kümesinde onarım Yöneticisi hizmetini etkinleştirmek için [tek başına Windows kümesi yapılandırma ayarlarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) kullanabilirsiniz.

Onarım Yöneticisi hizmetini etkinleştirmek için:

1. İlk olarak, `apiversion` [genel küme yapılandırmalarının](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) ' ın veya daha yüksek `04-2017` olarak ayarlandığından emin olun:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Şimdi aşağıda gösterildiği gibi `addonFeatures` `fabricSettings` bölümden sonra aşağıdaki bölümü ekleyerek onarım Yöneticisi hizmetini etkinleştirin:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Güncelleştirilmiş küme bildirimini kullanarak, [Yeni bir küme oluşturun](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) veya [küme yapılandırmasını yükseltmek için](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server)küme bildiriminizi bu değişikliklerle güncelleştirin. Küme güncelleştirilmiş küme bildirimiyle çalışmaya başladıktan sonra, kümenizde çalışan onarım Yöneticisi sistem hizmetini, Service Fabric Gezgini 'ndeki sistem hizmetleri bölümünde olarak `fabric:/System/RepairManagerService`görebilirsiniz.

### <a name="configure-windows-updates-for-all-nodes"></a>Tüm düğümler için Windows güncelleştirmelerini yapılandırma

Aynı anda birden çok küme düğümü yeniden başlatabildiğinden otomatik Windows güncelleştirmeleri kullanılabilirlik kaybına neden olabilir. Düzeltme Eki düzenleme uygulaması, varsayılan olarak her küme düğümündeki otomatik Windows Update devre dışı bırakmayı dener. Ancak, ayarlar bir yönetici veya grup ilkesi tarafından yönetiliyorsa, Windows Update ilkesini "Indirmadan önce bildir" olarak ayarlamayı öneririz.

## <a name="download-the-app-package"></a>Uygulama paketini indirin

Uygulama paketini indirmek için lütfen düzeltme eki düzenleme uygulamasının GitHub sürüm [sayfasını](https://github.com/microsoft/Service-Fabric-POA/releases/latest/) ziyaret edin.

## <a name="configure-the-app"></a>Uygulamayı yapılandırma

Düzeltme Eki düzenleme uygulamasının davranışı gereksinimlerinizi karşılayacak şekilde yapılandırılabilir. Uygulama oluşturma veya güncelleştirme sırasında uygulama parametresine geçirerek varsayılan değerleri geçersiz kılın. Uygulama parametreleri `ApplicationParameter` `Start-ServiceFabricApplicationUpgrade` veya cmdlet'leribelirtilereksağlayabilirsiniz.`New-ServiceFabricApplication`

|**Parametre**        |**Tür**                          | **Ayrıntılar**|
|:-|-|-|
|MaxResultsToCache    |Uzun                              | Önbelleğe alınması gereken Windows Update sonuçlarının en fazla sayısı. <br>Varsayılan değer 3000 ' dir: <br> -Düğüm sayısı 20 ' dir. <br> -Ayda bir düğümde gerçekleşen güncelleştirmelerin sayısı beş ' dür. <br> -İşlem başına sonuç sayısı 10 olabilir. <br> -Son üç aya ait sonuçlar depolanmalıdır. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy, Windows güncelleştirmelerini Service Fabric kümesi düğümlerine yüklemek için Düzenleyici hizmeti tarafından kullanılacak ilkeyi gösterir.<br>                         İzin verilen değerler şunlardır: <br>                                                           <b>Nodewise</b>. Windows Update tek seferde bir düğüm yüklenir. <br>                                                           <b>Upgradedomainwise</b>. Windows Update tek seferde bir yükseltme etki alanı yüklenir. (En fazla, bir yükseltme etki alanına ait tüm düğümler Windows Update için de kullanılabilir.)<br> Kümeniz için en uygun ilke hangisi olduğuna karar vermek için [SSS](#frequently-asked-questions) bölümüne bakın.
|LogsDiskQuotaInMB   |Uzun  <br> Varsayılanını 1024)               |Düğümlerde yerel olarak kalıcı hale getirebilen, en büyük düzeltme eki düzenleme uygulaması günlük boyutu (MB).
| WUQuery               | dize<br>Varsayılanını "IsInstalled = 0")                | Windows güncelleştirmelerini almak için sorgu. Daha fazla bilgi için bkz [. Wuquery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| Installwindowsosonlyupdates | Boole değeri <br> (varsayılan: false)                 | Hangi güncelleştirmelerin indirilip yükleneceğini denetlemek için bu bayrağı kullanın. Aşağıdaki değerlere izin verilir <br>true-yalnızca Windows işletim sistemi güncelleştirmelerini kurar.<br>false-makinede bulunan tüm güncelleştirmeleri yükleme.          |
| Wuoperationtimeoutınminutes | Int <br>Varsayılanını 90)                   | Herhangi bir Windows Update işlemi için (arama veya indirme veya yükleme) zaman aşımını belirtir. İşlem belirtilen zaman aşımı süresi içinde tamamlanmazsa, durdurulur.       |
| WURescheduleCount     | Int <br> Varsayılanını e                  | Hizmetin bir işlemin kalıcı olarak başarısız olması durumunda Windows Update 'in müşteri sizinle randevusunu en fazla kaç kez başarısız olduğunu.          |
| WURescheduleTimeInMinutes | Int <br>Varsayılanını ila | Hizmetin Windows Update 'in müşteri sizinle randevusunu hata durumunda çalıştığı zaman aralığı devam ediyor. |
| WUFrequency           | Virgülle ayrılmış dize (varsayılan: "Haftalık, Çarşamba, 7:00:00")     | Windows Update yükleme sıklığı. Biçim ve olası değerler şunlardır: <br>-Aylık, gg, ss: DD: SS, örneğin, aylık, 5, 12:22:32.<br>Alan gg (gün) için izin verilen değerler, 1-28 ve "Last" aralığı arasındaki sayılardır. <br> -Haftalık, gün, ss: DD: SS, örneğin, haftalık, Salı, 12:22:32.  <br> -Günlük, ss: DD: SS, örneğin, günlük, 12:22:32.  <br> -None Windows Update gerçekleştirilmeyeceğini gösterir.  <br><br> Saatlerin UTC olarak olduğunu unutmayın.|
| AcceptWindowsUpdateEula | Boole değeri <br>(Varsayılan: true) | Bu bayrak ayarlanarak uygulama, makinenin sahibi adına Windows Update için Son Kullanıcı Lisans sözleşmesini kabul eder.              |

> [!TIP]
> Windows Update hemen gerçekleşmesini istiyorsanız uygulama dağıtım zamanına göre ayarlayın `WUFrequency` . Örneğin, beş düğümlü bir test kümeniz olduğunu ve uygulamayı 5:00 PM UTC 'de dağıtmayı planladığınızı varsayalım. Uygulama yükseltme veya dağıtımının en fazla 30 dakika sürdüğünü varsaydıysanız, WUFrequency 'yi "günlük, 17:30:00" olarak ayarlayın

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

1. Kümeyi hazırlamak için tüm önkoşul adımlarını sona erdirin.
2. Düzeltme Eki düzenleme uygulamasını diğer Service Fabric uygulamalar gibi dağıtın. Uygulamayı PowerShell kullanarak dağıtabilirsiniz. [PowerShell kullanarak uygulama dağıtma ve kaldırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications)bölümündeki adımları izleyin.
3. Uygulamayı dağıtım sırasında yapılandırmak için öğesini `ApplicationParameter` `New-ServiceFabricApplication` cmdlet 'e geçirin. Size kolaylık olması için, uygulama ile birlikte Deploy. ps1 betiğini sağladık. Betiği kullanmak için:

    - Kullanarak `Connect-ServiceFabricCluster`bir Service Fabric kümesine bağlanın.
    - Uygun `ApplicationParameter` değer ile birlikte Deploy. ps1 PowerShell betiğini yürütün.

> [!NOTE]
> Betiği ve uygulama klasörü PatchOrchestrationApplication klasörünü aynı dizinde saklayın.

## <a name="upgrade-the-app"></a>Uygulamayı yükseltme

PowerShell kullanarak var olan bir düzeltme eki düzenleme uygulamasını yükseltmek için, [PowerShell kullanarak uygulama yükseltme Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell)içindeki adımları izleyin.

## <a name="remove-the-app"></a>Uygulamayı kaldırma

Uygulamayı kaldırmak için [PowerShell kullanarak uygulama dağıtma ve kaldırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications)bölümündeki adımları izleyin.

Size kolaylık olması için, uygulama ile birlikte Deploy. ps1 betiğini sağladık. Betiği kullanmak için:

  - Kullanarak ```Connect-ServiceFabricCluster```bir Service Fabric kümesine bağlanın.

  - PowerShell betiğini undeploy. ps1 ' i yürütün.

> [!NOTE]
> Betiği ve uygulama klasörü PatchOrchestrationApplication klasörünü aynı dizinde saklayın.

## <a name="view-the-windows-update-results"></a>Windows Update sonuçlarını görüntüleme

Düzeltme Eki düzenleme uygulaması, bekleyen sonuçları kullanıcıya göstermek için REST API 'Lerini kullanıma sunar. JSON sonucu örneği:
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

JSON alanları aşağıda açıklanmıştır.

Alan | Değerler | Ayrıntılar
-- | -- | --
OperationResult | 0-başarılı<br> 1-hatalarla başarılı oldu<br> 2-başarısız oldu<br> 3-iptal edildi<br> 4-zaman aşımı Ile iptal edildi | Genel işlemin sonucunu gösterir (genellikle bir veya daha fazla güncelleştirmenin yüklenmesini içeren).
resultCode | OperationResult ile aynı | Bu alan, tek bir güncelleştirme için yükleme işleminin sonucunu gösterir.
OperationType | 1-yükleme<br> 0-arama ve Indirme.| Yükleme, sonuçlarda varsayılan olarak gösterilecek tek OperationType ' dır.
WindowsUpdateQuery | Varsayılan değer "IsInstalled = 0" |Güncelleştirmeleri aramak için kullanılan Windows Update sorgusu. Daha fazla bilgi için bkz [. Wuquery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
RebootRequired | doğru-yeniden başlatma gerekiyordu<br> false-yeniden başlatma gerekli değil | Güncelleştirmelerin yüklenmesinin tamamlanabilmesi için yeniden başlatma işleminin gerekip gerekmediğini gösterir.
OperationStartTime | DateTime | İşlemin (Indirme/yükleme) başladığı süreyi belirtir.
OperationTime | DateTime | İşlemin (Indirme/yükleme) tamamlandığı süreyi belirtir.
HResult | 0-başarılı<br> diğer hata| UpdateID "7392acaf-6a85-427c-8a8d-058c25beb0d6" olan Windows Update hatasının nedenini gösterir.

Henüz bir güncelleştirme zamanlanmamışsa, JSON sonucu boştur.

Windows Update sonuçları sorgulamak için kümede oturum açın. Ardından, Koordinatör hizmetinin birincili için çoğaltma adresini bulun ve tarayıcıdan URL 'yi ziyaret edin: http://&lt;Replica-IP&gt;:&lt;applicationport&gt;/patchorchestrationapplication/v1/ GetWindowsUpdateResults.

Koordinatör hizmeti için REST uç noktası dinamik bir bağlantı noktasına sahiptir. Tam URL 'yi denetlemek için Service Fabric Explorer başvurun. Örneğin, sonuçları adresinde `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`bulabilirsiniz.

![REST uç noktasının görüntüsü](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Kümede ters proxy etkinse, URL 'ye küme dışından da erişebilirsiniz.
İsabet etmesi gereken uç nokta&lt;http://SERVERURL&gt;:&lt;daproxyport&gt;/patchorchestrationapplication/koordinattorservice/v1/getwindowsupdateresults.

Kümede ters proxy 'yi etkinleştirmek için [Azure Service Fabric 'de ters proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy)'deki adımları izleyin. 

> 
> [!WARNING]
> Ters proxy yapılandırıldıktan sonra, kümedeki bir HTTP uç noktasını kullanıma sunan tüm mikro hizmetler, küme dışından adreslenebilir.

## <a name="diagnosticshealth-events"></a>Tanılama/sistem durumu olayları

Aşağıdaki bölümde, Service Fabric kümelerinde Patch Orchestration uygulaması aracılığıyla düzeltme eki güncelleştirmeleriyle ilgili sorunları ayıklama/Tanılama hakkında bilgi sağlanır.

> [!NOTE]
> Aşağıdaki birçok kendi kendine tanılama iyileştirmelerini almak için, POA 'nın v 1.4.0 sürümünün yüklü olması gerekir.

NodeAgentNTService, düğümlere güncelleştirme yüklemek için [onarım görevleri](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtask?view=azure-dotnet) oluşturur. Ardından her görev, Koordinatör Torservice tarafından görev onay ilkesine göre hazırlanır. Hazırlanan görevler son olarak, küme sağlıksız durumdaysa hiçbir görevi onaylayamayacak Onarım Yöneticisi tarafından onaylanır. Güncelleştirmelerin düğüm üzerinde nasıl ilerleyerek anlamak için adım adım devam etmenizi sağlar.

1. Her düğümde çalışan NodeAgentNTService, zamanlanan sürede kullanılabilir Windows Update arar. Güncelleştirmeler varsa, bu, devam edip düğüme indirilir.
2. Bu güncelleştirmeler indirildikten sonra NodeAgentNTService, POS___ < unique_id > adlı düğüm için karşılık gelen onarım görevi oluşturur. Bu onarım görevlerini, düğüm ayrıntıları bölümünde [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps) cmdlet 'ini kullanarak veya SFX içinde görüntüleyebilir. Onarma görevi oluşturulduktan sonra, hızlı bir şekilde talep edilen [duruma](https://docs.microsoft.com/dotnet/api/system.fabric.repair.repairtaskstate?view=azure-dotnet)gider.
3. Koordinatör hizmeti, belirli bir durumda onarma görevlerini düzenli olarak arar ve öne geçer ve TaskApprovalPolicy temel alınarak durumu hazırlamaya yönelik olarak güncelleştirir. TaskApprovalPolicy NodeWise olacak şekilde yapılandırıldıysa, bir düğüme karşılık gelen bir onarım görevi yalnızca hazırlama/onaylama/yürütme/geri yükleme durumunda bulunan başka bir onarım görevi yoksa hazırlanır. Benzer şekilde, UpgradeWise TaskApprovalPolicy söz konusu olduğunda, yukarıdaki durumlarda yalnızca aynı yükseltme etki alanına ait olan düğümler için görevler vardır. Onarım görevi hazırlık durumuna taşındıktan sonra, karşılık gelen Service Fabric düğümü "restart" olarak [devre dışı bırakılır](https://docs.microsoft.com/powershell/module/servicefabric/disable-servicefabricnode?view=azureservicefabricps) .

   POA (v 1.4.0 ve üzeri), düzeltme eki eklenen düğümleri göstermek için Koordinatör Terservice üzerinde "ClusterPatchingStatus" özelliği ile olayları gönderir. Aşağıdaki görüntüde güncelleştirmelerin _poanode_0 üzerine yüklendiğini gösterir:

    [![Küme düzeltme eki uygulama durumu görüntüsü](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png)](media/service-fabric-patch-orchestration-application/clusterpatchingstatus.png#lightbox)

4. Düğüm devre dışı bırakıldıktan sonra onarım görevi yürütülüyor durumuna taşınır.
   
   >[!NOTE]
   > Devre dışı bırakılmış durumda olan bir düğüm yeni bir onarım görevini engelleyebilir ve bu, kümedeki düzeltme eki uygulama işlemini durdurur.

5. Onarım görevi yürütülüyor durumundaysa, bu düğümdeki düzeltme eki yüklemesi başlar. Burada, düzeltme eki yüklendikten sonra düğüm, düzeltme ekine bağlı olarak yeniden başlatılamaz veya başlatılamayabilir. Onarım görevinin geri yükleme durumuna taşındığını, bu da düğümü yeniden geri yüklemeyi ve ardından tamamlandı olarak işaretlenmesini sağlar.

   V 1.4.0 ve uygulamanın üzerindeki sürümlerinde, "WUOperationStatus-[Düğene]" özelliğindeki NodeAgentService üzerindeki sistem durumu olaylarına bakarak güncelleştirme durumu bulunabilir. Aşağıdaki resimlerde vurgulanan bölümler, ' poanode_0 ' ve ' poanode_2 ' düğümündeki Windows Update 'in durumunu gösterir:

   [![Windows Update işlem durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusa.png#lightbox)

   [![Windows Update işlem durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusb.png#lightbox)

   Ayrıca, kümeye bağlanarak ve [Get-ServiceFabricRepairTask](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricrepairtask?view=azureservicefabricps)kullanarak onarım görevinin durumunu getirerek PowerShell kullanarak ayrıntıları alabilir. Aşağıdaki örnekte gösterildiği gibi, "POS__poanode_2_125f2969-933c-4774-85D1-ebdf85e79f15" görevinin Downloadtamamlanma durumunda olduğunu gösterir. Güncelleştirmelerin "poanode_2" düğümüne indirildiği ve görev yürütme durumuna taşındıktan sonra yüklemenin denendiği anlamına gelir.

   ``` powershell
    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k = Get-ServiceFabricRepairTask -TaskId "POS__poanode_2_125f2969-933c-4774-85d1-ebdf85e79f15"

    D:\service-fabric-poa-bin\service-fabric-poa-bin\Release> $k.ExecutorData
    {"ExecutorSubState":2,"ExecutorTimeoutInMinutes":90,"RestartRequestedTime":"0001-01-01T00:00:00"}
    ```

   Daha sonra bulunmakta olan daha fazla yoksa, Windows olay günlüklerini kullanarak sorun hakkında daha fazla bilgi edinmek için belirli VM/VM 'lerde oturum açın. Yukarıda bahsedilen onarma görevi yalnızca şu yürütücü alt durumlarına sahip olabilir:

      Yürütişalt durumu | Ayrıntı
    -- | -- 
      Hiçbiri = 1 |  Düğümde devam eden bir işlem olmadığını gösterir. Olası durum geçişleri.
      DownloadCompleted = 2 | İndirme işleminin başarılı, kısmi hata veya hata ile tamamlandığını gösterir.
      Instalonaylanan onaylandı = 3 | İndirme işleminin daha önce tamamlandığını ve Onarım Yöneticisi yüklemeyi onayladığını gösterir.
      Instalınstalprogress = 4 | Onarım görevinin yürütme durumuna karşılık gelir.
      Yüklemetamamlandı = 5 | Yüklemenin başarılı, kısmen başarılı veya başarısız ile tamamlandığını belirtir.
      RestartRequested = 6 | Düzeltme Eki yüklemesinin tamamlandığını ve düğümde bekleyen bir yeniden başlatma eylemi olduğunu belirtir.
      RestartNotNeeded = 7 |  Düzeltme eki yüklemesi tamamlandıktan sonra yeniden başlatmanın gerekli olmadığı anlamına gelir.
      RestartCompleted = 8 | Yeniden başlatmanın başarıyla tamamlandığını gösterir.
      OperationCompleted = 9 | Windows Update işlemi başarıyla tamamlandı.
      Operationdurdurulan = 10 | Windows Update işleminin iptal edileceği anlamına gelir.

6. Uygulamanın v 1.4.0 ve üzeri sürümlerde, bir düğümdeki güncelleştirme denemesi tamamlandığında, bir sonraki deneyene, güncelleştirme indirip yüklemeye başlamak için NodeAgentService 'e "WUOperationStatus-[Düğene]" özelliğine sahip bir olay gönderilir. Aşağıdaki görüntüye bakın:

     [![Windows Update işlem durumu görüntüsü](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png)](media/service-fabric-patch-orchestration-application/wuoperationstatusc.png#lightbox)

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Düzeltme Eki düzenleme uygulama günlükleri Service Fabric çalışma zamanı günlüklerinin bir parçası olarak toplanır.

Günlükleri Tanılama Aracı/seçtiğiniz işlem hattı aracılığıyla yakalamak istemeniz durumunda. Düzeltme Eki düzenleme uygulaması olayları [olay kaynağı](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netframework-4.5.1) aracılığıyla günlüğe kaydetmek için aşağıdaki sabit sağlayıcı kimliklerini kullanır

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Sistem durumu raporları

Düzeltme Eki düzenleme uygulaması, aşağıdaki durumlarda sistem durumu raporlarını Koordinatör hizmetine veya düğüm Aracısı hizmetine göre de yayımlar:

#### <a name="the-node-agent-ntservice-is-down"></a>NTService düğüm Aracısı çalışmıyor

Düğüm Aracısı NTService düğüm üzerinde kapalıysa, düğüm Aracısı hizmetine karşı bir uyarı düzeyi sistem durumu raporu oluşturulur.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Onarım Yöneticisi hizmeti etkin değil

Onarım Yöneticisi hizmeti kümede bulunamazsa, Düzenleyici hizmeti için bir uyarı düzeyi sistem durumu raporu oluşturulur.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

S. **Düzeltme Eki düzenleme uygulaması çalışırken Kümemin neden hata durumunda olduğunu görüyorum?**

A. Yükleme işlemi sırasında, düzeltme eki düzenleme uygulaması düğümleri devre dışı bırakır veya yeniden başlatır, bu durum geçici olarak kümenin sistem durumuna yol açabilir.

Uygulamanın ilkesine bağlı olarak, düzeltme eki uygulama sırasında tek bir düğüm aşağı gidebilir *veya* yükseltme etki alanının tamamı aynı anda devam edebilir.

Windows Update yüklemesinin sonuna kadar, düğümler yeniden başlatma sonrası yeniden etkinleştirilmiştir.

Aşağıdaki örnekte, iki düğüm çalışmadığından ve MaxPercentageUnhealthyNodes İlkesi ihlal edildiğinden, küme geçici olarak bir hata durumuna geçti. Düzeltme Eki işlemi devam edene kadar hata geçicidir.

![Sağlıksız kümenin görüntüsü](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sorun devam ederse, sorun giderme bölümüne bakın.

S. **Düzeltme Eki düzenleme uygulaması uyarı durumunda**

A. Uygulamaya karşı gönderilen bir sistem durumu raporunun kök nedeni olup olmadığını denetleyin. Genellikle uyarı, sorunun ayrıntılarını içerir. Sorun geçici ise, uygulamanın bu durumdan otomatik olarak kurtarılması beklenir.

S. **Küm sağlıksız ve acil bir işletim sistemi güncelleştirmesi yapmam gerekiyorsa ne yapabilirim?**

A. Düzeltme Eki düzenleme uygulaması, küme kötü durumda olduğu sürece güncelleştirmeleri yüklemez. Düzeltme Eki düzenleme uygulaması iş akışının engellemesini kaldırmak için kümenizi sağlıklı bir duruma getirmeyi deneyin.

S. **Kümem için TaskApprovalPolicy ' NodeWise ' veya ' UpgradeDomainWise ' olarak ayarlanmalıdır mi?**

A. ' UpgradeDomainWise ', bir yükseltme etki alanına ait tüm düğümlerin paralel olarak düzeltilmesiyle, genel kümenin daha hızlı bir şekilde iyileştirmesini sağlar. Bu, düzeltme eki uygulama işlemi sırasında yükseltme etki alanına ait düğümlerin ( [devre dışı](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) durumda) kullanılamadığı anlamına gelir.

' NodeWise ' ilke düzeltme eklerinin tek seferde yalnızca bir düğümü olması durumunda bu, genel küme düzeltme eki uygulama işleminin uzun sürme süresini gösterir. Ancak, en fazla, düzeltme eki işlemi sırasında yalnızca bir düğüm kullanılamaz ( [devre dışı](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) durumda).

Kümeniz, düzeltme eki uygulama sırasında N-1 yükseltme etki alanı sayısı üzerinde çalışmaya tolerans sağlayabilir (burada N, kümenizdeki toplam yükseltme etki alanı sayısıdır), ilkeyi ' UpgradeDomainWise ' olarak ayarlayabilir, aksi takdirde ' NodeWise ' olarak ayarlayabilirsiniz.

S. **Bir düğüme yama ne kadar sürer?**

A. Bir düğümün düzeltme eki uygulama birkaç dakika sürebilir (örneğin: [Windows Defender tanım güncelleştirmeleri](https://www.microsoft.com/en-us/wdsi/definitions)) Saat (örneğin: [Windows toplu güncelleştirmeleri](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%20server%20cumulative%20update)). Bir düğümü düzeltme için gereken süre, çoğunlukla 
 - Güncelleştirmelerin boyutu
 - Düzeltme eki uygulama penceresinde uygulanması gereken güncelleştirme sayısı
 - Güncelleştirmelerin yüklenmesi için geçen süre (gerekliyse) ve yeniden başlatma sonrası yükleme adımlarını tamamlaymanız gerekir.
 - VM/makine ve ağ koşullarının performansı.

S. **Bir kümenin tamamına yama ne kadar sürer?**

A. Bir kümenin tamamına yama yapmak için gereken süre aşağıdaki etkenlere bağlıdır:

- Bir düğümü düzeltme için gereken süre.
- Düzenleyici hizmetinin ilkesi. -Varsayılan ilke `NodeWise`, tek seferde yalnızca bir düğüm düzeltme eki uygulama ile sonuçlanır, bundan `UpgradeDomainWise`daha yavaş olur. Örneğin: Bir düğümün düzeltme için yaklaşık 1 saat sürerse, her biri 4 düğüm içeren, 5 yükseltme etki alanına sahip bir 20 düğüm (aynı düğüm türü) kümesine yama yapmak için.
    - İlkenin tamamı, ilke ise, tüm kümeye düzeltme eki uygulamak için ~ 20 saat sürer.`NodeWise`
    - İlke ise ~ 5 saat sürer`UpgradeDomainWise`
- Küme yükü-her düzeltme eki uygulama, müşteri iş yükünün kümedeki diğer kullanılabilir düğümlere yeniden konumlandırması gerektirir. YAMAKTA olan düğüm bu süre içinde [devre dışı bırakılıyor](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabling) durumunda olabilir. Küme yoğun yük yakın çalışıyorsa, devre dışı bırakma işlemi daha uzun sürer. Bu nedenle, bu tür bir işlem koşulında genel düzeltme eki uygulama işlemi yavaş görünebilir.
- Düzeltme sırasında küme sistem durumu sorunları- [kümenin sistem](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction) durumunda herhangi bir [azalma](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet#System_Fabric_Health_HealthState_Error) , düzeltme eki uygulama işlemini kesintiye uğratır. Bu, tüm kümeyi düzeltme için gereken toplam zamanı ekler.

S. **Windows Update sonuçlarında bazı güncelleştirmeleri neden görüyorum, ancak makinede Windows Update geçmişi altında değil REST API.**

A. Bazı ürün güncelleştirmeleri yalnızca ilgili güncelleştirme/düzeltme eki geçmişinde görüntülenir. Örneğin, Windows Defender güncelleştirmeleri Windows Server 2016 ' de Windows Update geçmişi 'nde görünebilir veya görünmeyebilir.

S. **Geliştirici kümemi (tek düğümlü küme) yaması için düzeltme eki düzenleme uygulaması kullanılabilir mi?**

A. Hayır, düzeltme eki düzenleme uygulaması tek düğümlü bir kümeyi düzeltme için kullanılamaz. [Service Fabric sistem hizmetleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) veya herhangi bir müşteri uygulaması kapalı kalma süresi olduğu ve bu nedenle düzeltme için herhangi bir onarım işi hiçbir şekilde onarım Yöneticisi tarafından onaylanmayacak şekilde, bu sınırlama tasarıma göre yapılır.

S. **Linux üzerinde Nasıl yaparım? yama kümesi düğümleri mi?**

A. Linux 'ta güncelleştirmeleri düzenlemek için bkz. [Azure sanal makine ölçek kümesi otomatik işletim sistemi görüntüsü yükseltmeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) .

S.**güncelleştirme döngüsüyle neden uzun sürüyor?**

A. JSON sonucu için sorgu yapın, ardından tüm düğümlerin güncelleştirme döngüsünün girişi boyunca ilerleyin ve sonra OperationStartTime ve OperationTime (OperationCompletionTime) kullanarak her düğümde güncelleştirme yüklemesi tarafından gerçekleştirilen süreyi bulmayı deneyebilirsiniz. Üzerinde hiçbir güncelleştirme olmadığı büyük BIR zaman penceresi varsa, bunun nedeni kümenin hata durumunda olması ve bu onarım yöneticisinin diğer herhangi bir diğer onarım görevini onaylamadığı anlamına gelebilir. Güncelleştirme yüklemesinin herhangi bir düğümde uzun sürme olasılığı varsa, düğüm uzun zamandan güncelleştirilmemiş olabilir ve çok sayıda güncelleştirme bekleyen yükleme işlemi zaman alabilir. Düğüm üzerinde düzeltme eki uygulama, genellikle düğümün devre dışı bırakılmasının çekirdek/veri kaybı durumlarına neden olabileceği için bu durum, düğümün devre dışı bırakılması nedeniyle engellenmiş olduğu bir durum olabilir.

S. **POA düzeltme eki eklendiğinde düğümü devre dışı bırakmak neden gereklidir?**

A. Düzeltme Eki düzenleme uygulaması, düğüm üzerinde çalışan tüm Service Fabric hizmetlerini durduran/yeniden bulan ' Restart ' amacına sahip düğümü devre dışı bırakır. Bu, uygulamaların yeni ve eski dll 'lerin bir karışımını kullanarak bitmemesini sağlamak için yapılır, bu nedenle bir düğümü devre dışı bırakmadan düzeltme eki uygulamak önerilmez.

## <a name="disclaimers"></a>Bildirimler

- Düzeltme Eki düzenleme uygulaması kullanıcı adına Windows Update son kullanıcı lisans anlaşmasını kabul eder. İsteğe bağlı olarak, ayar uygulamanın yapılandırmasında kapatılabilir.

- Düzeltme Eki düzenleme uygulaması, kullanımı ve performansı izlemek için telemetri toplar. Uygulamanın telemetrisi, Service Fabric çalışma zamanının telemetri ayarı ayarını izler (varsayılan olarak açık).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="a-node-is-not-coming-back-to-up-state"></a>Düğüm, duruma geri gelmedi

**Düğüm devre dışı durumda kalmış olabilir, nedeni**:

Bir güvenlik denetimi bekliyor. Bu durumu gidermek için sağlıklı durumda yeterli sayıda düğüm bulunduğundan emin olun.

**Düğüm devre dışı bırakılmış bir durumda kalabilir, çünkü**:

- Düğüm el ile devre dışı bırakıldı.
- Devam eden bir Azure altyapı işi nedeniyle düğüm devre dışı bırakıldı.
- Düğüm, düğümü yama için düzeltme eki düzenleme uygulaması tarafından geçici olarak devre dışı bırakıldı.

**Düğüm aşağı durumunda kalmış olabilir, çünkü**:

- Düğüm, el ile bir duruma koyulandı.
- Düğüm, bir yeniden başlatma işlemi yaşıyor (düzeltme eki düzenleme uygulaması tarafından tetiklenebilir).
- Bozuk bir VM veya makine ya da ağ bağlantısı sorunları nedeniyle düğüm çalışmıyor.

### <a name="updates-were-skipped-on-some-nodes"></a>Bazı düğümlerde güncelleştirmeler atlandı

Düzeltme Eki düzenleme uygulaması, yeniden zamanlama ilkesine göre bir Windows güncelleştirmesi yüklemeye çalışır. Hizmet, düğümü kurtarmaya çalışır ve uygulama ilkesine göre güncelleştirmeyi atlar.

Böyle bir durumda, düğüm Aracısı hizmetine karşı uyarı düzeyi bir sistem durumu raporu oluşturulur. Windows Update sonucu hatanın olası nedenini da içerir.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Güncelleştirme yüklenirken kümenin sistem durumu hataya gidiyor

Hatalı bir Windows Update, belirli bir düğümdeki veya yükseltme etki alanındaki bir uygulamanın veya kümenin sistem durumunu getirebilir. Düzeltme Eki düzenleme uygulaması, küme sağlıklı bir şekilde yeniden başlatılana kadar sonraki Windows Update işlemine devam eder.

Yöneticinin, Windows Update nedeniyle uygulamanın veya kümenin neden sağlıksız olacağını belirlemesi gerekir.

## <a name="release-notes"></a>Sürüm Notları

>[!NOTE]
> Sürüm 1.4.0 'den başlayarak, sürüm notları ve yayınları GitHub sürüm [sayfasında](https://github.com/microsoft/Service-Fabric-POA/releases/)bulunabilir.

### <a name="version-110"></a>Sürüm 1.1.0
- Genel Yayın

### <a name="version-111"></a>Sürüm 1.1.1
- Nodeagentservice yüklemesinin önlendiği NodeAgentService 'in SetupEntryPoint içindeki bir hata düzeltildi.

### <a name="version-120"></a>Sürümü 1.2.0

- Sistem yeniden başlatma iş akışı etrafında hata düzeltmeleri.
- Onarım görevlerinin hazırlanması sırasında durum denetimi beklendiği gibi gerçekleşmediği için RM görevlerinin oluşturulmasında hata düzeltildi.
- Windows hizmeti POANodeSvc 'nin başlangıç modunu Auto ' dan Gecikmeli-otomatik ' e değiştirdi.

### <a name="version-121"></a>Sürüm 1.2.1

- Küme ölçeği azaltma iş akışında hata düzeltildi. Varolmayan düğümlere ait olmayan BIR onarım görevi için çöp toplama mantığı sunuldu.

### <a name="version-122"></a>Sürüm 1.2.2

- Çeşitli hata düzeltmeleri.
- İkili dosyalar artık imzalandı.
- Uygulama için sfpkg bağlantısı eklendi.

### <a name="version-130"></a>Sürüm 1.3.0

- Installwindowsosonlyupdates false olarak ayarlandığında, artık tüm kullanılabilir güncelleştirmeler yüklenir.
- Otomatik güncelleştirmeleri devre dışı bırakma mantığını değiştirdi. Bu, otomatik güncelleştirmelerin sunucu 2016 ve üzeri sürümlerde devre dışı bırakılmamasına neden olan bir hatayı düzeltir.
- Gelişmiş kullanım durumları için hem mikro hizmet mikro hizmetleri için parametreli yerleştirme kısıtlaması.

### <a name="version-131"></a>Sürüm 1.3.1
- POA 1.3.0, otomatik güncelleştirmeleri devre dışı bırakma hatası nedeniyle Windows Server 2012 R2 veya daha düşük bir süre içinde çalışmamakta olan gerileme düzeltiliyor. 
- Installwindowsosonlyupdates yapılandırmasının her zaman doğru olarak çekildiği hata düzeltiliyor.
- Installwindowsosonlyupdates varsayılan değeri false olarak değiştiriliyor.

### <a name="version-132"></a>Sürüm 1.3.2
- Geçerli düğüm adının alt kümesi olan bir ada sahip düğümler olması durumunda bir düğümde düzeltme eki uygulama yaşam döngüsünü gösteren bir sorunu düzeltme. Bu tür düğümler için mümkün olan, düzeltme eki uygulama kaçırıldı veya yeniden başlatma beklemede. 
