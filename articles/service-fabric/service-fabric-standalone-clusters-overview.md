---
title: Bağımsız Hizmet Kumaş kümeleri genel bakış
description: Service Fabric kümeleri Windows Server ve Linux'ta çalışır, bu da Service Fabric uygulamalarını Windows Server veya Linux çalıştırabileceğiniz her yerde dağıtabileceğiniz ve barındırabileceğiniz anlamına gelir.
author: dkkapur
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: e8912ef5bc0fd6009443b736031fc9af57ab6c5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465633"
---
# <a name="overview-of-service-fabric-standalone-clusters"></a>Hizmet Kumaş Bağımsız kümelerine Genel Bakış

Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine veya VM'ye küme düğümü denir. Kümeler binlerce düğüme ölçeklenebilir. Kümeye yeni düğümler eklerseniz, Service Fabric artan düğüm sayısı boyunca hizmet bölümü yinelemelerini ve örneklerini yeniden dengeler. Genel uygulama performansı artırır ve belleğe erişim için çekişme azalır. Kümedeki düğümler verimli bir şekilde kullanılıyorsa, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric, her düğümdeki donanımı daha iyi kullanmak için bölüm yinelemelerini ve örnekleri azalan düğüm sayısı boyunca yeniden dengeler.

Düğüm türü kümedeki bir düğüm kümesinin boyutunu, sayısını ve özelliklerini tanımlar. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır. Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır. [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir.

Şirket içinde Bir Hizmet Kumaşı kümesi oluşturma işlemi, seçtiğiniz herhangi bir bulutta bir vm kümesiyle küme oluşturma işlemine benzer. VM'leri sağlamak için atılan ilk adımlar, kullanmakta olduğunuz bulut sağlayıcısı veya şirket içi ortam tarafından yönetilir. Aralarında ağ bağlantısı etkinleştirilmiş bir VM kümesi ne zaman, ardından Service Fabric paketini ayarlamak, küme ayarlarını ayarlamak ve küme oluşturma ve yönetim komut dosyalarını çalıştırmak için adımlar aynıdır. Bu, yeni barındırma ortamlarını hedeflemeyi seçtiğinizde Service Fabric kümelerini işletme ve yönetme bilginizin ve deneyiminizin aktarılabilir olmasını sağlar.

## <a name="cluster-security"></a>Küme güvenliği
Hizmet Kumaşı kümesi, sahip olduğunuz bir kaynaktır.  Yetkisiz kullanıcıların bu kümelere bağlanmasını önlemeye yardımcı olmak için kümelerinizi güvenli hale getirmek sizin sorumluluğunuzdadır. Kümede üretim iş yüklerini çalıştırırken güvenli küme özellikle önemlidir.

### <a name="node-to-node-security"></a>Düğümden düğüme güvenlik
Düğümden düğüme güvenlik, bir kümedeki VM'ler veya bilgisayarlar arasındaki iletişimi güvence altına alar. Bu güvenlik senaryosu, kümeye yalnızca kümeye katılmaya yetkili bilgisayarların kümedeki uygulamaları ve hizmetleri barındırmaya katılmasını sağlar. Service Fabric, bir kümeyi güvenli hale getirmek ve uygulama güvenliği özellikleri sağlamak için X.509 sertifikalarını kullanır.  Küme trafiğini güvence altına almak ve küme ve sunucu kimlik doğrulaması sağlamak için küme sertifikası gereklidir.  Test kümeleri için kendi imzalı sertifikalar kullanılabilir, ancak güvenilir bir sertifika yetkilisinden gelen bir sertifika üretim kümelerini güvence altına almak için kullanılmalıdır.

Windows bağımsız küme için Windows güvenliği de etkinleştirilebilir. Windows Server 2012 R2 ve Windows Active Directory'niz varsa, Windows security'yi yönetilen hizmet hesapları grubuyla kullanmanızı öneririz. Aksi takdirde, Windows hesapları ile Windows güvenlik kullanın.

Daha fazla bilgi için [Düğümden düğüme güvenlik](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>İstemciden düğüme güvenlik
İstemciden düğüme güvenlik istemcilerin kimliğini doğrular ve kümedeki istemci ve tek tek düğümler arasındaki iletişimin güvenli hale ilmesine yardımcı olur. Bu tür güvenlik, kümeye yalnızca yetkili kullanıcıların kümeye ve kümede dağıtılan uygulamalara erişebilmesini sağlamaya yardımcı olur. İstemciler, X.509 sertifika güvenlik kimlik bilgileri yle benzersiz bir şekilde tanımlanır. Herhangi bir sayıda isteğe bağlı istemci sertifikası, yöneticinin veya kümedeki kullanıcı istemcilerinin kimliğini doğrulamak için kullanılabilir.

İstemci sertifikalarına ek olarak, Azure Etkin Dizin kümeile istemcilerin kimliğini doğrulamak için de yapılandırılabilir.

Daha fazla bilgi için [İstemci den düğüme güvenliği](service-fabric-cluster-security.md#client-to-node-security) okuyun

### <a name="role-based-access-control-rbac"></a>Rol Tabanlı Erişim Denetimi (RBAC)
Service Fabric, farklı kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamak için erişim denetimini de destekler. Bu, kümenin daha güvenli hale getirmeye yardımcı olur. Bir kümeye bağlanan istemciler için iki erişim denetimi türü desteklenir: Yönetici rolü ve Kullanıcı rolü.  

Daha fazla bilgi için [Rol Tabanlı Erişim Denetimi 'ni (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)okuyun.

## <a name="scaling"></a>Ölçeklendirme

Uygulama talepleri zaman içinde değişir. Artan uygulama iş yükünü veya ağ trafiğini karşılamak veya talep düştüğünde küme kaynaklarını azaltmak için küme kaynaklarını artırmanız gerekebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, kümeyi yatay olarak ölçeklendirebilir (düğüm sayısını değiştirebilir) veya dikey olarak (düğümlerin kaynaklarını değiştirebilirsiniz). Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz. Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

Daha fazla bilgi için [ölçekleme bağımsız kümeleri](service-fabric-cluster-scaling-standalone.md)okuyun.

## <a name="upgrading"></a>Yükseltme

Bağımsız küme, tamamen sahip olduğunuz bir kaynaktır. Altta yatan işletim sistemi yama ve kumaş yükseltmeleri başlatılması sorumludur. Kümenizi, Microsoft yeni bir sürüm yayımladığında otomatik çalışma zamanı yükseltmeleri alacak şekilde ayarlayabilir veya istediğiniz desteklenen çalışma zamanı sürümünü seçmeyi seçebilirsiniz. Kumaş yükseltmelerine ek olarak, işletim sistemi yama ve sertifikalar veya uygulama bağlantı noktaları gibi küme yapılandırmasını güncelleştirmek. 

Daha fazla bilgi için, [bağımsız kümeleri yükseltme'yi](service-fabric-cluster-upgrade-standalone.md)okuyun.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Bu işletim sistemlerini çalıştıran VM'lerde veya bilgisayarlarda kümeler oluşturabilirsiniz (Linux henüz desteklenmemektedir):

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 2019

## <a name="next-steps"></a>Sonraki adımlar
Bağımsız kümeleri [güvence altına alma,](service-fabric-cluster-security.md) [ölçekleme](service-fabric-cluster-scaling-standalone.md)ve [yükseltme](service-fabric-cluster-upgrade-standalone.md) hakkında daha fazla bilgi edinin.

Service [Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.
