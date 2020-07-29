---
title: Tek başına Service Fabric kümelerine genel bakış
description: Service Fabric kümeler Windows Server ve Linux üzerinde çalışır. Bu, Windows Server veya Linux 'un çalışabileceği her yerde Service Fabric uygulamaları dağıtabilmeniz ve barındırabilmeniz anlamına gelir.
author: dkkapur
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: 6abe6fca77251a16bcb7663a5192f46fef3476b0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85080658"
---
# <a name="overview-of-service-fabric-standalone-clusters"></a>Tek başına Service Fabric kümelerine genel bakış

Service Fabric küme, mikro hizmetlerinizin dağıtıldığı ve yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Bir kümenin parçası olan makineye veya VM 'ye küme düğümü denir. Kümeler, binlerce düğüme ölçeklendirebilir. Kümeye yeni düğümler eklerseniz, hizmet bölümü çoğaltmaları ve örneklerinin artan düğüm sayısı genelinde yeniden dengelenmesi Service Fabric. Genel uygulama performansı, bellek düşüşlerine erişim için gelişir ve çekişmeyi geliştirir. Kümedeki düğümler verimli bir şekilde kullanılmıyorsa, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric, her düğümdeki donanımın daha iyi kullanılmasını sağlamak için bölüm çoğaltmalarını ve örnekleri, azaltılmış düğüm sayısı genelinde yeniden dengeler.

