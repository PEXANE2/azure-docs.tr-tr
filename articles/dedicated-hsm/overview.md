---
title: Ayrılmış HSM nedir? - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM'ye genel bakış, Azure'da FIPS 140-2 Düzey 3 sertifikasını karşılayan önemli depolama özellikleri sağlar
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: dd5ce117645ef2b368bbf8f0e441770d6e746b5b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "70881084"
---
# <a name="what-is-azure-dedicated-hsm"></a>Azure Ayrılmış HSM nedir?

Azure Özel HSM, Azure'da şifreleme anahtar depolaması sağlayan bir Azure hizmetidir. Özel HSM en sıkı güvenlik gereksinimlerini karşılar. FIPS 140-2 Seviye 3 onaylı cihazlara ve HSM cihazının tam ve özel kontrolüne ihtiyaç duyan müşteriler için ideal bir çözümdür. 

 HSM aygıtları çeşitli Azure bölgelerinde genel olarak dağıtılır. Bunlar kolayca bir çift aygıt olarak sağlanabilir ve yüksek kullanılabilirlik için yapılandırılabilir. HSM cihazları, bölgesel düzeydeki başarısızlıklara karşı güvence sağlamak için bölgeler arasında da kullanılabilir. Microsoft, Gemalto'nun [SafeNet Luna Network HSM 7 (Model A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) cihazını kullanarak Özel HSM hizmetini sunar. Bu aygıt en yüksek düzeyde performans ve şifreleme tümleştirme seçenekleri sunar. 

Bunlar sağlandıktan sonra, HSM aygıtları doğrudan müşterinin sanal ağına bağlanır. Ayrıca, noktadan siteye veya siteden siteye VPN bağlantısını yapılandırdığınızda şirket içi uygulama ve yönetim araçları yla da erişilebilirler. Müşteriler, Gemalto'nun destek portalından HSM aygıtlarını yapılandırmak ve yönetmek için yazılım ve dokümantasyona başvurur.

## <a name="why-use-azure-dedicated-hsm"></a>Azure Özel HSM'yi neden kullanıyor?

### <a name="fips-140-2-level-3-compliance"></a>FIPS 140-2 Seviye-3 uyumluluğu

