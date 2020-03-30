---
title: Azure Hizmet Kumaşı Mimarisi
description: Bu makalede, bulut için ölçeklenebilir, güvenilir ve kolay yönetilen uygulamalar oluşturmak için kullanılan dağıtılmış bir sistem platformu olan Service Fabric mimarisi açıklanmaktadır.
services: service-fabric
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 972700dded1841994de9252b4aa4bbc8eaefeaf8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76024698"
---
# <a name="service-fabric-architecture"></a>Service Fabric mimarisi

Service Fabric katmanlı alt sistemlerle üretilmiştir. Bu alt sistemler aşağıdaki uygulamaları yazmanızı sağlar:

* Yüksek oranda kullanılabilir
* Ölçeklenebilir
* Yönetilebilir
* Sınanabilir

Aşağıdaki diyagram, Service Fabric'in ana alt sistemlerini gösterir.

![Hizmet Kumaş mimarisi diyagramı](media/service-fabric-architecture/service-fabric-architecture.png)

Dağıtılmış bir sistemde, kümedeki düğümler arasında güvenli bir şekilde iletişim kurma yeteneği çok önemlidir. Yığının tabanında düğümler arasında güvenli iletişim sağlayan taşıma alt sistemi vardır. Aktarım alt sisteminin üzerinde, Hizmet Dokusu'nun hataları algılayabilmesi, lider seçimini gerçekleştirebilmesi ve tutarlı yönlendirme sağlayabilmeleri için farklı düğümleri tek bir varlığa (adlandırılmış kümeler) kümeleyen federasyon alt sistemi yer alır. Federasyon alt sisteminin üzerine katmanlı güvenilirlik alt sistemi, çoğaltma, kaynak yönetimi ve başarısızlık gibi mekanizmalar aracılığıyla Service Fabric hizmetlerinin güvenilirliğinden sorumludur. Federasyon alt sistemi, tek bir düğüm üzerinde bir uygulamanın yaşam döngüsünü yöneten barındırma ve etkinleştirme alt sisteminin de temelini de alır. Yönetim alt sistemi, uygulamaların ve hizmetlerin yaşam döngüsünü yönetir. Sınama alt sistemi, uygulama geliştiricilerin uygulamaları ve hizmetleri üretim ortamlarına dağıtmadan önce ve sonra simüle edilmiş hatalar aracılığıyla hizmetlerini sınamalarına yardımcı olur. Service Fabric, iletişim alt sistemi ile servis konumlarını çözümleme olanağı sağlar. Geliştiricilere maruz kalan uygulama programlama modelleri, takım lamayı etkinleştirmek için uygulama modeliyle birlikte bu alt sistemlerin üzerine katmanlanır.

## <a name="transport-subsystem"></a>Taşıma alt sistemi

Aktarım alt sistemi noktadan noktaya bir datagram iletişim kanalı uygular. Bu kanal, hizmet kumaş kümeleri içinde iletişim ve hizmet kumaş küme ve istemciler arasındaki iletişim için kullanılır. Federasyon katmanında yayın ve çok noktaya yayın uygulanmasıiçin temel oluşturan tek yönlü ve istek-yanıt iletişim kalıplarını destekler. Aktarım alt sistemi, X509 sertifikalarını veya Windows güvenliğini kullanarak iletişimi güvence altına alar. Bu alt sistem Service Fabric tarafından dahili olarak kullanılır ve uygulama programlama için geliştiriciler tarafından doğrudan erişilebilir değildir.

## <a name="federation-subsystem"></a>Federasyon alt sistemi

Dağıtılmış bir sistemdeki düğüm kümesini ikna etmek için sistemin tutarlı bir görünümüne sahip olmanız gerekir. Federasyon alt sistemi, taşıma alt sisteminin sağladığı iletişim ilkellerini kullanır ve çeşitli düğümleri, neden olabileceği tek bir birleşik kümeye diker. Diğer alt sistemlerin ihtiyaç duyduğu dağıtılmış sistemleri sağlar - başarısızlık algılama, lider seçimi ve tutarlı yönlendirme. Federasyon alt sistemi, 128 bit belirteç alanı olan dağıtılmış karma tabloların üzerine inşa edilmiştir. Alt sistem düğümler üzerinde bir halka topolojisi oluşturur ve halkadaki her düğüm sahiplik için belirteç alanının bir alt kümesi olarak ayrılır. Başarısızlık algılama için, katman kalp atışı ve tahkim dayalı bir kiralama mekanizması kullanır. Federasyon alt sistemi ayrıca, herhangi bir zamanda yalnızca tek bir belirteç sahibinin var olduğunu karmaşık birleştirme ve ayrılma protokolleri ile garanti eder. Bu lider seçimi ve tutarlı yönlendirme garantileri sağlar.

## <a name="reliability-subsystem"></a>Güvenilirlik alt sistemi

Güvenilirlik alt sistemi *Çoğaltıcı*, *Failover Manager*ve *Kaynak Dengeleyici*kullanımı ile bir Hizmet Kumaş hizmet durumunu son derece kullanılabilir hale getirmek için mekanizma sağlar.

