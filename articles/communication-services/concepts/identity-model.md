---
title: Kimlik modeli
titleSuffix: An Azure Communication Services concept
description: Kimlikler ve erişim belirteçleri hakkında bilgi edinin
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 10/26/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 336e708334778e107331f5f393476c4b3dbb98a3
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94507551"
---
# <a name="identity-model"></a>Kimlik modeli

Azure Iletişim Hizmetleri, Identity-nostic hizmetidir. Bu tasarımın birden çok avantajı vardır:
- Kimlik yönetimi sisteminizden mevcut kimlikleri yeniden kullanma
- Tümleştirme senaryoları için esneklik
- Kimliklerinizi Azure Iletişim Hizmetleri için özel tutar

Sisteminizdeki mevcut bilgileri çoğaltmak yerine, iş çalışmanıza özgü eşleme ilişkisini korursunuz. Örneğin, 1:1, 1: N, N:1, N:M. kimliklerini eşleme Dış tanımlayıcılar (telefon numaraları, kullanıcılar, cihazlar, uygulamalar, GUID gibi) Azure Iletişim kimliği olarak kullanılamaz. Azure Iletişim hizmeti kimliği için oluşturulan erişim belirteçleri, sohbet veya çağırma gibi temel elemanlarına erişmek için kullanılır. 

## <a name="identity"></a>Kimlik

Kimlikler, Azure Communication Service Yönetim Kitaplığı ile oluşturulur. Kimlik, konuşmalardaki tanımlayıcı olarak işlev görür ve erişim belirteçleri oluşturmak için kullanılır. Aynı kimlik birden çok cihazda birden çok eş zamanlı oturuma katılabilir. Kimliğin aynı anda birden çok etkin erişim belirteci olabilir. Kimlik, kaynak veya aboneliğin silinmesi, tüm erişim belirteçlerinin ve bu kimlik için depolanan tüm verilerin silinmesinin iptal edilmesine neden olur. Silinen kimlik yeni erişim belirteçleri veremez, daha önce depolanan verilere (örneğin, sohbet iletileri) erişemez. 

Sahip olduğunuz kimlik sayısına, ancak temel elemanların kullanımına göre ücretlendirilmez. Kimlik sayısının kısıtlamak zorunda olmadığı, uygulamanızın kimliklerini Azure Iletişim Hizmetleri kimlikleriyle eşleme. Eşleme özgürlüğü sayesinde, gizlilik koşullarına göre sorumluluğu gelir. Uygulamanızın kullanıcısı sisteminizden silinmesi istediğinde, bu kullanıcıyla ilişkili tüm kimlikleri silmeniz gerekir.

Azure Iletişim Hizmetleri anonim kullanıcılar için özel kimlikler sağlamıyor. Kullanıcılar ve kimlikler arasındaki eşlemeyi etkilemez, kimliğin anonim olup olmadığını anlamayabilir. Kavramı gereksinimlerinize uyacak şekilde tasarlayabilirsiniz. Önerimiz, her uygulamanın anonim kullanıcısı için yeni kimlik oluşturmaktır. Geçerli erişim belirtecinin elinde bulunan herkes, kimliğin silinmemiş içeriğe erişim alabilir. Örneğin, Kullanıcı tarafından gönderilen sohbet iletileri. Erişim yalnızca erişim belirtecinin parçası olan kapsamlarla kısıtlıdır. Kapsamlar hakkında daha fazla ayrıntı, bölüm *erişim belirtecinde* bulunur.

### <a name="mapping-of-identities"></a>Kimlikleri eşleme

Azure Iletişim Hizmetleri, ıMS işlevlerini çoğaltmıyor. Müşterilerin müşteriye özgü kimlikleri kullanması için bir yol sağlamaz. Örneğin, telefon numarası veya e-posta adresi. Bunun yerine, uygulamanızın kimliklerine atayabilmeniz için benzersiz tanımlayıcılar sağlar. Azure Iletişim Hizmetleri, kullanıcılarınızın gerçek kimliğini açığa çıkarması gerekebilecek herhangi bir tür bilgiyi depolamaz.

Yineleme yerine, kimlik etki alanındaki kullanıcıların Azure Iletişim hizmeti kimliklerine nasıl eşleneceğini tasarlayacaksınız. 1:1, 1: N, N:1 veya m:ntip düzenlerini izleyebilirsiniz. Tek bir kullanıcının tek bir kimlikle mi, yoksa birden çok kimliğe mi eşlendiğine karar verebilirsiniz. Yeni kimlik oluşturulduğunda, bu kimliğin eşlemesini uygulamanızın kullanıcısına veya kullanıcılarına depolamanız önerilir. Kimlikler, temel nesnelerin kullanımı için erişim belirteçleri gerektirdiğinden, kimliğin uygulamanızın kullanıcısı veya kullanıcıları için bilinmesi gerekir.

