---
title: Azure Etkinlik Hub'larında paylaşılan erişim imzasıyla erişimi yetkilendirme
description: Bu makalede, Paylaşılan Erişim İmzaları (SAS) kullanarak Azure Etkinlik Hub'ları kaynaklarına erişim yetkisi hakkında bilgi verilmektedir.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: bdb1896f8a40c6de21ae76b536bfccec316341cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69992802"
---
# <a name="authorizing-access-to-event-hubs-resources-using-shared-access-signatures"></a>Paylaşılan Erişim İmzalarını kullanarak Olay Hub kaynaklarına erişim yetkisi verme
Paylaşılan erişim imzası (SAS), Olay Hub'ları ad alanınızdaki kaynaklara sınırlı erişim sağlamanızı sağlar. SAS, yetkilendirme kurallarına dayalı Olarak Olay Hub'ları kaynaklarına erişimi korur. Bu kurallar bir ad alanı veya varlık (olay hub veya konu) üzerinde yapılandırılır. Bu makalede, SAS modeline genel bir bakış sağlar ve SAS en iyi uygulamalarını gözden geçirir.

## <a name="what-are-shared-access-signatures"></a>Paylaşılan erişim imzaları nelerdir?
Paylaşılan erişim imzası (SAS), yetkilendirme kurallarına dayalı Olarak Olay Hub'ları kaynaklarına temsilci erişimi sağlar. Yetkilendirme kuralının bir adı vardır, belirli haklarla ilişkilidir ve bir çift şifreleme anahtarı taşır. SAS belirteçleri oluşturmak için kuralın adını ve anahtarını Olay Hub'ları istemcileri veya kendi kodunuzda kullanırsınız. İstemci daha sonra istenen işlem için yetkilendirmeyi kanıtlamak için belirteci Olay Hub'larına geçirebilir.

SAS, basit belirteçler kullanan talep tabanlı bir yetkilendirme mekanizmasıdır. SAS kullanılarak, anahtarlar asla kabloya geçirilir. Anahtarlar, daha sonra hizmet tarafından doğrulanabilecek bilgileri şifreleme olarak imzalamak için kullanılır. SAS, istemcinin hemen bir yetkilendirme kuralı adı ve eşleşen bir anahtara sahip olduğu bir kullanıcı adı ve parola düzenine benzer şekilde kullanılabilir. SAS, istemcinin imza anahtarına sahip olmadan bir güvenlik belirteci hizmetinden zaman sınırlı ve imzalı erişim belirteci aldığı federe güvenlik modeline benzer şekilde kullanılabilir.

> [!NOTE]
> Azure Etkinlik Hub'ları, Azure Etkin Dizin (Azure AD) kullanarak Etkinlik Hub'ları kaynaklarına yetki lendirmeyi destekler. Azure AD tarafından döndürülen OAuth 2.0 belirteci kullanarak kullanıcıları veya uygulamaları yetkilendirmek, paylaşılan erişim imzaları (SAS) üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile belirteçleri kodunuzda depolamaya ve olası güvenlik açıklarını riske atmaya gerek yoktur.
>
> Microsoft, azure etkinlik hub'ları uygulamalarınızla mümkün olduğunda Azure AD kullanmanızı önerir. Daha fazla bilgi için bkz: [Azure Etkin Dizinini kullanarak Azure Etkinlik Hub'larına erişimi yetkilendirme](authorize-access-azure-active-directory.md).

> [!IMPORTANT]
> SAS (Paylaşılan Erişim İmzaları) belirteçleri kaynaklarınızı korumak için çok önemlidir. SAS, ayrıntılılık sağlarken, istemcilere Etkinlik Hub'ları kaynaklarınıza erişim izni verir. Bunlar herkese açık olarak paylaşılmamalıdır. Paylaşımda, sorun giderme nedenleriyle gerekirse, günlük dosyalarının azaltılmış bir sürümünü kullanmayı veya günlük dosyalarından SAS belirteçlerini (varsa) silmeyi düşünün ve ekran görüntülerinin SAS bilgilerini içermediğinden emin olun.