Birçok kuruluş, şifreleme anahtar depolamanın [FIPS 140-2 Düzey-3](https://csrc.nist.gov/publications/detail/fips/140/2/final) gereksinimlerini karşıladığını belirleyen sıkı endüstri düzenlemelerine sahiptir. Microsoft'un çok kiracılı Azure Key Vault hizmeti şu anda yalnızca FIPS 140-2 Düzey-2 sertifikası sağlar. Azure Özel HSM, finansal hizmetler sektörü, devlet kurumları ve FIPS 140-2 Düzey-3 gereksinimlerini karşılaması gereken diğerleri için gerçek bir ihtiyacı karşılar.

### <a name="single-tenant-devices"></a>Tek kiracılı aygıtlar

Müşterilerimizin çoğu şifreleme depolama cihazının tek bir kira için bir gereklilik var. Azure Özel HSM hizmeti, Microsoft'un genel olarak dağıtılan veri merkezlerinden birinden fiziksel bir aygıt sağlamalarını sağlar. Bir müşteriye sağlandıktan sonra, aygıta yalnızca o müşteri erişebilir.

### <a name="full-administrative-control"></a>Tam yönetim kontrolü

Birçok müşteri, yönetim amaçları için tam yönetim denetimine ve aygıtlarına tek erişim ekine ihtiyaç duyar. Bir aygıt sağlandıktan sonra, aygıta yalnızca müşteri yönetici veya uygulama düzeyinde erişime sahiptir.

 Müşteri aygıta ilk kez eriştinkten sonra Microsoft'un yönetim denetimi yoktur ve bu noktada müşteri parolayı değiştirir. Bu noktadan itibaren, müşteri tam yönetim kontrolü ve uygulama yönetimi yeteneği ile gerçek bir tek kiracı. Microsoft, seri bağlantı noktası bağlantısı üzerinden telemetri için monitör düzeyinde erişimi (yönetici rolü değil) korur. Bu erişim sıcaklık, güç kaynağı durumu ve fan sağlığı gibi donanım monitörlerini kapsar. 
 
 Müşteri, gerekli olan bu izlemeyi devre dışı kalmaktadır. Ancak, devre dışı kalanları, Microsoft'tan proaktif sistem durumu uyarıları almazlar.

### <a name="high-performance"></a>Yüksek performans

Gemalto cihazı çeşitli nedenlerle bu hizmet için seçildi. Çok çeşitli şifreleme algoritması desteği, çeşitli desteklenen işletim sistemleri ve geniş API desteği sunar. Dağıtılan özel model, RSA-2048 için saniyede 10.000 operasyonla mükemmel performans sunar. Benzersiz uygulama örnekleri için kullanılabilecek 10 bölümü destekler. Bu aygıt düşük gecikme, yüksek kapasite ve yüksek iş elde etme aygıtıdır.

### <a name="unique-cloud-based-offering"></a>Benzersiz bulut tabanlı teklif

Microsoft, benzersiz bir müşteri kümesi için belirli bir gereksinimi tanıdı. Yeni müşterilere FIPS 140-2 Düzey 3 onaylı özel bir HSM hizmeti sunan ve bulut tabanlı ve şirket içi uygulama entegrasyonu sunan tek bulut sağlayıcısıdır.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Azure Özel HSM sizin için uygun mu?

Azure Özel HSM, belirli bir büyük ölçekli kuruluş türü için benzersiz gereksinimleri karşılayan özel bir hizmettir. Sonuç olarak, Azure müşterilerinin büyük bir kısmının bu hizmetin kullanım profiline uymaması beklenmektedir. Pek çok kişi Azure Key Vault hizmetini daha uygun ve uygun maliyetli bulur. Gereksinimlerinize uygun olup olmadığına karar vermenize yardımcı olmak için aşağıdaki kriterleri belirledik.

### <a name="best-fit"></a>En uygun

Azure Özel HSM, HSM aygıtlarına doğrudan ve tek erişim gerektiren "kaldırma ve kaydırma" senaryoları için en uygundur. Örneklere şunlar dahildir:

- Uygulamaları şirket içinden Azure Sanal Makineleri'ne geçirme
- AWS Cloud HSM Classic hizmetini kullanan sanal makinelere Amazon AWS EC2'den uygulamaları geçirmek (Amazon bu hizmeti yeni müşterilere sunmuyor)
- Azure Sanal Makinelerde Apache/Ngnix SSL Boşaltma, Oracle TDE ve ADCS gibi küçültülmüş yazılımları çalıştırma 

### <a name="not-a-fit"></a>Uyum değil.

Azure Özel HSM, aşağıdaki senaryo türü için uygun değildir: Müşteri tarafından yönetilen anahtarlarla şifrelemeyi destekleyen Microsoft bulut hizmetleri (Azure Bilgi Koruması, Azure Disk Şifrelemesi, Azure Veri Gölü Deposu, Azure Depolama, Azure SQL gibi Azure Özel HSM ile tümleşik olmayan Veritabanı ve Office 365 için Müşteri Anahtarı.

### <a name="it-depends"></a>Duruma göre değişir

Azure Özel HSM'nin sizin için çalışıp çalışmayacağı, yapabileceğiniz veya yapamayacağınız gereksinimlerin ve uzlaşmaların karmaşık olabilecek bir karışımına bağlıdır. Bir örnek FIPS 140-2 Düzey 3 gereksinimidir. Bu gereksinim yaygındır ve Özel HSM şu anda bu gereksinimi karşılamak için tek seçenektir. Bu zorunlu gereksinimler alakalı değilse, genellikle Azure Anahtar Kasası ve Özel HSM arasında bir seçimdir. Karar vermeden önce gereksinimlerinizi değerlendirin.

Seçeneklerinizi tartmanız gereken durumlar şunlardır: 

- Müşterinin Azure sanal makinesinde çalışan yeni kod
- Azure sanal makinede SQL Server TDE
- Azure Depolama istemci tarafı şifrelemesi
- SQL Server ve Azure SQL DB Her Zaman Şifrelenir

## <a name="next-steps"></a>Sonraki adımlar

Bu son derece özel bir hizmettir. Bu nedenle, fiyatlandırma, destek ve hizmet düzeyi anlaşmaları da dahil olmak üzere bu belge kümesindeki temel kavramları tam olarak anlamanızı öneririz. 

[Gemalto tümleştirme kılavuzları,](https://safenet.gemalto.com/partners/microsoft/) HSM'lerin varolan bir sanal ağ ortamına sağlanmasını kolaylaştırmanıza yardımcı olur. Dağıtım mimarinizi nasıl ayarlayabileceğinizi belirlemenize yardımcı olacak nasıl yapılır kılavuzları da vardır.

* [Yüksek kullanılabilirlik](high-availability.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
