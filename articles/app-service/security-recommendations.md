---
title: Güvenlik önerileri
description: Paylaşılan sorumluluk modelimizde belirtildiği gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olmak için güvenlik önerilerini uygulayın. Uygulamanızın güvenliğini artırın.
author: barclayn
manager: barbkess
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: e3f901fd78436b42729607d38f0ddb533f31cd30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74684122"
---
# <a name="security-recommendations-for-app-service"></a>App Service için güvenlik önerileri

Bu makale, Azure Uygulama Hizmeti için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelimizde açıklandığı gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olacak ve Web App çözümlerinizin genel güvenliğini artıracaktır. Microsoft'un hizmet sağlayıcı sorumluluklarını yerine getirmek için ne yaptığı hakkında daha fazla bilgi için [Azure altyapı güvenliği'ni](../security/fundamentals/infrastructure.md)okuyun.

## <a name="general"></a>Genel

| Öneri | Yorumlar |
|-|-|----|
| Güncel kalın | Desteklenen platformların, programlama dillerinin, protokollerin ve çerçevelerin en son sürümlerini kullanın. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Yorumlar |
|-|----|
| Anonim erişimi devre dışı | Anonim istekleri desteklemeniz gerekmedikçe, anonim erişimi devre dışı kındırın. Azure Uygulama Hizmeti kimlik doğrulama seçenekleri hakkında daha fazla bilgi için [Azure Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirme](overview-authentication-authorization.md)bilgisine bakın.|
| Kimlik doğrulama gerektirir | Mümkün olduğunda, kimlik doğrulama ve yetkilendirmeyi işlemek için kod yazmak yerine Uygulama Hizmeti kimlik doğrulama modüllerini kullanın. [Azure Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirme](overview-authentication-authorization.md)ye bakın. |
| Kimlik doğrulaması erişimiyle arka uç kaynaklarını koruma | Kullanıcının kimliğini kullanabilir veya bir arka uç kaynağına kimlik doğrulamak için bir uygulama kimliği kullanabilirsiniz. Bir uygulama kimliğini kullanmayı seçtiğinizde yönetilen bir [kimlik](overview-managed-identity.md)kullanın.
| İstemci sertifikası kimlik doğrulaması gerektirme | İstemci sertifikası kimlik doğrulaması, yalnızca sağladığınız sertifikaları kullanarak kimlik doğrulaması yapabilecek istemcilerin bağlantılarına izin vererek güvenliği artırır. |

## <a name="data-protection"></a>Veri koruma

