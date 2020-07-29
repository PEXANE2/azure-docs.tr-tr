---
title: Azure Service Fabric mimarisi
description: Bu makalede, bulut için ölçeklenebilir, güvenilir ve kolayca yönetilen uygulamalar oluşturmak için kullanılan dağıtılmış bir sistem platformu olan Service Fabric mimarisi açıklanmaktadır.
services: service-fabric
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 972700dded1841994de9252b4aa4bbc8eaefeaf8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76024698"
---
# <a name="service-fabric-architecture"></a>Service Fabric mimarisi

Service Fabric katmanlı alt sistemler ile oluşturulmuştur. Bu alt sistemler aşağıdaki uygulamaları yazmanızı sağlar:

* Yüksek oranda kullanılabilir
* Ölçeklenebilir
* Kolay
* Test edilebilir

Aşağıdaki diyagramda Service Fabric ana alt sistemleri gösterilmektedir.

![Service Fabric mimarisinin diyagramı](media/service-fabric-architecture/service-fabric-architecture.png)

Dağıtılmış bir sistemde, bir kümedeki düğümler arasında güvenli bir şekilde iletişim kurma olanağı çok önemlidir. Yığının tabanında, düğümler arasında güvenli iletişim sağlayan aktarım alt sistemidir. Aktarım alt sisteminin üzerinde, farklı düğümleri tek bir varlığa (kümeler olarak adlandırılır) oluşturan Federasyon alt sistemi, Service Fabric sorunları tespit edebilir, öncü seçim gerçekleştirebilir ve tutarlı yönlendirme sağlayabilir. Federasyon alt sisteminin üst kısmında katmanlanan güvenilirlik alt sistemi, çoğaltma, kaynak yönetimi ve yük devretme gibi mekanizmalar aracılığıyla Service Fabric hizmetlerinin güvenmesinden sorumludur. Federasyon alt sistemi, tek bir düğümdeki uygulamanın yaşam döngüsünü yöneten barındırma ve etkinleştirme alt sistemini de temel alır. Yönetim alt sistemi, uygulama ve hizmetlerin yaşam döngüsünü yönetir. Test etme alt sistemi, uygulama geliştiricilerinin uygulamaları ve Hizmetleri üretim ortamlarına dağıtmadan önce ve sonrasında kendilerini sanal hatalara karşı test etmesine yardımcı olur. Service Fabric, iletişim alt sistemi aracılığıyla hizmet konumlarını çözme olanağı sağlar. Geliştiricilere sunulan uygulama programlama modelleri, araçları etkinleştirmek için uygulama modeliyle birlikte bu alt sistemlerin üst kısmında katmanlıdır.

## <a name="transport-subsystem"></a>Taşıma alt sistemi

Taşıma alt sistemi, noktadan noktaya bir veri birimi iletişim kanalı uygular. Bu kanal Service Fabric kümeleri içindeki iletişim ve Service Fabric kümesi ve istemcileri arasındaki iletişim için kullanılır. Tek yönlü ve istek-yanıt iletişim desenlerini destekler, bu da Federasyon katmanında yayın ve çok noktaya yayın uygulama temelini sağlar. Taşıma alt sistemi, x509 sertifikalarını veya Windows güvenliğini kullanarak iletişimin güvenliğini sağlar. Bu alt sistem, Service Fabric tarafından dahili olarak kullanılır ve uygulama programlama için geliştiricilere doğrudan erişilemez.

## <a name="federation-subsystem"></a>Federasyon alt sistemi

Dağıtılmış bir sistemdeki bir düğüm kümesiyle ilgili nedenlerle, sistemin tutarlı bir görünümüne sahip olmanız gerekir. Federasyon alt sistemi, aktarım alt sistemi tarafından sunulan iletişim temel öğelerini kullanır ve çeşitli düğümleri, bunun nedeni olarak tek bir Birleşik kümede birleştirir. Diğer alt sistemler, hata algılama, öncü seçim ve tutarlı yönlendirme için gereken dağıtılmış sistem temelleri sağlar. Federasyon alt sistemi, 128 bit belirteç alanı olan dağıtılmış karma tablolarının üzerine kurulmuştur. Alt sistem düğümler üzerinde bir halka topolojisi oluşturur ve halkadaki her düğüm, sahiplik için belirteç alanının bir alt kümesini ayırmaktadır. Hata algılama için, katman kalp ve yönetim temelinde bir kiralama mekanizması kullanır. Federasyon alt sistemi Ayrıca, herhangi bir anda yalnızca tek bir sahip belirtecin mevcut olduğu karmaşık birleştirme ve ayrılma protokollerinden de garanti eder. Bu, öncü seçim ve tutarlı yönlendirme garantisi sağlar.

## <a name="reliability-subsystem"></a>Güvenilirlik alt sistemi

Güvenilirlik alt sistemi, *Replicator*, *Yük Devretme Yöneticisi*ve *kaynak dengeleyici*kullanımı aracılığıyla bir Service Fabric hizmetinin durumunu yüksek oranda kullanılabilir hale getirme mekanizmasını sağlar.