* Çoğalıcı, birincil hizmet yinelemesindeki durum değişikliklerinin otomatik olarak ikincil yinelemelere çoğaltılmasını sağlar ve bir hizmet çoğaltma kümesindeki birincil ve ikincil yinelemeler arasında tutarlılık sağlar. Çoğalıcı, yineleme kümesindeki yinelemeler arasında çoğunluk yönetiminden sorumludur. İşlemlistesini çoğaltmak için başarısız birim ile etkileşime girmektedir ve yeniden yapılandırma aracısı bunu yineleme kümesinin yapılandırmasını sağlar. Bu yapılandırma, işlemlerin çoğaltılması gereken yinelemeleri gösterir. Service Fabric, programlama modeli API tarafından hizmet durumunu son derece kullanılabilir ve güvenilir hale getirmek için kullanılabilen Fabric Replicator adında varsayılan bir çoğaltıcı sağlar.
* Failover Manager, düğümler kümeye eklendiğinde veya kümeden kaldırıldığında yükün kullanılabilir düğümler arasında otomatik olarak yeniden dağıtılmasını sağlar. Kümedeki bir düğüm başarısız olursa, küme kullanılabilirliği korumak için hizmet yinelemelerini otomatik olarak yeniden yapılandıracaktır.
* Kaynak Yöneticisi, kümedeki hata etki alanları arasında hizmet yinelemelerini yerleştirir ve tüm başarısız birimlerin çalışır durumda olmasını sağlar. Kaynak Yöneticisi, en iyi düzgün yük dağılımını elde etmek için altta yatan ortak küme düğümleri havuzunda hizmet kaynaklarını da dengeler.

## <a name="management-subsystem"></a>Yönetim alt sistemi

Yönetim alt sistemi uçten uca hizmet ve uygulama yaşam döngüsü yönetimi sağlar. PowerShell cmdlets ve idari API'ler, kullanılabilirlik kaybı olmadan uygulamaları sağlamanızı, dağıtmanızı, yamanıza, yükseltmenize ve de-provision uygulamalarınızı sağlamanıza olanak tanır. Yönetim alt sistemi bunu aşağıdaki hizmetler aracılığıyla gerçekleştirir.

* **Küme Yöneticisi**: Hata Yöneticisi ile güvenilirlikten, uygulamaları hizmet yerleştirme kısıtlamalarına göre düğümlere yerleştirmek için etkileşimde bulunan birincil hizmettir. Başarısız alt sistemdeki Kaynak Yöneticisi, kısıtlamaların hiçbir zaman kırılmamasını sağlar. Küme yöneticisi, uygulamanın kullanım ömründen de-provision'a kadar yönetir. Yükseltmeler sırasında uygulama kullanılabilirliğinin semantik sağlık açısından kaybolmadığından emin olmak için sistem durumu yöneticisiyle bütünleşir.
* **Sağlık Yöneticisi**: Bu hizmet, uygulamaların, hizmetlerin ve küme varlıklarının sistem durumu izlemesini sağlar. Küme varlıkları (düğümler, hizmet bölümleri ve yinelemeler gibi) sistem durumu bilgilerini bildirebilir ve bu bilgiler merkezi sistem durumu deposuna toplanır. Bu sistem durumu bilgileri, kümedeki birden çok düğüme dağıtılan hizmetlerin ve düğümlerin genel bir zaman içinde sağlık anlık görüntüsünü sağlayarak gerekli düzeltici eylemleri yapmanızı sağlar. Sistem durumu sorgusu API'leri, sistem alt sistemine bildirilen sistem durumu olaylarını sorgulamanızı sağlar. Sistem durumu sorgusu API'leri, sistem durumu deposunda depolanan ham sistem durumu verilerini veya belirli bir küme varlığı için toplu, yorumlanmış sistem durumu verilerini döndürer.
* **Resim Deposu**: Bu hizmet, uygulama ikililerinin depolanmasını ve dağıtımını sağlar. Bu hizmet, uygulamaların yüklendiği ve indirildiği basit bir dağıtılmış dosya deposu sağlar.

## <a name="hosting-subsystem"></a>Hosting alt sistemi

Küme yöneticisi, belirli bir düğüm için yönetmesi gereken barındırma alt sistemini (her düğümüzerinde çalışan) bildirir. Barındırma alt sistemi daha sonra bu düğüm üzerinde uygulamanın yaşam döngüsünü yönetir. Kopyaların düzgün bir şekilde yerleştirildiğinden ve sağlıklı olduğundan emin olmak için güvenilirlik ve sistem durumu bileşenleriyle etkileşime geçer.

## <a name="communication-subsystem"></a>İletişim alt sistemi

Bu alt sistem, Adlandırma hizmeti aracılığıyla küme içinde güvenilir ileti ve hizmet bulma sağlar. Adlandırma hizmeti, hizmet adlarını kümedeki bir konuma giderir ve kullanıcıların hizmet adlarını ve özelliklerini yönetmesini sağlar. Adlandırma hizmetini kullanarak, istemciler bir hizmet adını çözmek ve hizmet meta verilerini almak için kümedeki herhangi bir düğümle güvenli bir şekilde iletişim kurabilir. Basit bir Adlandırma istemcisi API'si kullanarak, Service Fabric kullanıcıları düğüm dinamizmine veya kümenin yeniden boyutlandırılmasına rağmen geçerli ağ konumunu çözebilen hizmetler ve istemciler geliştirebilir.

## <a name="testability-subsystem"></a>Test edilebilirlik alt sistemi

Test edilebilirlik, Service Fabric üzerine inşa edilmiş hizmetleri test etmek için özel olarak tasarlanmış bir araç paketidir. Araçlar, bir geliştiricinin, bir hizmetin ömrü boyunca, tümü kontrollü ve güvenli bir şekilde, yaşam boyu yaşayacağı çok sayıda durumu ve geçişi uygulamak ve uygulamak için anlamlı hatalara kolayca neden olmasını ve test senaryolarını çalıştırmasına izin verir. Sınama özelliği, kullanılabilirliğini kaybetmeden çeşitli olası hatalar dan yinelenebilen daha uzun testler çalıştırmak için de bir mekanizma sağlar. Bu, üretimde bir test ortamı sağlar.
