---
title: Azure Blockchain Belirteçleri tekkullanım
description: Azure Blockchain Belirteçleri birleştirme özelliği, gelişmiş senaryolar için belirteçler oluşturmak için esneklik sağlar.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: a3fe1b290917de20b7c3af31fe386ed93580d850
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325114"
---
# <a name="azure-blockchain-tokens-composability"></a>Azure Blockchain Belirteçleri tekkullanım

[!INCLUDE [Preview note](./includes/preview.md)]

Belirteç birleştirme özelliği, gelişmiş senaryolar için belirteçler oluşturmak için esneklik sağlar. [Önceden oluşturulmuş dört belirteç şablonu](templates.md#base-token-types)kullanılarak uygulanamayan karmaşık bir senaryonuz olabilir. Belirteç birleştirme özelliği, kendi belirteç şablonunuzu oluşturmak için tanımlanmış davranışları ekleyerek veya kaldırarak kendi belirteç şablonlarınızı tasarlamanıza olanak tanır. Yeni bir belirteç şablonu oluştururken, Azure Blockchain Belirteçleri tüm belirteç dilbilgisi kurallarını doğrular. Oluşturulan şablonlar, bağlı blockchain ağlarında vermek için Azure Blockchain Belirteçleri hizmetine kaydedilir.

Belirteç şablonunuzu tasarlamak için aşağıdaki bölümlerdeki [belirteç davranışlarını](templates.md#token-behaviors) kullanabilirsiniz.

## <a name="burnable-b"></a>Yanıcı (b)

Belirteçleri tedarikten kaldırma yeteneği.

Örneğin, bir hediye kartı için çevrimiçi kredi kartı puanlarını kullandığınızda, kredi kartı puanları yanar.

## <a name="delegable-g"></a>Atılabilir (g)

Sahip olduğunuz belirteç üzerinde gerçekleştirilen eylemleri temsilciye verme becerisi.

Temsilci, belirteci sahibi olarak eylemleri gerçekleştirebilirsiniz. Örneğin, oylamayı uygulamak için atılabilir bir belirteç kullanabilirsiniz. Atılabilir bir belirteç, oy belirteci sahibinin kendi adına başka birinin oy kullanmasını sağlar.

## <a name="logable-l"></a>Oturum açılabilir (l)

Kayıt olma yeteneği.

Örneğin, belirli bir filmi gösteren her tiyatroya film dağıtımı için bir günlüğe kaydedilebilir bir belirteç dağıtabilirsiniz. Filmin oynanabilmesi için, telif hakkı ödemeleri filmin gösterimi sırasında gösterilmeye göre olduğundan, gösterimin her gösterim için bir işlem kaydetmesi gerekir. Aktörler inşa dağıtımda tiyatro başına gösterilen film başına ödemeleri doğrulamak için film belirteçleri kullanabilirsiniz.

## <a name="mint-able-m"></a>Naneli (m)

Belirteç sınıfı için ek belirteçlere basma yeteneği. Minter rolü mintable davranışı içerir.

Örneğin, bir sadakat programı uygulamak isteyen bir perakende şirketi, sadakat programı için mintable belirteçleri kullanabilir. Müşteri tabanı büyüdükçe müşterileri için ek sadakat puanları nane basabilirler.  

## <a name="non-subdividable-or-whole-d"></a>Bölünemez veya bütün (~d)

Bir belirteci daha küçük parçalara bölünmesini önlemek için kısıtlama.

Örneğin, tek bir resim boyama birden çok küçük parçalara bölünemez. 

## <a name="non-transferable-t"></a>Devredilemez (~t)

İlk belirteç sahibinden sahiplik değişikliğini önlemek için kısıtlama.

Örneğin, üniversite diploması devredilemez bir belirteçtir. Mezuna diploma verildikten sonra, mezundan başka bir kişiye devredilemez.

## <a name="roles-r"></a>Roller (r)

Belirli davranışlar için belirteç şablonu sınıfındaki rolleri tanımlama yeteneği.

Belirteç oluşturma zamanında bir belirteç desteklediği rol adlarının bir listesini sağlayabilirsiniz. Roller belirtildiğinde, kullanıcı bu davranışlara roller atayabilir. Şu anda, sadece minter rolü desteklenir.

## <a name="singleton-s"></a>Singleton (lar)

Bir belirteç kaynağına izin verme kısıtlaması.

Örneğin, bir müze eser singleton belirtecidir. Müze eserleri eşsizdir. Bir yapıyı temsil eden bir belirteç, yalnızca tek bir öğeye sahiptir.

## <a name="subdividable-d"></a>Bölünebilir (d)

Bir belirteci daha küçük parçalara bölme yeteneği.

Örneğin, bir dolar sente bölünebilir.

## <a name="transferable-t"></a>Devredilebilir (t)

Belirteç sahipliğini devretme yeteneği.

Örneğin, mülk unvanı, mülk satıldığında bir kişiden diğerine aktarılabilen devredilebilir bir belirteçtir.

## <a name="next-steps"></a>Sonraki adımlar

Azure [Blockchain Belirteçleri hesap yönetimi](account-management.md)hakkında bilgi edinin.
