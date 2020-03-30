---
title: Bağımsız Küme Dağıtım Hazırlığı
description: Bir üretim iş yükünü işlemek için tasarlanmış bir küme dağıtmadan önce düşünülecek, ortamı nhazırlama ve küme yapılandırmasını oluşturma yla ilgili belgeler.
author: dkkapur
ms.topic: conceptual
ms.date: 9/11/2018
ms.author: dekapur
ms.openlocfilehash: 6a00b7d1b72d594c08021982b2448de6275414c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610072"
---
# <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Hizmet Kumaş Tek Başına küme dağıtımınızı planlayın ve hazırlayın

<a id="preparemachines"></a>Kümenizi oluşturmadan önce aşağıdaki adımları gerçekleştirin.

## <a name="plan-your-cluster-infrastructure"></a>Küme altyapınızı planlayın
"Sahip olduğunuz" makinelerde bir Service Fabric kümesi oluşturmak üzeresiniz, böylece kümenin ne tür hatalardan hayatta kalmak istediğinize karar verebilirsiniz. Örneğin, bu makinelere sağlanan ayrı güç hatlarına veya Internet bağlantılarına ihtiyacınız var mı? Buna ek olarak, bu makinelerin fiziksel güvenlik düşünün. Makineler nerede bulunur ve kimlerin bunlara erişmesi gerekir? Bu kararları verdikten sonra, makineleri mantıksal olarak çeşitli hata etki alanlarıyla eşleyebilirsiniz (sonraki adıma bakın). Üretim kümeleri için altyapı planlaması, test kümelerinden daha fazla ilgilidir.

## <a name="determine-the-number-of-fault-domains-and-upgrade-domains"></a>Hata etki alanlarının sayısını belirleme ve etki alanlarını yükseltme
[ *Hata etki alanı* (FD)](service-fabric-cluster-resource-manager-cluster-description.md) fiziksel bir hata birimidir ve veri merkezlerindeki fiziksel altyapıyla doğrudan ilişkilidir. Hata etki alanı, tek bir hata noktasını paylaşan donanım bileşenlerinden (bilgisayarlar, anahtarlar, ağlar ve daha fazlası) oluşur. Hata etki alanları ve raflar arasında 1:1 haritalama olmamasına rağmen, gevşek konuşma, her raf bir hata etki alanı olarak kabul edilebilir.

ClusterConfig.json'da FD'ler belirttiğiniz zaman, her FD için adı seçebilirsiniz. Service Fabric hiyerarşik FD'leri destekler, böylece altyapı topolojinizi onlara yansıtabilirsiniz.  Örneğin, aşağıdaki FD'ler geçerlidir:

* "faultDomain": "fd:/Room1/Rack1/Machine1"
* "faultDomain": "fd:/FD1"
* "faultDomain": "fd:/Room1/Rack1/PDU1/M1"

*Yükseltme etki alanı* (UD), düğümlerin mantıksal bir birimidir. Service Fabric tarafından düzenlenen yükseltmeler (uygulama yükseltmesi veya küme yükseltmesi) sırasında, ud'deki tüm düğümler yükseltmeyi gerçekleştirmek için aşağı alınırken, diğer UD'lerde bulunan düğümler isteklere hizmet etmek için kullanılabilir durumda kalır. Makinelerinizde gerçekleştirdiğiniz firmware yükseltmeleri ABD'li leri onurlandırmaz, bu nedenle bunları bir seferde bir makine yapmanız gerekir.

Bu kavramlar hakkında düşünmenin en basit yolu, fd'leri planlanmamış hata birimi ve UD'leri planlı bakım birimi olarak değerlendirmektir.

ClusterConfig.json'da UD'leri belirttiğiniz zaman, her UD için adı seçebilirsiniz. Örneğin, aşağıdaki adlar geçerlidir:

* "upgradeDomain": "UD0"
* "upgradeDomain": "UD1A"
* "upgradeDomain": "DomainRed"
* "upgradeDomain": "Mavi"