Düğüm türü, kümedeki bir düğüm kümesinin boyutunu, sayısını ve özelliklerini tanımlar. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır. Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır. [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir.

Şirket içinde Service Fabric kümesi oluşturma işlemi, bir VM kümesiyle tercih ettiğiniz herhangi bir bulutta küme oluşturma işlemine benzerdir. VM 'Leri sağlamaya yönelik ilk adımlar, kullanmakta olduğunuz bulut sağlayıcısına veya şirket içi ortama tabidir. Aralarında ağ bağlantısı etkinleştirilmiş bir VM kümesine sahip olduktan sonra, Service Fabric paketini ayarlama, küme ayarlarını düzenleme ve küme oluşturma ve yönetim komut dosyalarını çalıştırma adımları aynıdır. Bu, yeni barındırma ortamlarını hedeflemesini seçtiğinizde Service Fabric kümelerini çalıştırma ve yönetme deneyiminizin ve bilgilerinizin yönetibilmesini sağlar.

## <a name="cluster-security"></a>Küme güvenliği

Service Fabric kümesi sahip olduğunuz bir kaynaktır.  Yetkisiz kullanıcıların bunlara bağlanmasını önlemeye yardımcı olmak için kümelerinizin güvenliğini sağlamak sizin sorumluluğunuzdadır. Küme üzerinde üretim iş yüklerini çalıştırırken güvenli bir küme özellikle önemlidir.

> [!NOTE]
> Windows kimlik doğrulaması, Kerberos 'u temel alır. NTLM, kimlik doğrulama türü olarak desteklenmez.
>
> Mümkün olduğunda, Service Fabric kümeleri için X. 509.440 sertifika kimlik doğrulamasını kullanın.

### <a name="node-to-node-security"></a>Düğümden düğüme güvenlik

Düğümden düğüme güvenlik, bir kümedeki VM 'Ler veya bilgisayarlar arasındaki iletişimin güvenliğini sağlar. Bu güvenlik senaryosu, yalnızca kümeye katılma yetkisi olan bilgisayarların kümedeki uygulamaları ve Hizmetleri barındırmak için katılmasına olanak sağlar. Service Fabric, bir kümeyi güvenli hale getirmek ve uygulama güvenliği özellikleri sağlamak için X. 509.440 sertifikaları kullanır.  Küme trafiğinin güvenliğini sağlamak ve küme ve sunucu kimlik doğrulaması sağlamak için bir küme sertifikası gerekir.  Otomatik olarak imzalanan sertifikalar, test kümeleri için kullanılabilir, ancak üretim kümelerini güvenli hale getirmek için güvenilir bir sertifika yetkilisinden bir sertifika kullanılmalıdır.

Windows güvenliği, Windows tek başına kümesi için de etkinleştirilebilir. Windows Server 2012 R2 ve Windows Active Directory varsa, grup tarafından yönetilen hizmet hesaplarıyla Windows güvenliği kullanmanızı öneririz. Aksi takdirde, Windows Güvenlik ile Windows hesaplarını kullanın.

Daha fazla bilgi için [düğümden düğüme güvenliği](service-fabric-cluster-security.md#node-to-node-security) okuyun

### <a name="client-to-node-security"></a>İstemciden düğüme güvenlik

İstemciden düğüme güvenlik, istemcilerin kimliğini doğrular ve kümedeki istemci ve tek düğümler arasındaki iletişimin güvenliğini sağlamaya yardımcı olur. Bu tür bir güvenlik, kümeye ve kümeye dağıtılan uygulamalara yalnızca yetkili kullanıcıların erişebildiğinden emin olmanıza yardımcı olur. İstemciler, X. 509.440 sertifika güvenlik kimlik bilgileri aracılığıyla benzersiz şekilde tanımlanır. Küme ile yönetici veya Kullanıcı istemcilerinin kimliğini doğrulamak için herhangi bir sayıda isteğe bağlı istemci sertifikası kullanılabilir.

İstemci sertifikalarına ek olarak, Azure Active Directory, küme ile istemcilerin kimliğini doğrulamak için de yapılandırılabilir.

Daha fazla bilgi için [istemciden düğüme güvenliği](service-fabric-cluster-security.md#client-to-node-security) okuyun

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Service Fabric Ayrıca, farklı Kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamak üzere erişim denetimini destekler. Bu, kümenin daha güvenli olmasına yardımcı olur. Bir kümeye bağlanan istemciler için iki erişim denetimi türü desteklenir: yönetici rolü ve Kullanıcı rolü.  

Daha fazla bilgi için [rol tabanlı Access Control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)okuyun.

## <a name="scaling"></a>Ölçeklendirme

Uygulama taleplerine zaman içinde değişiklik yapılır. Daha fazla uygulama iş yükünü veya ağ trafiğini karşılamak için küme kaynaklarını artırmanız veya talep düştüğünde küme kaynaklarını azaltmanız gerekebilir. Service Fabric kümesi oluşturduktan sonra, kümeyi yatay olarak ölçeklendirebilirsiniz (düğüm sayısını değiştirebilir) veya dikey (düğümlerin kaynaklarını değiştirebilirsiniz). Küme üzerinde iş yükleri çalışırken bile kümeyi istediğiniz zaman ölçeklendirebilirsiniz. Küme ölçeklenirken uygulamalarınız da otomatik olarak ölçeklendirilir.

Daha fazla bilgi için [tek başına kümeleri ölçeklendirin](service-fabric-cluster-scaling-standalone.md).

## <a name="upgrading"></a>Yükseltmenin

Tek başına küme, tamamen sahip olduğunuz bir kaynaktır. Temel alınan işletim sisteminde düzeltme eki uygulama ve doku yükseltmeleri başlatma konusunda siz sorumlusunuz. Kümenizi otomatik çalışma zamanı yükseltmeleri alacak şekilde ayarlayabilir, Microsoft yeni bir sürüm yayınlar veya istediğiniz desteklenen bir çalışma zamanı sürümünü seçmenizi sağlar. Doku yükseltmelerine ek olarak, işletim sistemini de değiştirebilir ve sertifikalar ya da uygulama bağlantı noktaları gibi küme yapılandırmasını güncelleştirebilirsiniz. 

Daha fazla bilgi için [tek başına kümeleri yükseltme](service-fabric-cluster-upgrade-standalone.md)makalesini okuyun.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Bu işletim sistemlerini çalıştıran VM 'lerde veya bilgisayarlarda kümeler oluşturabilirsiniz (Linux henüz desteklenmemektedir):

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 2019

## <a name="next-steps"></a>Sonraki adımlar

Tek başına kümelerin [güvenliğini sağlama](service-fabric-cluster-security.md), [ölçeklendirme](service-fabric-cluster-scaling-standalone.md)ve [yükseltme](service-fabric-cluster-upgrade-standalone.md) hakkında daha fazla bilgi edinin.

[Service Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.
