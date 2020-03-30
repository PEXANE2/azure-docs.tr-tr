---
title: Mimariye genel bakış - Azure Active Directory | Microsoft Dokümanlar
description: Azure Etkin Dizin ideninin ne olduğunu ve Azure Active Directory'yi kullanarak Azure'u nasıl yönetirize edin.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 854fb4649f8c1113f20abe5807dd0ce473ba6ee3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368072"
---
# <a name="what-is-the-azure-active-directory-architecture"></a>Azure Active Directory mimarisi nedir?

Azure Active Directory (Azure AD), kullanıcılarınız için Azure hizmet ve kaynaklarına erişimi güvenli bir şekilde yönetmenizi sağlar. Azure AD ile birlikte eksiksiz kimlik yönetimi olanakları sunulur. Azure AD özellikleri hakkında daha fazla bilgi için bkz. [Azure Active Directory nedir?](active-directory-whatis.md)

Azure AD ile kullanıcı ve gruplar oluşturup bunları yönetebilir, ayrıca kurumsal kaynaklara erişim izni vermek ya da erişimi reddetmek için izinleri etkinleştirebilirsiniz. Kimlik yönetimi hakkında bilgi için bkz. [Azure kimlik yönetimi ile ilgili temel bilgiler](active-directory-whatis.md).

## <a name="azure-ad-architecture"></a>Azure AD mimarisi

Azure AD'nin coğrafi olarak dağıtılmış mimarisi, müşterilere şirket genelinde kullanılabilirlik ve performans sunan kapsamlı izleme, otomatik yeniden yönlendirme, geri ödeme ve kurtarma özelliklerini bir araya getirir.

Bu makalede aşağıdaki mimari öğeler ele alınmaktadır:

*   Hizmet mimarisi tasarımı
*   Ölçeklenebilirlik
*   Sürekli kullanılabilirlik
*   Veri merkezleri

### <a name="service-architecture-design"></a>Hizmet mimarisi tasarımı

Erişilebilir ve kullanılabilir, veri açısından zengin bir sistem oluşturmanın en yaygın yolu bağımsız yapı taşları veya ölçek birimleridir. Azure AD veri katmanı için ölçek birimleri *bölüm*olarak adlandırılır.

Veri katmanında, okuma-yazma özelliği sağlayan çok sayıda ön uç hizmeti bulunur. Aşağıdaki diyagram, tek dizinli bir bölümün bileşenlerinin coğrafi olarak dağıtılmış veri merkezleri boyunca nasıl teslim edilir olduğunu gösterir.

  ![Tek dizin li bölüm diyagramı](./media/active-directory-architecture/active-directory-architecture.png)

Azure AD mimarisinin bileşenleri, birincil çoğaltma ve ikincil çoğaltma öğelerini içerir.

#### <a name="primary-replica"></a>Birincil çoğaltma

*Birincil çoğaltma*, ait olduğu bölüm için *yazma* işlemlerini alır. Her yazma işlemi, çağırana başarılı sonucu döndürmeden önce hemen farklı bir veri merkezindeki ikincil çoğaltmaya çoğaltılır, böylece yazma işlemlerinin coğrafi olarak yedekli dayanıklılığı sağlanır.

#### <a name="secondary-replicas"></a>İkincil çoğaltmalar

Tüm dizin *okumaları,* fiziksel olarak farklı coğrafyalarda bulunan veri merkezlerinde bulunan *ikincil yinelemelerden*servis edilir. Veriler zaman uyumsuz olarak kopyalandığı için çok sayıda ikincil çoğaltma vardır. Kimlik doğrulama istekleri gibi dizin okumaları, müşterilere yakın veri merkezlerinden servis edilir. İkincil çoğaltmalar, okuma ölçeklenebilirliğinden sorumludur.

### <a name="scalability"></a>Ölçeklenebilirlik

Ölçeklenebilirlik, bir hizmetin artan performans taleplerine göre genişleyebilme becerisidir. Yazma ölçeklenebilirliği, veri bölümlendirilerek sağlanır. Okuma ölçeklenebilirliği, bir bölümden dünyanın dört bir yanına dağıtılmış birden fazla ikincil çoğaltmaya verilerin çoğaltılması yoluyla sağlanır.

Dizin uygulamalarından gelen istekler fiziksel olarak en yakın oldukları veri merkezine yönlendirilir. Yazma işlemleri, okuma-yazma tutarlılığı sağlamak üzere şeffaf bir şekilde birincil çoğaltmaya yönlendirilir. Dizinler çoğu zaman okuma işlemlerini sunduğundan, ikincil çoğaltmalar bölümlerin ölçeğini önemli ölçüde genişletir.