Kullanıcı depolaması için ilişkisel veritabanı kullanıyorsanız, uygulama, eşleme senaryonuza göre farklılık gösterebilir. 1:1 veya N:1 eşleyen senaryolar için, Azure Communication Services kimliğinizi depolamak üzere tabloya bir *Communicationservicesıd* sütunu ekleyebilirsiniz. İlişki 1: N veya N:D senaryolarında, ilişkisel veritabanında ayrı bir tablo oluşturmayı düşünebilirsiniz.

## <a name="access-token"></a>Erişim belirteci

Erişim belirteci, Azure Iletişim hizmeti temel elemanlarına erişim sağlamak için kullanılabilen bir JWT belirtecidir. Verilen erişim belirtecinin bütünlük koruması vardır ve talepleri, verildikten sonra değiştirilemez. Diğer bir deyişle, kimlik, süre sonu veya kapsamlar gibi özelliklerin el ile değiştirilmesi, erişim belirtecini geçersiz hale getirir. Geçersiz kılınabilme belirteçleri olan temel elemanlar kullanımı, temel erişim reddine neden olur. 

Erişim belirtecinin özellikleri şunlardır: *kimlik, süre sonu* ve *kapsamlar*. Erişim belirteci her zaman 24 saat için geçerlidir. Bu zaman erişim belirteci geçersiz kılındıktan sonra herhangi bir temel erişim için kullanılamaz. Kimliğin, sunucu tarafı hizmetinden yeni erişim belirteci isteme yöntemi vardır. Parametre *kapsamı* , kullanılabilecek boş olmayan bir temel kümesi tanımlar. Azure Iletişim Hizmetleri, erişim belirteçleri için aşağıdaki kapsamları destekler:

|Ad|Açıklama|
|---|---|
|Sohbet|  Bir sohbete katılma olanağı verir|
|Hizmetinin|  Kimlikleri ve telefon numaralarını çağırma olanağı verir|


Erişim belirtecini sona ermeden önce iptal etmek isterseniz, Azure Communication Service Yönetim Kitaplığı 'nı kullanarak bunu yapabilirsiniz. Belirtecin iptali anında iptal değildir ve yayılması 15 dakikaya kadar sürer. Kimlik, kaynak veya aboneliğin kaldırılması, tüm erişim belirteçlerinin iptalinden oluşmasına neden olur. Bir kullanıcının belirli işlevlere erişme yeteneğini kaldırmak istiyorsanız, tüm erişim belirteçlerini iptal edin. Daha sonra, daha sınırlı bir kapsam kümesiyle yeni bir erişim belirteci yayınlayın.
Azure Iletişim hizmeti 'nin erişim anahtarlarının dönmesi, eski erişim anahtarıyla oluşturulmuş tüm etkin erişim belirteçlerinin iptalinden oluşmasına neden olur. Tüm kimlikler Azure Iletişim hizmetine erişimi kaybedecektir ve yeni erişim belirteçleri vermek için gereklidir. 

İstemci uygulamasında değil, sunucu tarafı hizmetinize erişim belirteçleri verilmesini öneririz. Bu, verme işlemi için erişim anahtarı veya yönetilen kimlik olması gerekir. Erişim anahtarlarının istemci uygulamasıyla paylaşılması için güvenlik nedenleriyle önerilmez. İstemci uygulaması, istemcilerinizin kimliğini doğrulayabilecek ve adına erişim belirteci veren güvenilir hizmet uç noktasını kullanmalıdır. Mimari hakkında daha fazla ayrıntıyı [burada](./client-and-server-architecture.md)bulabilirsiniz.

Erişim belirteçlerini bir yedekleme deposuna önbellekliyorsanız, şifrelemeyi kullanmanızı öneririz. Erişim belirteci hassas verilere sahiptir ve korunmadığı takdirde kötü amaçlı etkinlik için kullanılabilir. Erişim belirtecine sahip ile SDK 'yı başlatabilir ve API 'ye erişim sağlayabilirsiniz. Erişilebilir API yalnızca erişim belirtecinin sahip olduğu kapsamlar temelinde kısıtlanır. Erişim belirteçlerinin yalnızca kapsamlarla verilmesini öneririz, bu gereklidir.