* Çoğaltıcı, birincil hizmet çoğaltmasında durum değişikliklerinin otomatik olarak ikincil çoğaltmalara çoğaltılmasını sağlar ve bir hizmet çoğaltma kümesindeki birincil ve ikincil çoğaltmalar arasındaki tutarlılığı sağlar. Çoğaltıcı, çoğaltma kümesindeki çoğaltmalar arasında çekirdek yönetiminden sorumludur. Çoğaltılacak işlemlerin listesini almak için yük devretme birimiyle etkileşime girer ve yeniden yapılandırma Aracısı bunu çoğaltma kümesinin yapılandırması ile birlikte sağlar. Bu yapılandırma, işlemlerin hangi çoğaltmaları çoğaltılacağını gösterir. Service Fabric, hizmet durumunu yüksek oranda kullanılabilir ve güvenilir hale getirmek için programlama modeli API 'SI tarafından kullanılabilen yapı çoğaltıcı adlı varsayılan bir çoğaltıcı sağlar.
* Yük Devretme Yöneticisi, kümeye düğümler eklendiğinde veya kümeden kaldırıldığında, yük otomatik olarak kullanılabilir düğümler arasında yeniden dağıtılır. Kümedeki bir düğüm başarısız olursa, küme, kullanılabilirliği sürdürmek için hizmet çoğaltmalarını otomatik olarak yeniden yapılandırır.
* Kaynak Yöneticisi, hizmet çoğaltmalarını kümedeki hata etki alanları arasında koyar ve tüm yük devretme birimlerinin çalışır durumda olmasını sağlar. Kaynak Yöneticisi ayrıca, en uygun Tekdüzen yük dağıtımına ulaşmak için temel alınan paylaşılan küme düğümleri genelinde hizmet kaynaklarını dengeler.

## <a name="management-subsystem"></a>Yönetim alt sistemi

Yönetim alt sistemi uçtan uca hizmet ve uygulama yaşam döngüsü yönetimi sağlar. PowerShell cmdlet 'leri ve yönetim API 'Leri, kullanılabilirlik kaybı olmadan uygulamaları sağlamanıza, dağıtmanıza, düzeltme ekine, yükseltmenize ve serbest bir şekilde sağlamanıza olanak tanır. Yönetim alt sistemi bunu aşağıdaki hizmetler aracılığıyla gerçekleştirir.

* **Küme Yöneticisi**: Bu, uygulamaları, hizmet yerleştirme kısıtlamalarına göre düğümlere yerleştirmek için güvenilirlikten yük devretme Yöneticisi etkileşim kuran birincil hizmettir. Yük devretme alt sistemindeki Kaynak Yöneticisi kısıtlamaların hiçbir şekilde bozulmamasını sağlar. Küme Yöneticisi, sağlama/sağlama için uygulamaların yaşam döngüsünü yönetir. Yükseltmeler sırasında uygulama kullanılabilirliğinin anlamsal bir durum perspektifinden kaybolmamasını sağlamak için sistem durumu yöneticisiyle tümleştirilir.
* **Health Manager**: Bu hizmet, uygulamalar, hizmetler ve küme varlıklarının sistem durumunu izlemeye izin vermez. Küme varlıkları (düğümler, hizmet bölümleri ve çoğaltmalar gibi), daha sonra merkezi sistem durumu deposuna toplanmış olan sistem durumu bilgilerini raporlayabilir. Bu sistem durumu bilgileri, kümedeki birden çok düğüme dağıtılmış hizmetlerin ve düğümlerin genel bir zaman noktası anlık görüntüsü sağlar ve bu sayede gerekli düzeltici eylemler gerçekleştirebilirsiniz. Sistem durumu sorgusu API 'Leri, sistem durumu alt sistemine bildirilen sistem durumu olaylarını sorgulamanızı sağlar. Sistem durumu sorgusu API 'Leri, belirli bir küme varlığı için sistem durumu deposunda depolanan ham sistem durumu verilerini veya toplanmış, yorumlanan sistem durumu verilerini döndürür.
* **Görüntü deposu**: Bu hizmet, uygulama ikililerinin depolama ve dağıtımını sağlar. Bu hizmet, uygulamaların sürümünden yüklendiği ve indirildiği basit bir dağıtılmış dosya deposu sağlar.

## <a name="hosting-subsystem"></a>Barındırma alt sistemi

Küme Yöneticisi, belirli bir düğüm için yönetilmesi gereken hizmetleri barındıran alt sistemi (her düğüm üzerinde çalışan) bilgilendirir. Barındırma alt sistemi daha sonra o düğümdeki uygulamanın yaşam döngüsünü yönetir. Çoğaltmaların doğru şekilde yerleştirildiğinden ve sağlıklı olduğundan emin olmak için güvenilirlik ve sistem durumu bileşenleriyle etkileşime girer.

## <a name="communication-subsystem"></a>İletişim alt sistemi

Bu alt sistem, adlandırma hizmeti aracılığıyla küme ve hizmet bulma içinde güvenilir mesajlaşma sağlar. Adlandırma hizmeti, hizmet adlarını kümedeki bir konuma çözümler ve kullanıcıların hizmet adlarını ve özelliklerini yönetmesine olanak sağlar. İstemciler, adlandırma hizmetini kullanarak bir hizmet adını çözümlemek ve hizmet meta verilerini almak için kümedeki herhangi bir düğümle güvenli bir şekilde iletişim kurabilir. Service Fabric kullanıcıları basit bir adlandırma istemci API 'SI kullanarak geçerli ağ konumunu çözebilen hizmet ve istemcileri, Node dynamisd veya kümenin yeniden boyutlandırmasına karşın geliştirebilir.

## <a name="testability-subsystem"></a>Test edilebilirlik alt sistemi

Test, Service Fabric üzerinde oluşturulan Hizmetleri test etmek için özel olarak tasarlanmış bir araç paketidir. Bu araçlar, bir geliştiricinin yaşam süresi boyunca deneyimle karşılaşacağı çok sayıda durumu ve geçişi, tüm denetimli ve güvenli bir şekilde gerçekleştirmesini sağlamak üzere anlamlı hatalara ve test senaryolarını çalıştırmasına olanak tanır. Yük kararlılığı Ayrıca, kullanılabilirliği kaybetmeden çeşitli olası hatalarda yineleyebilir uzun testleri çalıştırmak için bir mekanizma sağlar. Bu, size bir test üretim ortamı sağlar.
