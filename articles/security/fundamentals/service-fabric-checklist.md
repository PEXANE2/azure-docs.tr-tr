---
title: Azure Service Fabric güvenlik denetim listesi | Microsoft Docs
description: Bu makalede, Azure Fabric güvenlik güvenliği için bir dizi denetim listesi sunulmaktadır.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2019
ms.author: tomsh
ms.openlocfilehash: c30b70d2fccb7580dcb94c2322c0ad3a52461f34
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927722"
---
# <a name="azure-service-fabric-security-checklist"></a>Azure Service Fabric güvenlik denetim listesi
Bu makalede, Azure Service Fabric ortamınızı güvenli hale getirmenize yardımcı olacak kullanımı kolay bir denetim listesi sunulmaktadır.

## <a name="introduction"></a>Giriş
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur. Service Fabric ayrıca bulut uygulamalarını geliştirme ve yönetme sürecinde karşılaşılan başlıca sorunların giderilmesini de sağlar. Geliştiriciler ve yöneticiler, karmaşık altyapı sorunlarını çözmeye çalışmak yerine görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilir. Service Fabric, bu iş yüklerinin ölçeklenebilir, güvenilir ve yönetilebilir olmasını sağlar.

## <a name="checklist"></a>Denetim listesi
Güvenli bir Azure Service Fabric çözümünün yönetiminde ve yapılandırmasındaki önemli sorunları gözlediğinizden emin olmanıza yardımcı olması için aşağıdaki denetim listesini kullanın.


|Denetim listesi kategorisi| Açıklama |
| ------------ | -------- |
|[Rol tabanlı erişim denetimi (RBAC)](../../service-fabric/service-fabric-cluster-security-roles.md) | <ul><li>Erişim denetimi, küme yöneticisinin farklı Kullanıcı grupları için belirli küme işlemlerine erişimi sınırlandırıp kümenin daha güvenli olmasını sağlar.</li><li>Yöneticiler, yönetim özelliklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. </li><li> Varsayılan olarak kullanıcılar, yönetim özelliklerine (örneğin, sorgu özellikleri) yalnızca okuma erişimine ve uygulama ve Hizmetleri çözme imkanına sahiptir.</li></ul>|
|[X. 509.440 sertifikaları ve Service Fabric](../../service-fabric/service-fabric-cluster-security.md) | <ul><li>Üretim iş yüklerini çalıştıran kümelerde kullanılan [sertifikaların](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates) , doğru şekilde yapılandırılmış bir Windows Server sertifika hizmeti kullanılarak oluşturulması veya onaylanan bir [SERTIFIKA yetkilisinden (CA)](https://en.wikipedia.org/wiki/Certificate_authority)alınması gerekir.</li><li>Üretimde, [MakeCert. exe](https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx)gibi araçlarla oluşturulan [geçici veya test sertifikalarını](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-create-temporary-certificates-for-use-during-development) hiçbir şekilde kullanmayın. </li><li>Otomatik olarak imzalanan bir [sertifika](../../service-fabric/service-fabric-windows-cluster-x509-security.md) kullanabilirsiniz, ancak bunu yalnızca test kümeleri için ve üretimde değil, yalnızca bu şekilde yapmanız gerekir.</li></ul>|
|[Küme güvenliği](../../service-fabric/service-fabric-cluster-security.md) | <ul><li>Küme güvenliği senaryolarında düğümden düğüme güvenliği, Istemciden düğüme güvenliği, [rol tabanlı erişim denetimi (RBAC)](../../service-fabric/service-fabric-cluster-security-roles.md)bulunur.</li></ul>|
|[Küme kimlik doğrulaması](../../service-fabric/service-fabric-cluster-creation-via-arm.md) | <ul><li>Küme Federasyonu için [düğümden düğüme iletişimin](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-security.md) kimliğini doğrular. </li></ul>|
|[Sunucu kimlik doğrulaması](../../service-fabric/service-fabric-cluster-creation-via-arm.md) | <ul><li>Bir yönetim istemcisinde [küme yönetimi uç noktalarının](../../service-fabric/service-fabric-cluster-creation-via-portal.md) kimliğini doğrular.</li></ul>|
|[Uygulama güvenliği](../../service-fabric/service-fabric-cluster-creation-via-arm.md)| <ul><li>Uygulama yapılandırma değerlerini şifreleme ve şifre çözme.</li><li>   Çoğaltma sırasında düğümler arasında verilerin şifrelenmesi.</li></ul>|
|[Küme sertifikası](../../service-fabric/service-fabric-windows-cluster-x509-security.md) | <ul><li>Bu sertifika, bir kümedeki düğümler arasındaki iletişimin güvenliğini sağlamak için gereklidir.</li><li>    Parmak Izi bölümünde birincil sertifikanın parmak izini ve parmak izi Ikincil değişkenlerinde ikincil değeri ayarlayın.</li></ul>|
|[Sunucusertifikası](../../service-fabric/service-fabric-windows-cluster-x509-security.md)| <ul><li>Bu sertifika, bu kümeye bağlanmayı denediğinde istemciye sunulur. Yükseltme için birincil ve ikincil olmak üzere iki farklı sunucu sertifikası kullanabilirsiniz.</li></ul>|
|Clientcertificateparmak Izleri| <ul><li>Bu, kimliği doğrulanmış istemcilere yüklemek istediğiniz bir sertifika kümesidir. </li></ul>|
|ClientCertificateCommonNames| <ul><li>CertificateCommonName için ilk istemci sertifikasının ortak adını ayarlayın. Certificateıssuerparmak Izi, bu sertifikanın vereninin parmak izine ait. </li></ul>|
|Smarproxycertificate| <ul><li>Bu, [ters proxy](../../service-fabric/service-fabric-reverseproxy.md)'nizin güvenliğini sağlamak istiyorsanız belirtime isteğe bağlı bir sertifikadır. </li></ul>|
|Key Vault| <ul><li>Azure 'da Service Fabric kümelerine yönelik sertifikaları yönetmek için kullanılır.  </li></ul>|


## <a name="next-steps"></a>Sonraki adımlar

- [Service Fabric en iyi güvenlik uygulamaları](service-fabric-best-practices.md)
- [Küme yükseltme işlemini ve beklentilerini Service Fabric](../../service-fabric/service-fabric-cluster-upgrade.md)
- [Service Fabric uygulamalarınızı Visual Studio 'Da yönetme](../../service-fabric/service-fabric-manage-application-in-visual-studio.md).
- [Service Fabric sistem durumu modeli tanıtımı](../../service-fabric/service-fabric-health-introduction.md).
