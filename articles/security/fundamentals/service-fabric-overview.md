---
title: Azure Service Fabric güvenliğine genel bakış | Microsoft Docs
description: Bu makalede Azure Service Fabric güvenliğine genel bakış sunulmaktadır.
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
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 5329323122d8470b19055040b6f00b6db7c0e75f
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998941"
---
# <a name="azure-service-fabric-security-overview"></a>Azure Service Fabric güvenliğine genel bakış
[Azure Service Fabric](../../service-fabric/service-fabric-overview.md) , ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştıran bir dağıtılmış sistemler platformudur. Service Fabric, bulut uygulamaları geliştirme ve yönetme sorunlarını giderir. Geliştiriciler ve Yöneticiler, karmaşık altyapı sorunlarından kaçınıp ölçeklenebilir ve güvenilir olan görev açısından kritik, zorlu iş yüklerini uygulamaya odaklanabilirler.

Bu makale, Service Fabric dağıtımına yönelik güvenlik konularına genel bakış.

## <a name="secure-your-cluster"></a>Kümenizin güvenliğini sağlama
Azure Service Fabric, Hizmetleri bir makine kümesi genelinde düzenler. Yetkisiz kullanıcıların, özellikle üretim iş yüklerini çalıştırdıkları durumlarda bu kullanıcılara bağlanmasını engellemek için kümelerin güvenliği sağlanmalıdır. Güvenli olmayan bir küme oluşturmak mümkün olsa da, bu, anonim kullanıcıların kümeye bağlanmasına izin verebilir (Yönetim uç noktalarını genel İnternet 'e açığa alıyorsa).

Tek başına veya Azure 'da çalışan kümeler için, dikkate alınması gereken iki senaryo düğümden düğüme güvenlik ve istemciden düğüme güvenlik. Bu senaryoları uygulamak için çeşitli teknolojiler kullanabilirsiniz.

### <a name="node-to-node-security"></a>Düğümden düğüme güvenlik
Düğümden düğüme güvenlik, bir kümedeki VM 'Ler veya makineler arasındaki iletişim için geçerlidir. Düğümden düğüme güvenlik ile, yalnızca kümeye katılma yetkisine sahip olan bilgisayarlar, kümedeki uygulamaları ve Hizmetleri barındırmak için katılabilirler.

Windows üzerinde çalışan Azure veya tek başına kümeler üzerinde çalışan kümeler, Windows Server makineleri için [sertifika güvenliği](https://msdn.microsoft.com/library/ff649801.aspx) veya [Windows güvenliği](https://msdn.microsoft.com/library/ff649396.aspx) kullanabilir.

#### <a name="node-to-node-certificate-security"></a>Düğümden düğüme Sertifika güvenliği

Service Fabric, bir küme oluştururken belirttiğiniz X. 509.440 sunucu sertifikalarını kullanır. Bu sertifikaların ne olduğuna ve bunları nasıl elde edebilir veya oluşturabileceğiniz hakkında hızlı bir genel bakış için bkz. [sertifikalarla çalışma](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

Azure portal, Azure Resource Manager şablonları veya tek başına JSON şablonu aracılığıyla küme oluştururken sertifika güvenliği yapılandırırsınız. Birincil bir sertifika ve sertifika rollover 'lar için kullanılan isteğe bağlı bir ikincil sertifika belirtebilirsiniz. Belirttiğiniz birincil ve ikincil sertifikalar, yönetici istemcisinden ve [istemciden düğüme güvenlik](../../service-fabric/service-fabric-cluster-security.md)için belirttiğiniz salt okunurdur istemci sertifikalarından farklı olmalıdır.

### <a name="client-to-node-security"></a>İstemciden düğüme güvenlik
İstemci kimliklerini kullanarak istemciden düğüme güvenliği yapılandırırsınız. Bir istemci ve küme arasında güven oluşturmak için, kümeyi, hangi istemci kimliklerinin güvenebileceği hakkında bilgi sahibi olacak şekilde yapılandırmanız gerekir.

Service Fabric, bir Service Fabric kümesine bağlı istemciler için iki erişim denetimi türünü destekler:

-   **Yönetici**: Okuma/yazma özellikleri dahil olmak üzere yönetim özelliklerine tam erişim.
-   **Kullanıcı**: Yalnızca yönetim özelliklerine okuma erişimi (örneğin, sorgu özellikleri) ve uygulama ve Hizmetleri çözme özelliği.

Küme yöneticileri, erişim denetimini kullanarak belirli küme işlemleri türlerine erişimi sınırlayabilir. Bu, kümeyi daha güvenli hale getirir.

#### <a name="client-to-node-certificate-security"></a>İstemciden düğüme Sertifika güvenliği

Azure portal, Kaynak Yöneticisi şablonları veya tek başına JSON şablonu aracılığıyla bir küme oluşturduğunuzda istemciden düğüme Sertifika güvenliği yapılandırırsınız. Yönetici istemci sertifikası ve/veya Kullanıcı istemci sertifikası belirtmeniz gerekir. Bu sertifikaların, düğümden düğüme güvenlik için belirttiğiniz birincil ve ikincil sertifikalardan farklı olduğundan emin olun.

Yönetici sertifikasını kullanarak kümeye bağlanan istemciler, yönetim özelliklerine tam erişime sahiptir. Salt okuma Kullanıcı istemci sertifikasını kullanarak kümeye bağlanan istemciler yalnızca yönetim özelliklerine okuma erişimine sahiptir. Diğer bir deyişle, bu sertifikalar rol tabanlı erişim denetimi (RBAC) için kullanılır.

Bir kümede sertifika güvenliğini yapılandırma hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonu kullanarak küme ayarlama](../../service-fabric/service-fabric-cluster-creation-via-arm.md).

#### <a name="client-to-node-azure-active-directory-security"></a>İstemciden düğüme Azure Active Directory güvenliği

Azure üzerinde çalışan kümeler Azure Active Directory (Azure AD) kullanarak yönetim uç noktalarına erişimi de güvenli hale getirebilirsiniz. Gerekli Azure Active Directory yapıtları oluşturma, küme oluşturma sırasında doldurma ve bu kümelere bağlanma hakkında daha fazla bilgi için, bkz. [bir Azure Resource Manager şablonu kullanarak küme ayarlama](../../service-fabric/service-fabric-cluster-creation-via-arm.md).

Azure AD, kuruluşların (kiracılar olarak bilinir) uygulamalara Kullanıcı erişimini yönetmesine olanak sağlar. Web tabanlı bir oturum açma ARABIRIMI ve yerel istemci deneyimine sahip uygulamalar vardır.

Service Fabric kümesi, Web tabanlı Service Fabric Explorer ve Visual Studio da dahil olmak üzere yönetim işlevlerine birkaç giriş noktası sunar. Sonuç olarak, kümeye erişimi denetlemek için iki Azure AD uygulaması oluşturursunuz: bir Web uygulaması ve bir yerel uygulama.

Azure kümelerinde, düğümden düğüme güvenlik için istemcilerin ve sertifikaların kimliğini doğrulamak üzere Azure AD güvenliği kullanmanızı öneririz.

Windows Server 2012 R2 ve Active Directory içeren tek başına Windows Server kümeleri için, grup yönetilen hizmet hesapları (gMSAs) ile Windows güvenliği kullanmanızı öneririz. Aksi takdirde, Windows Güvenlik ile Windows hesaplarını kullanın.

## <a name="understand-monitoring-and-diagnostics-in-service-fabric"></a>Service Fabric izleme ve tanılamayı anlama
[İzleme ve tanılama](../../service-fabric/service-fabric-diagnostics-overview.md) , her türlü ortamda uygulama ve hizmet geliştirmek, test etmek ve dağıtmak için önemlidir. Service Fabric çözümler, uygulama ve hizmetlerin yerel bir geliştirme ortamında veya üretimde beklendiği gibi çalışmasını sağlamak için izleme ve tanılama uyguladığınızda en iyi şekilde çalışır.

Bir güvenlik perspektifinden, izlemenin ve Tanılamanın ana amaçları şunlardır:

-   Bir güvenlik olayının neden olabileceği donanım ve altyapı sorunlarını tespit edin ve tanılayın.
-   Uzlaşmaya yönelik bir gösterge (IOC) olabilecek yazılım ve uygulama sorunlarını tespit edin.
-   Yanlışlıkla hizmet reddine engel olmak için kaynak tüketimini anlayın.

İzleme ve tanılama iş akışı üç adımdan oluşur:

1.  **Olay oluşturma**: Olay oluşturma, hem altyapı (küme) düzeyinde hem de uygulama/hizmet düzeyinde olayları (Günlükler, izlemeler, özel olaylar) içerir. Sunulan özellikleri ve daha fazla izleme eklemeyi anlamak için [altyapı düzeyinde olaylar](../../service-fabric/service-fabric-diagnostics-event-generation-infra.md) ve [uygulama düzeyi olaylar](../../service-fabric/service-fabric-diagnostics-event-generation-app.md) hakkında daha fazla bilgi edinin.

2.  **Olay toplama**: Oluşturulan olayların görüntülenebilmesi için toplanıp toplanmaları gerekir. Genellikle [Azure tanılama](../../service-fabric/service-fabric-diagnostics-event-aggregation-wad.md) (aracı tabanlı günlük koleksiyonuna benzer) veya [EventFlow](../../service-fabric/service-fabric-diagnostics-event-aggregation-eventflow.md) (işlem içi günlük koleksiyonu) kullanmanızı öneririz.

3.  **Analiz**: Analiz ve görüntü sağlamak için olayların bir biçimde görselleştirildiği ve erişilebilir olması gerekir. İzleme ve tanılama verilerinin çözümlenmesi ve görselleştirilmesi için çeşitli platformlar vardır. [Azure izleyici günlükleri](../../service-fabric/service-fabric-diagnostics-event-analysis-oms.md) ve [Azure Application Insights](../../service-fabric/service-fabric-diagnostics-event-analysis-appinsights.md) önerilir, çünkü Service Fabric iyi bir şekilde tümleşir.

[Azure izleyici](../../azure-monitor/overview.md) 'yi bir Service Fabric kümesinin oluşturulduğu birçok Azure kaynağını izlemek için de kullanabilirsiniz.

İzleme, hizmetler genelinde sistem durumunu ve yüklemeyi izleyebilecek ve sistem durumu modeli hiyerarşisindeki herhangi bir şey için sistem durumunu rapor eden ayrı bir hizmettir. Bir izleme kullanmak, tek bir hizmetin görünümüne göre algılanamayan hataları önlemeye yardımcı olabilir. 

Watchdogs, Kullanıcı etkileşimi olmadan düzeltici eylemleri gerçekleştiren kod barındırmak için de iyi bir yerdir. Bir örnek, depolama alanında belirli zaman aralıklarında günlük dosyalarını temizliyor. [Azure Service Fabric izleme örneğinde](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/)örnek bir izleme hizmeti uygulamasını bulabilirsiniz.

## <a name="secure-communication-by-using-certificates"></a>Sertifikaları kullanarak güvenli iletişim
Sertifikalar, tek başına Windows kümenizin çeşitli düğümleri arasındaki iletişimi güvenli hale getirmenize yardımcı olur. X. 509.440 sertifikalarını kullanarak, bu kümeye bağlanan istemcilerin kimliklerini de doğrulayabilirsiniz. Bu, yalnızca yetkili kullanıcıların kümeye erişebilmesini sağlar. Oluşturduğunuz sırada kümede bir sertifikayı etkinleştirmenizi öneririz.

X. 509.440 dijital sertifikalar genellikle istemci ve sunucuların kimlik doğrulaması için kullanılır. İletileri şifrelemek ve dijital olarak imzalamak için de kullanılır.

Aşağıdaki tabloda, küme kurulumunda ihtiyacınız olan sertifikalar listelenmektedir:

|Sertifika bilgileri ayarı |Açıklama|
|-------------------------------|-----------|
|ClusterCertificate|    Bu sertifika, bir kümedeki düğümler arasındaki iletişimin güvenliğini sağlamak için gereklidir. İki küme sertifikası kullanabilirsiniz: birincil sertifika ve yükseltme için ikincil.|
|Sunucusertifikası| Bu sertifika, bu kümeye bağlanmayı denediğinde istemciye sunulur. İki sunucu sertifikası kullanabilirsiniz: birincil sertifika ve yükseltme için ikincil.|
|Clientcertificateparmak Izleri|  Bu, kimliği doğrulanmış istemcilere yüklenecek bir sertifika kümesidir.|
|ClientCertificateCommonNames|  Bu, CertificateCommonName için ilk istemci sertifikasının ortak adıdır. Certificateıssuerparmak Izi, bu sertifikayı veren için parmak izi.|
|Smarproxycertificate|   Bu, [ters proxy](../../service-fabric/service-fabric-reverseproxy.md)'nizin güvenliğini sağlamak için belirtebileceğiniz isteğe bağlı bir sertifikadır.|

Sertifikaların güvenliğini sağlama hakkında daha fazla bilgi için bkz. [X. 509.440 sertifikalarını kullanarak Windows 'da tek başına kümeyi güvenli hale](../../service-fabric/service-fabric-windows-cluster-x509-security.md)getirme.

## <a name="understand-role-based-access-control"></a>Rol tabanlı erişim denetimini anlama
Yönetici ve Kullanıcı istemci rollerini, her biri için ayrı kimlikler (Sertifikalar dahil) sağlayarak küme oluşturma sırasında belirtirsiniz. Varsayılan erişim denetimi ayarları ve varsayılan ayarların nasıl değiştirileceği hakkında daha fazla bilgi için bkz. [Service Fabric istemcileri Için rol tabanlı erişim denetimi](../../service-fabric/service-fabric-cluster-security-roles.md).

## <a name="secure-standalone-clusters-by-using-windows-security"></a>Windows güvenliğini kullanarak tek başına kümeleri güvenli hale getirme
Service Fabric kümesine yetkisiz erişimi engellemek için, kümeyi güvenli hale getirin. Küme üretim iş yüklerini çalıştırdığında güvenlik özellikle önemlidir. ClusterConfig. JSON dosyasındaki Windows güvenliğini kullanarak düğümden düğüme ve istemciden düğüme güvenliği yapılandırırsınız.

Service Fabric bir gMSA altında çalışması gerektiğinde, [Clustergmsaıdentity](../../service-fabric/service-fabric-windows-cluster-windows-security.md)ayarlayarak düğümden düğüme güvenliği yapılandırırsınız. Düğümler arasında güven ilişkileri oluşturmak için, onları birbirleriyle haberdar etmeniz gerekir.

Bir Active Directory etki alanı içinde makine grubu kullanmak istiyorsanız, Kümekimliği ayarlayarak düğümden düğüme güvenliği yapılandırırsınız. Daha fazla bilgi için bkz. [Active Directory makine grubu oluşturma](https://msdn.microsoft.com/library/aa545347).

Clienentidentities kullanarak istemciden düğüme güvenliği yapılandırırsınız. Kümeyi güvenebileceği istemci kimliklerini tanıyacak şekilde yapılandırmanız gerekir. Güveni iki şekilde oluşturabilirsiniz:

-   Bağlanabilecek etki alanı grubu kullanıcılarını belirtin.
-   Bağlanabilecek etki alanı düğümü kullanıcılarını belirtin.

## <a name="configure-application-security-in-service-fabric"></a>Service Fabric 'de uygulama güvenliğini yapılandırma
### <a name="manage-secrets-in-service-fabric-applications"></a>Service Fabric uygulamalarında gizli dizileri yönetme
Gizlilikler, depolama bağlantı dizeleri, parolalar veya düz metin olarak işlenmemelidir diğer değerler gibi herhangi bir hassas bilgi olabilir.

Anahtarları ve gizli dizileri yönetmek için [Azure Key Vault](../../key-vault/key-vault-overview.md) kullanabilirsiniz. Ancak, bir uygulamadaki gizli dizi kullanımı belirli bir bulut platformuna bağlı değildir. Uygulamaları herhangi bir yerde barındırılan bir kümeye dağıtabilirsiniz. Bu akışta dört ana adım vardır:

1.  Veri şifreleme sertifikası alın.
2.  Sertifikayı kümenize yükler.
3.  Bir uygulamayı sertifikayla dağıtmakta ve bir hizmetin Settings. xml yapılandırma dosyasına eklerken gizli değerleri şifreleyin.
4.  Şifrelenmiş değerleri, aynı şifreleme sertifikasıyla şifrelerini çözerek Settings. xml dosyasından okuyun.

Daha fazla bilgi için bkz. [Service Fabric uygulamalarında gizli dizileri yönetme](../../service-fabric/service-fabric-application-secret-management.md).

### <a name="configure-security-policies-for-an-application"></a>Bir uygulama için güvenlik ilkelerini yapılandırma
Azure Service Fabric güvenliği 'ni kullanarak, farklı Kullanıcı hesapları altında kümede çalışan uygulamaların güvenli hale getirilmesine yardımcı olabilirsiniz. Service Fabric güvenliği, uygulamaların kullanıcı hesapları kapsamında (örneğin, dosyalar, dizinler ve sertifikalar) dağıtımı sırasında kullandığı kaynakların korunmasına da yardımcı olur. Bu, paylaşılan barındırılan bir ortamda bile çalışan uygulamaları daha güvenli hale getirir.

Güvenlik ilkelerini yapılandırmaya yönelik görevler şunları içerir:

-   Bir hizmet kurulumu giriş noktası için ilkeyi yapılandırma
-   Bir kurulum giriş noktasından PowerShell komutları başlatılıyor
-   Yerel hata ayıklama için konsol yeniden yönlendirmeyi kullanma
-   Hizmet kodu paketleri için ilke yapılandırma
-   HTTP ve HTTPS uç noktaları için güvenlik erişimi ilkesi atama

## <a name="secure-communication-for-services"></a>Hizmetler için güvenli iletişim
Güvenlik, iletişimin en önemli yönlerinden biridir. Reliable Services uygulama çerçevesi, güvenliği artırmak için kullanabileceğiniz, önceden oluşturulmuş birkaç iletişim yığını ve aracı sağlar. Daha fazla bilgi için bkz. [hizmet Için güvenli hizmet uzaktan iletişim iletişimleri](../../service-fabric/service-fabric-reliable-services-secure-communication.md).

## <a name="next-steps"></a>Sonraki adımlar
- Küme güvenliği hakkında kavramsal bilgiler için bkz. [Azure Resource Manager kullanarak Service Fabric kümesi oluşturma](../../service-fabric/service-fabric-cluster-creation-via-arm.md) ve [Azure Portal kullanarak Service Fabric kümesi oluşturma](../../service-fabric/service-fabric-cluster-creation-via-portal.md).
- Service Fabric 'de küme güvenliği hakkında daha fazla bilgi için bkz. [Service Fabric kümesi güvenlik senaryoları](../../service-fabric/service-fabric-cluster-security.md).