Dizin uygulamaları en yakın veri merkezlerine bağlanır. Bu bağlantı performansı artırır ve bu nedenle ölçekleme mümkündür. Bir dizin bölümünde çok sayıda ikincil çoğaltma olabileceğinden, ikincil çoğaltmalar dizin istemcilerine daha yakın yerleştirilebilir. Yalnızca yazma yoğunluklu olan iç dizin hizmeti bileşenleri, etkin birincil çoğaltmayı doğrudan hedefler.

### <a name="continuous-availability"></a>Sürekli kullanılabilirlik

Kullanılabilirlik (veya çalışma süresi) bir sistemin kesintisiz çalışma yeteneğini tanımlar. Azure AD'nin yüksek kullanılabilirliğinin anahtarı, hizmetlerin coğrafi olarak dağıtılmış birden çok veri merkezinde trafiği hızla kaydırabilmesidir. Her veri merkezi bağımsızdır ve bu da ilişkilendirme den-correlated hata modları sağlar. Azure AD, bu yüksek kullanılabilirlik tasarımı sayesinde bakım etkinlikleri için kapalı kalma süresi gerektirmez.

Azure AD'nin bölüm tasarımı, dikkatle düzenlenmiş ve deterministic birincil çoğaltma başarısızlık işlemini içeren tek kalıplı bir tasarım kullanılarak kurumsal AD tasarımına göre basitleştirilir.

#### <a name="fault-tolerance"></a>Hataya dayanıklılık

Bir sistem donanım, ağ ve yazılım hatalarına dayanıklı ise kullanılabilirliği daha yüksektir. Dizin üzerindeki her bölüm için yüksek oranda kullanılabilir bir ana çoğaltma mevcuttur: birincil çoğaltma. Bu çoğaltma üzerinde yalnızca bölüme yazma işlemleri gerçekleştirilir. Bu çoğaltma sürekli olarak ve yakından izlenirken, bir hata algılanması durumunda yazma işlemleri hemen başka bir çoğaltmaya kaydırılabilir (bu çoğaltma yeni birincil çoğaltma olur). Yük devretme sırasında genellikle 1-2 dakikalık yazma kullanılabilirliği kaybı olabilir. Bu süre boyunca okuma kullanılabilirliği etkilenmez.

Okuma işlemleri (yazma işlemlerinden onlarca kat fazladır) yalnızca ikincil çoğaltmalara gider. İkincil çoğaltmalar bir kez etkili olduğundan, okumalar genellikle aynı veri merkezinde bulunan başka bir çoğaltmaya yönlendirilerek belirli bir bölümdeki herhangi bir çoğaltmanın kaybı kolayca telafi edilebilir.

#### <a name="data-durability"></a>Veri dayanıklılığı

Bir yazma, kabul edilmeden önce en az iki veri merkezine bağlıdır. Bu, önce birincil üzerine yazmayı taahhüt ederek ve ardından yazmayı hemen en az bir diğer veri merkezine çoğaltarak gerçekleşir. Bu yazma eylemi, birincil barındırma veri merkezi nin olası bir felaket kaybı veri kaybına neden olmaz sağlar.