FD'ler ve ABD'ler hakkında daha ayrıntılı bilgi için [bkz.](service-fabric-cluster-resource-manager-cluster-description.md)

Üretimdeki bir küme, üretim ortamında desteklenebilmesi için en az üç FD'ye yayılmalıdır, düğümlerin bakımı ve yönetimi üzerinde tam kontrole sahipseniz, yani makinelerin güncellenmesi nden ve değiştirilmesinden siz sorumlusunuz. Makineler üzerinde tam denetiminizin olmadığı ortamlarda (diğer bir deyişle Amazon Web Services VM örnekleri) çalışan kümeler için, kümenizde en az beş FD olmalıdır. Her FD'nin bir veya daha fazla düğümü olabilir. Bu, zamanlamalarına bağlı olarak, kümeler halinde uygulamaların ve hizmetlerin çalışmasını engelleyebilecek makine yükseltmeleri ve güncelleştirmelerinin neden olduğu sorunları önlemek içindir.

## <a name="determine-the-initial-cluster-size"></a>İlk küme boyutunu belirleme

Genel olarak, kümenizdeki düğüm sayısı iş gereksinimlerinize, yani kümede kaç hizmet ve kapsayıcının çalışacağına ve iş yüklerinizi sürdürmek için kaç kaynağa ihtiyacınız olduğuna bağlı olarak belirlenir. Üretim kümeleri için, kümenizde 5 FD'yi kapsayan en az beş düğüm olmasını öneririz. Ancak, yukarıda açıklandığı gibi, düğümleriniz üzerinde tam denetime sahipseniz ve üç FD'ye yayılabiliyorsanız, üç düğüm de bu işi yapmalıdır.

Durum durumu yla çalışan test kümeleri üç düğüme sahip olmalıdır, oysa yalnızca durum disyükleri çalıştıran test kümelerinin yalnızca tek bir düğüme ihtiyacı vardır. Ayrıca, geliştirme amacıyla, belirli bir makinede birden fazla düğüm olabilir unutulmamalıdır. Ancak bir üretim ortamında Service Fabric, fiziksel veya sanal makine başına yalnızca bir düğüm destekler.

## <a name="prepare-the-machines-that-will-serve-as-nodes"></a>Düğüm görevi göreceksiniz makineleri hazırlamak

Kümeye eklemek istediğiniz her makine için önerilen bazı özellikler şunlardır:

* En az 16 GB RAM
* En az 40 GB kullanılabilir disk alanı
* 4 çekirdekli veya daha büyük bir CPU
* Tüm makineler için güvenli bir ağa veya ağlara bağlantı
* Windows Server OS yüklü (geçerli sürümler: 2012 R2, 2016, 1709 veya 1803). Service Fabric sürüm 6.4.654.9590 ve daha sonra sunucu 2019 ve 1809 destekler.
* [.NET Framework 4.5.1 veya üstü](https://www.microsoft.com/download/details.aspx?id=40773), tam yükleme
* [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/install/installing-windows-powershell)
* [RemoteRegistry hizmeti](https://technet.microsoft.com/library/cc754820) tüm makinelerde çalışıyor olmalıdır
* Servis Kumaş kurulum sürücü NTFS Dosya Sistemi olmalıdır

Kümeyi dağıtan ve yapılandıran küme yöneticisinin her makinede [yönetici ayrıcalıkları](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) olması gerekir. Service Fabric’i bir etki alanı denetleyicisine yükleyemezsiniz.

## <a name="download-the-service-fabric-standalone-package-for-windows-server"></a>Windows Server için Hizmet Kumaşı bağımsız paketini indirin
[Download Link - Service Fabric Alone Package - Windows Server](https://go.microsoft.com/fwlink/?LinkId=730690) ve paketin zipini, kümenin bir parçası olmayan bir dağıtım makinesine veya kümenizin bir parçası olacak makinelerden birine.

## <a name="modify-cluster-configuration"></a>Küme yapılandırmayı değiştirme
Bağımsız bir küme oluşturmak için kümenin belirtimini açıklayan bağımsız bir küme yapılandırması ClusterConfig.json dosyası oluşturmanız gerekir. Yapılandırma dosyasını aşağıdaki bağlantıda bulunan şablonlara dayandırabilirsiniz. <br>
[Bağımsız Küme Yapılandırmaları](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Bu dosyadaki bölümlerle ilgili ayrıntılar [için, bağımsız Windows kümesi için Yapılandırma ayarlarına](service-fabric-cluster-manifest.md)bakın.

İndirdiğiniz paketten ClusterConfig.json dosyalarından birini açın ve aşağıdaki ayarları değiştirin:

| **Yapılandırma Ayarı** | **Açıklama** |
| --- | --- |
| **DüğümTürleri** |Düğüm türleri küme düğümlerinizi çeşitli gruplara ayırmanızı sağlar. Bir kümenin en az bir NodeType'ı olmalıdır. Bir gruptaki tüm düğümlerin aşağıdaki ortak özellikleri vardır: <br> **Ad** - Bu düğüm türü adıdır. <br>**Bitiş Noktası Bağlantı Noktaları** - Bunlar, bu düğüm türüyle ilişkili çeşitli adlandırılmış uç noktaları (bağlantı noktaları)dır. Bu bildirimde başka bir şeyle çakışmadıkları ve makine/VM üzerinde çalışan başka bir uygulama tarafından kullanılmadıkları sürece istediğiniz bağlantı noktası numarasını kullanabilirsiniz. <br> **Yerleşim Özellikleri** - Bunlar, sistem hizmetleri veya hizmetleriniz için yerleşim kısıtlaması olarak kullandığınız bu düğüm türüne ait özellikleri açıklar. Bu özellikler, belirli bir düğüm için ekstra meta veri sağlayan kullanıcı tanımlı anahtar/değer çiftleridir. Düğüm özelliklerine örnek olarak düğümün sabit diskveya grafik kartı olup olmadığı, sabit diskindeki iğ sayısı, çekirdekler ve diğer fiziksel özellikler verilebilir. <br> **Kapasiteler** - Düğüm kapasiteleri, belirli bir düğümün tüketim için mevcut olduğu belirli bir kaynağın adını ve miktarını tanımlar. Örneğin, bir düğüm "MemoryInMb" adlı bir metrik için kapasiteye sahip olduğunu ve varsayılan olarak 2048 MB'a sahip olduğunu tanımlayabilir. Bu kapasiteler, belirli miktarda kaynak gerektiren hizmetlerin, bu kaynakları gerekli tutarlarda bulunan düğümlere yerleştirildiğinden emin olmak için çalışma zamanında kullanılır.<br>**IsPrimary** - Birden fazla NodeType tanımlı varsa, sistem hizmetlerinin çalıştığı değer *olan true*değeriyle yalnızca bir tanesinin birincil olarak ayarlandığından emin olun. Diğer tüm düğüm türleri *false* değerine ayarlanmalıdır |
| **Düğümler** |Bunlar, kümenin bir parçası olan düğümlerin her biri için ayrıntılardır (düğüm türü, düğüm adı, IP adresi, hata etki alanı ve düğümün yükseltme etki alanı). Kümenin oluşturulmasını istediğiniz makinelerin IP adresleriyle birlikte burada listelenmeleri gerekir. <br> Tüm düğümler için aynı IP adresini kullanıyorsanız, test amacıyla kullanabileceğiniz tek kutulu bir küme oluşturulur. Üretim iş yüklerini dağıtmak için Tek kutukümeleri kullanmayın. |

Küme yapılandırması tüm ayarları ortama göre yapılandırıldıktan sonra küme ortamına (adım 7) karşı sınanabilir.

<a id="environmentsetup"></a>

## <a name="environment-setup"></a>Ortamı ayarlama

Bir küme yöneticisi hizmet kumaşı bağımsız kümeyi yapılandırdığında, ortamın aşağıdaki ölçütlerle ayarlanması gerekir: <br>
1. Kümeyi oluşturan kullanıcının, küme yapılandırma dosyasında düğüm olarak listelenen tüm makinelerde yönetici düzeyinde güvenlik ayrıcalıkları olmalıdır.
2. Kümenin oluşturulduğu makinenin yanı sıra her küme düğüm makinesinin aşağıdakileri yapması gerekir:
   * Servis Kumaş ı SDK'nın kaldırMı
   * Servis Kumaş Çalışma Zamanı kaldırıldı mı
   * Windows Firewall hizmetini (mpssvc) etkinleştirin
   * Uzaktan Kayıt Defteri Hizmeti (uzak kayıt defteri) etkin
   * Dosya paylaşımı (SMB) etkin
   * Küme yapılandırma bağlantı noktalarını temel alan gerekli bağlantı noktalarının açılması
   * Windows SMB ve Uzaktan Kayıt Defteri hizmeti için gerekli bağlantı noktaları açıldı: 135, 137, 138, 139 ve 445
   * Ağ bağlantısının birbiriyle olması
3. Küme düğüm makinelerinin hiçbiri Etki Alanı Denetleyicisi olmamalıdır.
4. Dağıtılacak küme güvenli bir kümeyse, gerekli güvenlik ön koşullarının yerinde olduğunu ve yapılandırmaya karşı doğru şekilde yapılandırıldığı doğrulayın.
5. Küme makineleri Internet'e erişilemiyorsa, küme yapılandırmasında aşağıdakileri ayarlayın:
   * Devre dışı bırak *telemetri: "enableTelemetry"* kümesi *özellikleri* altında: yanlış
   * Geçerli küme sürümünün desteğin sonuna yaklaştığını & bildirimleri indiren otomatik Kumaş sürümünü devre dışı bırak: *"fabricClusterAutoupgradeEnabled"* ayarlı *özelliklerin* altında: yanlış
   * Alternatif olarak, ağ internet erişimi beyaz listedeki etki alanlarıyla sınırlıysa, aşağıdaki etki alanları otomatik yükseltme için gereklidir: go.microsoft.com download.microsoft.com

6. Uygun Hizmet Kumaş antivirüs hariç tutmaları ayarlayın:

| **Antivirus Dışlanmış dizinler** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (küme yapılandırmasından) |
| FabricLogRoot (küme yapılandırmasından) |

| **Antivirus Dışlanmış işlemler** |
| --- |
| Kumaş.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |

## <a name="validate-environment-using-testconfiguration-script"></a>TestConfiguration komut dosyalarını kullanarak ortamı doğrulama
TestConfiguration.ps1 komut dosyası tek başına paketbulunabilir. Yukarıdaki ölçütlerden bazılarını doğrulamak için En İyi Uygulamalar Çözümleyicisi olarak kullanılır ve kümenin belirli bir ortamda dağıtılıp dağıtılamadığını doğrulamak için akıl sağlığı denetimi olarak kullanılmalıdır. Herhangi bir hata varsa, sorun giderme için [Çevre Kurulumu](service-fabric-cluster-standalone-deployment-preparation.md) altındaki listeye bakın.

Bu komut dosyası, küme yapılandırma dosyasında düğüm olarak listelenen tüm makinelere yönetici erişimi olan herhangi bir makinede çalıştırılabilir. Bu komut dosyasının çalıştırılan makine kümenin bir parçası olmak zorunda değildir.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True
```

Şu anda bu yapılandırma sınayın modülü güvenlik yapılandırmasını doğrulamaz, bu nedenle bu işlem bağımsız olarak yapılmalıdır.

> [!NOTE]
> Bu modülü daha sağlam hale getirmek için sürekli iyileştirmeler yapıyoruz, bu nedenle şu anda TestConfiguration tarafından yakalanmadığına inandığınız hatalı veya eksik bir durum varsa, lütfen [destek kanallarımızdan](https://docs.microsoft.com/azure/service-fabric/service-fabric-support)bize bildirin.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Windows Server üzerinde çalıştırılan bir tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)