## <a name="shared-access-authorization-policies"></a>Paylaşılan erişim yetkilendirme ilkeleri
Her Olay Hub'ı ad alanı ve her Olay Hub'ı varlığı (olay merkezi örneği veya Kafka konusu) kurallardan oluşan paylaşılan bir erişim yetkilendirme ilkesine sahiptir. Ad alanı düzeyindeki ilke, bireysel ilke yapılandırmalarından bağımsız olarak ad alanı içindeki tüm varlıklar için geçerlidir.
Her yetkilendirme ilkesi kuralı için üç bilgi parçasına karar verirsiniz: ad, kapsam ve haklar. Ad bu kapsamda benzersiz bir addır. Kapsam, söz konusu kaynağın URI'sidir. Olay Hub'ları ad alanı için kapsam, tam nitelikli alan adıdır `https://<yournamespace>.servicebus.windows.net/`(FQDN), örneğin.

İlke kuralı tarafından sağlanan haklar aşağıdakilerin bir birleşimi olabilir:
- **Gönder** – Varlığa ileti gönderme hakkı verir
- **Dinle** – Varlığı dinleme veya alma hakkı verir
- **Yönet** – Varlıkların oluşturulması ve silinmesi de dahil olmak üzere ad alanının topolojisini yönetme hakkı verir

> [!NOTE]
> **Yönet** hakkı **Gönder** ve **Dinle** haklarını içerir.

Ad alanı veya varlık ilkesi, her biri temel hakları kapsayan üç kural kümesi ne kadar alan ve Gönder ve Dinle kombinasyonunu sağlayarak en fazla 12 paylaşılan erişim yetkilendirme kuralı tutabilir. Bu sınır, SAS ilke deposunun bir kullanıcı veya hizmet hesabı deposu olması amaçlanmadığının altını çizer. Uygulamanızın kullanıcı veya hizmet kimliklerine dayalı Olay Hub kaynaklarına erişim izni vermesi gerekiyorsa, kimlik doğrulama ve erişim denetiminden sonra SAS belirteçleri veren bir güvenlik belirteç hizmeti uygulaması gerekir.

Yetkilendirme kuralına **birincil anahtar** ve **ikincil**anahtar atanır. Bu anahtarlar kriptografik olarak güçlü anahtarlar. Onları kaybetmeyin ya da sızdırmayın. Azure portalında her zaman kullanılabilir. Oluşturulan anahtarlardan birini kullanabilir ve istediğiniz zaman yeniden oluşturabilirsiniz. İlkede bir anahtarı yeniden oluşturur veya değiştirirseniz, bu anahtara dayalı olarak daha önce verilmiş tüm belirteçler anında geçersiz olur. Ancak, bu tür belirteçlere dayalı olarak oluşturulan devam eden bağlantılar belirteç süresi dolana kadar çalışmaya devam eder.

Bir Olay Hub'ları ad alanı oluşturduğunuzda, ad alanı için **RootManageSharedAccessKey** adında bir ilke kuralı otomatik olarak oluşturulur. Bu ilke, tüm ad alanı için izinleri **yönetmiştir.** Bu kuralı yönetimkök hesabı gibi ele alabilmeniz ve uygulamanızda kullanmamanız önerilir. Portaldaki ad alanı için **Yapılandırma** sekmesinde PowerShell veya Azure CLI üzerinden ek ilke kuralları oluşturabilirsiniz.

## <a name="best-practices-when-using-sas"></a>SAS kullanırken en iyi uygulamalar
Uygulamalarınızda paylaşılan erişim imzalarını kullandığınızda, iki olası riskin farkında olmanız gerekir:

- Bir SAS sızdırılırsa, bu bilgileri alan herkes tarafından kullanılabilir ve bu da Olay Hub'ları kaynaklarınızı tehlikeye atabilir.
- İstemci uygulamasına sağlanan bir SAS'ın süresi doluyorsa ve uygulama hizmetinizden yeni bir SAS alamıyorsa, uygulamanın işlevselliği engellenebilir.

Paylaşılan erişim imzalarını kullanmak için aşağıdaki öneriler, bu riskleri azaltmaya yardımcı olabilir:

- **İstemciler gerekirse SAS'ı otomatik olarak yenilemelidir**: SAS'ı sağlayan hizmet kullanılamıyorsa yeniden deneme için zaman tanımak için istemciler sas'ı son kullanmatarihinden önce yenilemelidir. SAS'nizin, son kullanma süresi içinde tamamlanması beklenen az sayıda ki acil ve kısa süreli işlemler için kullanılması gerekiyorsa, SAS'ın yenilenmesi beklenmez. Ancak, sas üzerinden rutin olarak istekte bulunuyor istemciniz varsa, son kullanma süresi nin dolması olasılığı devreye girer. Önemli olan, SAS'ın kısa ömürlü (daha önce de belirtildiği gibi) olması ihtiyacını, istemcinin yeterince erken yenileme talebinde bulunma ihtiyacıyla dengelemektir (başarılı bir yenilemeden önce sona eren SAS nedeniyle aksamayı önlemek için).
- **SAS başlangıç saatine dikkat edin**: SAS'ın başlangıç saatini **şimdi**olarak ayarlarsanız, saat çarpıklığı (farklı makinelere göre geçerli zaman farklılıkları) nedeniyle, ilk birkaç dakika boyunca aralıklı olarak arızalar görülebilir. Genel olarak, başlangıç saatini geçmişte en az 15 dakika olarak ayarlayın. Ya da, her durumda hemen geçerli hale getirecek, hiç ayarlamayın. Aynı genellikle son kullanma süresi için de geçerlidir. Herhangi bir istek üzerine her iki yönde de 15 dakikaya kadar saat eğriliğini gözlemleyebileceğini unutmayın. 
- **Erişilecek kaynağa özel olun:** Güvenlik en iyi uygulama, kullanıcıya gerekli en az ayrıcalıkları sağlamaktır. Bir kullanıcının yalnızca tek bir varlığa okuma erişimine ihtiyacı varsa, bu tek bir varlığa okuma erişimi hakkı tanıyın ve tüm varlıklara erişimi okuma/yazma/silmeyin. Ayrıca, SAS bir saldırganın elinde daha az güce sahip olduğu için bir SAS tehlikeye girerse hasarı azaltmaya yardımcı olur.
- **Her zaman SAS kullanmayın**: Bazen Etkinlik Hub'larınıza karşı belirli bir operasyonla ilişkili riskler SAS'ın yararlarından daha ağır basar. Bu tür işlemler için, iş kuralı doğrulama, kimlik doğrulama ve denetimden sonra Olay Hub'larınıza yazan bir orta katman hizmeti oluşturun.
- **Her zaman HTTP'leri kullanın**: SAS oluşturmak veya dağıtmak için her zaman Https'yi kullanın. Bir SAS HTTP üzerinden geçirilir ve ele geçirilirse, bir adam-in-the-middle eklemek gerçekleştiren bir saldırgan SAS okumak ve daha sonra amaçlanan kullanıcı olabilir gibi kullanmak, potansiyel olarak hassas verileri ödün veya kötü niyetli kullanıcı tarafından veri bozulması için izin kullanabilirsiniz.

## <a name="conclusion"></a>Sonuç
Paylaşım erişimi imzaları, müşterilerinize Etkinlik Hub'ları kaynaklarına sınırlı izinler sağlamak için yararlıdır. Azure Etkinlik Hub'larını kullanan tüm uygulamalar için güvenlik modelinin önemli bir parçasıdırlar. Bu makalede listelenen en iyi uygulamaları izlerseniz, uygulamanızın güvenliğinden ödün vermeden kaynaklarınıza daha fazla erişim esnekliği sağlamak için SAS'yi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki ilgili makalelere bakın: 

- [Azure Etkin Dizin'i kullanarak bir uygulamadan Azure Etkinlik Hub'larına yönelik istekleri kimlik doğrulaması](authenticate-application.md)
- [Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın](authenticate-managed-identity.md)
- [Paylaşılan Erişim İmzalarını kullanarak Azure Etkinlik Hub'larına istekleri nidoğrulatın](authenticate-shared-access-signature.md)
- [Azure Active Directory'yi kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme](authorize-access-azure-active-directory.md)