Azure AD, başarısız lıklarla ilgili veri kaybetmemek için sıfır [Kurtarma Süresi Hedefi (RTO)](https://en.wikipedia.org/wiki/Recovery_time_objective) tutar. Buna aşağıdakiler dahildir:

* Belirteç verme ve dizin okuma
* Dizin için sadece yaklaşık 5 dakika RTO izin yazıyor

### <a name="datacenters"></a>Veri merkezleri

Azure AD çoğaltmaları, dünyanın dört bir yanında bulunan veri merkezlerinde depolanır. Daha fazla bilgi için Azure [küresel altyapısına](https://azure.microsoft.com/global-infrastructure/)bakın.

Azure AD, veri merkezleri arasında aşağıdaki özelliklere sahip olarak çalışır:

* Kimlik doğrulama, Grafik ve diğer AD hizmetleri Ağ Geçidi hizmetinin arkasında yer alan bir kimlik kimliğidir. Gateway bu hizmetlerin yük dengelemesini yönetir. Herhangi bir sağlıksız sunucuişlem durumu sondaları kullanılarak algılanırsa otomatik olarak üzerinden başarısız olur. Bu sağlık sondalarına dayanarak, Ağ Geçidi trafiği dinamik olarak sağlıklı veri merkezlerine yönlendirir.
* *Okumalar*için, dizin, birden çok veri merkezinde çalışan etkin bir yapılandırmada ikincil yinelemelere ve ilgili ön uç hizmetlerine sahiptir. Tüm veri merkezinin arızalanması durumunda, trafik otomatik olarak farklı bir veri merkezine yönlendirilir.
 **Yazmalar*için, dizin planlanan (yeni birincil eski birincil eşitlenir) veya acil arıza yordamları ile veri merkezleri arasında birincil (ana) çoğaltma üzerinde başarısız olur. Veri dayanıklılığı, herhangi bir adak en az iki veri merkezine çoğaltılması ile elde edilir.

#### <a name="data-consistency"></a>Veri tutarlılığı

Dizin modeli nihai tutarlılıklardan biridir. Dağıtılmış eşzamanlı çoğaltma sistemleriyle ilgili tipik bir sorun, "belirli" bir yinelemeden döndürülen verilerin güncel olmamasıdır. 

Azure AD, yazma işlemlerini birincil çoğaltmaya yönlendirerek ve eşzamanlı olarak ikincil çoğaltmaya geri çekerek, ikincil çoğaltmayı hedefleyen uygulamalar için okuma-yazma tutarlılığı sağlar.

Azure AD'nin Microsoft Graph API'sini kullanarak uygulama yazıları, okuma yazma tutarlılığı için bir dizin yinelemesine yakınlık sağlamaktan soyutlanır. Microsoft Graph API hizmeti, okumalar için kullanılan ikincil yinelemeye yakınlığı olan mantıksal bir oturum tutar; yakınlık, hizmetin ikincil yineleme veri merkezinde dağıtılmış bir önbellek kullanarak önbelleğe aldığı bir "yineleme belirteci" olarak yakalanır. Bu belirteç aynı mantıksal oturumun daha sonraki işlemleri için kullanılır. Aynı mantıksal oturumu kullanmaya devam etmek için, sonraki isteklerin aynı Azure AD veri merkezine yönlendirilmesi gerekir. Dizin istemci istekleri birden çok Azure AD veri merkezine yönlendiriliyorsa mantıksal bir oturuma devam etmek mümkün değildir; bu durumda istemci bağımsız okuma-yazma tutarlılıkları olan birden çok mantıksal oturumları vardır.

 >[!NOTE]
 >Yazma işlemleri, mantıksal oturumdaki okumaların verildiği ikincil çoğaltmaya hemen çoğaltılır.

#### <a name="backup-protection"></a>Yedekleme koruması

Dizin, bir müşterinin yanlışlıkla silmesi durumunda kolay kurtarma amacıyla kullanıcılar ve kiracılar için sabit silme yerine geçici silme gerçekleştirir. Kiracı yöneticiniz kullanıcıları yanlışlıkla silerse, silinen kullanıcıları kolayca geri alabilir ve geri yükleyebilir.

Azure AD tüm verilerin günlük yedeklemesini yapar ve bu nedenle mantıksal silme veya bozulma durumunda verileri yetkili olarak geri yükleyebilir. Veri katmanı, hataları denetleyebilmek ve belirli disk hatalarını otomatik olarak düzeltebilmek için hata düzeltme kodları kullanır.

#### <a name="metrics-and-monitors"></a>Ölçümler ve izleyiciler

Yüksek oranda kullanılabilir bir hizmetin çalıştırılması için birinci sınıf ölçüm ve izleme özellikleri gerekir. Azure AD, temel hizmet durumu ölçümlerini ve her bir hizmetinin başarı ölçütlerini sürekli olarak analiz edip raporlar. Ayrıca, her bir Azure REKLAM hizmetinde ve tüm hizmetlerde ölçümlerin ve izlemenin ve uyarının sürekli geliştirilmesi ve ayarlanması da vardır.

Herhangi bir Azure AD hizmeti beklendiği gibi çalışmıyorsa, işlevselliği mümkün olan en kısa sürede geri yüklemek için hemen eylem yapılır. En önemli metrik Azure AD parçaları, canlı site sorunlarının müşteriler için ne kadar hızlı algılanabileceği ve azaltılabildiğidir. Algılama süresini azaltmak (TTD Hedefi: <5 dakika) için izleme ve uyarılara, önlem alma süresini azaltmak için (TTM Hedefi: <30 dakika) operasyonel hazırlığa önemli ölçüde yatırım yapıyoruz.

#### <a name="secure-operations"></a>Güvenli işlemler

Herhangi bir işlem için çok faktörlü kimlik doğrulama (MFA) gibi operasyonel denetimlerin yanı sıra tüm operasyonların denetlenmesini kullanma. Buna ek olarak, sürekli olarak herhangi bir operasyonel görev-on-demand için gerekli geçici erişim sağlamak için tam zamanında yükseklik sistemi kullanarak. Daha fazla bilgi için bkz. [Güvenilir Bulut](https://azure.microsoft.com/support/trust-center).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory geliştirici kılavuzu](https://docs.microsoft.com/azure/active-directory/develop)