| Öneri | Yorumlar |
|-|-|
| HTTP'yi HTTP'lere Yönlendirme | Varsayılan olarak, istemciler hem HTTP hem de HTTPS kullanarak web uygulamalarına bağlanabilir. HTTPS hem şifreli hem de kimlik doğrulaması yapılan güvenli bir bağlantı sağlamak için SSL/TLS protokolünü kullandığından HTTP'yi HTTP'lere yönlendirmenizi öneririz. |
| Azure kaynaklarına iletişimi şifreleme | Uygulamanız [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) veya [Azure Depolama](/azure/storage/)gibi Azure kaynaklarına bağlandığında, bağlantı Azure'da kalır. Bağlantı Azure'daki paylaşılan ağdan geçtiğiiçin, tüm iletişimi her zaman şifrelemeniz gerekir. |
| Mümkün olan en son TLS sürümünü gerektirir | 2018 yılından bu yana yeni Azure Uygulama Hizmeti uygulamaları TLS 1.2 kullanıyor. TLS'nin yeni sürümleri, eski protokol sürümlerine göre güvenlik geliştirmelerini içerir. |
| FTPS'yi kullanma | Uygulama Hizmeti, dosyalarınızı dağıtmak için hem FTP'yi hem de FTPS'yi destekler. Mümkün olduğunda FTP yerine FTPS kullanın. Bu protokollerden biri veya her ikisi kullanılmadığında, [bunları devre dışı bırakmalısınız.](deploy-ftp.md#enforce-ftps) |
| Uygulama verilerinin güvenliğini sağlama | Veritabanı kimlik bilgileri, API belirteçleri veya özel anahtarlar gibi uygulama sırlarını kod veya yapılandırma dosyalarınızda depolamayın. Yaygın olarak kabul edilen yaklaşım, seçtiğiniz dilde standart deseni kullanarak [ortam değişkenleri](https://wikipedia.org/wiki/Environment_variable) olarak bunlara erişmektir. Azure Uygulama Hizmeti'nde, [uygulama ayarları](web-sites-configure.md) ve [bağlantı dizeleri](web-sites-configure.md)aracılığıyla ortam değişkenlerini tanımlayabilirsiniz. Uygulama ayarları ve bağlantı dizeleri Azure'da şifrelenmiş olarak depolanır. Uygulama ayarları, uygulama başladığında uygulamanızın işlem bellene enjekte edilmeden önce yalnızca şifresi çözülür. Şifreleme anahtarları düzenli olarak döndürülür. Alternatif olarak, gelişmiş sırlar yönetimi için Azure Uygulama Hizmeti uygulamanızı [Azure Key Vault](/azure/key-vault/) ile tümleştirebilirsiniz. [Anahtar Kasası'na yönetilen bir kimlikle erişerek,](../key-vault/tutorial-web-application-keyvault.md)Uygulama Hizmeti uygulamanız ihtiyacınız olan sırlara güvenli bir şekilde erişebilir. |

## <a name="networking"></a>Ağ Oluşturma

| Öneri | Yorumlar |
|-|-|
| Statik IP kısıtlamaları kullanma | Windows'daki Azure Uygulama Hizmeti, uygulamanıza erişmesine izin verilen IP adreslerinin bir listesini tanımlamanıza olanak tanır. İzin verilen liste tek tek IP adreslerini veya bir alt ağ maskesi tarafından tanımlanan bir dizi IP adresini içerebilir. Daha fazla bilgi için Azure [Uygulama Hizmeti Statik IP Kısıtlamaları'na](app-service-ip-restrictions.md)bakın.  |
| Yalıtılmış fiyatlandırma katmanını kullanma | Yalıtılmış fiyatlandırma katmanı dışında, tüm katmanlar uygulamalarınızı Azure Uygulama Hizmeti'ndeki paylaşılan ağ altyapısında çalıştırın. İzole edilen katman, uygulamalarınızı özel bir [Uygulama Hizmeti ortamında](environment/intro.md)çalıştırarak tam ağ yalıtımı sağlar. Bir Uygulama Hizmeti ortamı Azure [Sanal Ağı'nın](/azure/virtual-network/)kendi örneğinde çalışır.|
| Şirket içi kaynaklara erişirken güvenli bağlantılar kullanma | Şirket içi kaynaklara bağlanmak için [Karma bağlantıları,](app-service-hybrid-connections.md) [Sanal Ağ tümleştirmesini](web-sites-integrate-with-vnet.md)veya [Uygulama Hizmeti ortamını](environment/intro.md) kullanabilirsiniz. |
| Gelen ağ trafiğine maruz kalma sınırı | Ağ güvenlik grupları, ağ erişimini kısıtlamanızı ve açıkta kalan uç noktaların sayısını denetlemenize olanak tanır. Daha fazla bilgi için, [Bir Uygulama Hizmeti Ortamına Gelen Trafiği Nasıl Kontrol Edin.](environment/app-service-app-service-environment-control-inbound-traffic.md) |

## <a name="monitoring"></a>İzleme

| Öneri | Yorumlar |
|-|-|
|Azure Güvenlik Merkezi standart katmanını kullanma | [Azure Güvenlik Merkezi,](../security-center/security-center-app-services.md) Azure Uygulama Hizmeti ile yerel olarak entegre edilmiştir. Değerlendirmeleri çalıştırabilir ve güvenlik önerileri sağlayabilir. |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için [Güvenli Geliştirme Belgeleri'ne](../security/fundamentals/abstract-develop-secure-apps.md)bakın.
