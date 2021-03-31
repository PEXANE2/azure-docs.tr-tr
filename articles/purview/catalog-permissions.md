---
title: Katalog Izinleri (Önizleme)
description: Bu makalede, Azure purview 'da Role-Based Access Control (RBAC) yapılandırma hakkında genel bir bakış sunulmaktadır
author: yarong
ms.author: yarong
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 10/20/2020
ms.openlocfilehash: b351be1e7212dc9923f701599dd951a73254afe0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98610378"
---
# <a name="role-based-access-control-in-azure-purviews-data-plane"></a>Azure purview 'un veri düzlemine rol tabanlı erişim denetimi

Bu makalede, Azure purview 'ın [veri düzlemine](../azure-resource-manager/management/control-plane-and-data-plane.md#data-plane)Role-Based Access Control (RBAC) nasıl uygulandığı açıklanır.

> [!IMPORTANT]
> Bir purview hesabı oluşturan sorumlu, hangi veri düzlemine sahip olabileceklerini veya içinde bulunmayabilir, tüm veri düzlemi izinleri otomatik olarak verilir. Diğer kullanıcıların Azure purview 'da herhangi bir şey yapması için önceden tanımlanmış veri düzlemi rollerinin en az birinde olması gerekir.

## <a name="azure-purviews-pre-defined-data-plane-roles"></a>Azure purview 'ın önceden tanımlanmış veri düzlemi rolleri

Azure takip görünümü, Azure purview 'da nelerin erişebileceğini denetlemek için kullanılabilen önceden tanımlanmış bir veri düzlemi rolleri kümesi tanımlar. Bu roller şunlardır:

* **Purview veri okuyucu rolü** -purview portalına erişebilir ve tarama bağlamaları hariç Azure purview 'daki tüm içeriği okuyabilir
* **Purview Data Curator rolü** -purview portalına erişebilir ve tarama bağlamaları hariç Azure purview içindeki tüm içeriği okuyabilir, varlıklar hakkındaki bilgileri düzenleyebilir, sınıflandırma tanımlarını ve sözlük koşullarını düzenleyebilir ve varlıklara sınıflandırmalar ve Sözlük terimleri uygulayabilir.
* **Purview veri kaynağı yönetici rolü** -purview portalına erişimi yok (kullanıcının da veri okuyucu veya veri seçkin rolünde olması gerekir) ve Azure purview 'a veri taramanın tüm yönlerini yönetebilir, ancak tarama ile ilgili olanların ötesinde Azure purview içeriğine okuma veya yazma erişimi yoktur.

## <a name="understanding-how-to-use-azure-purviews-data-plane-roles"></a>Azure purview 'ın veri düzlemi rollerinin nasıl kullanılacağını anlama

Bir Azure purview hesabı oluşturulduğunda, Oluşturucu hem purview Data Curator hem de veri kaynağı Yöneticisi rollerindeki gibi değerlendirilir. Ama hesabı oluşturan rol deposunda bu rollere atanmaz. Azure Purview hesabı oluşturanı sorumlu olarak tanır ve kimlikleri temelinde bu özellikleri onları da içerecek şekilde genişletir.

Azure Purview Hesabını kullanacak olan diğer tüm kullanıcıların bu rollerden en az birine eklenmesi gerekir. Bu, bir Azure purview hesabı oluşturulduğunda, hiç kimse oluşturulamadığını, ancak bir veya daha fazla tanımlı rolün bir veya daha fazlasına yerleştirene kadar API 'Leri kullanamayacağı anlamına gelir.

Lütfen purview veri kaynağı Yöneticisi rolünün iki desteklenen senaryoya sahip olduğunu unutmayın. İlk senaryo, veri okuyucularını zaten takip eden ve tarama oluşturamayacak veri seçicileri olan kullanıcılar içindir. Bu kullanıcıların iki rol halinde olması gerekir: en az bir purview veri okuyucusu ya da izlenen veri kaynağı Yöneticisi rolüne yerleştirilme.

Purview veri kaynağı Yöneticisi için diğer senaryo, taramalar ayarlamak ve izlemek ve kataloğun verilerine erişim sahibi olmaması gereken hizmet sorumluları gibi programlama işlemlerine yöneliktir.

Bu senaryo, hizmet sorumlusu diğer iki rolün hiçbirine yerleştirilmeksizin purview veri kaynağı Yöneticisi rolüne yerleştirilerek uygulanabilir. Sorumlunun purview portalına erişimi olmaz, ancak bu o.k. Çünkü bu bir programlı sorumlu olduğundan ve yalnızca API 'Ler aracılığıyla iletişim kurar.

## <a name="putting-users-into-roles"></a>Kullanıcıları rollere yerleştirme

Bu nedenle, bir Azure purview hesabı oluşturduktan sonra ilk iş sıralaması, kişileri bu rollere atamakdır.

Rol ataması [Azure RBAC](../role-based-access-control/overview.md)aracılığıyla yönetilir.

Azure 'daki yalnızca iki yerleşik denetim düzlemi rolü, Kullanıcı rollerini atayabilir; Bunlar, sahipler veya Kullanıcı erişimi yöneticileridir. Bu nedenle, Azure purview için kişileri bu rollere koymak için bir sahip veya Kullanıcı erişimi Yöneticisi olan birini bulmalıdır ya da bir oneself olun.

### <a name="an-example-of-assigning-someone-to-a-role"></a>Bir role bir kullanıcı atama örneği

1. Adresine gidin https://portal.azure.com ve Azure purview hesabınıza gidin
1. Sol tarafta "erişim denetimi (ıAM)" seçeneğine tıklayın
1. Ardından [burada](../role-based-access-control/quickstart-assign-role-user-portal.md#create-a-resource-group) verilen genel yönergeleri izleyin

## <a name="role-definitions-and-actions"></a>Rol tanımları ve eylemler

Rol, bir eylem koleksiyonu olarak tanımlanır. Rollerin nasıl tanımlandığı hakkında daha fazla bilgi için [buraya](../role-based-access-control/role-definitions.md) bakın. Ve Azure purview [rolleri Için rol tanımları bölümüne bakın](../role-based-access-control/built-in-roles.md) .

## <a name="getting-added-to-a-data-plane-role-in-an-azure-purview-account"></a>Azure purview hesabındaki bir veri düzlemi rolüne ekleme

Kendi Studio 'yu kullanabilmeniz veya API 'Lerini çağırmak için bir Azure purview hesabına erişim verilmesini istiyorsanız bir Azure purview veri düzlemi rolüne eklenmeleri gerekir. Yalnızca bunu yapabilen kişiler, Azure purview hesabındaki sahipler veya Kullanıcı erişimi yöneticileridir. Çoğu kullanıcı için bir sonraki adım, size erişim izni veren doğru kişileri bulmanıza yardımcı olabilecek bir yerel yönetici bulvermektir.

Şirketlerinin [Azure Portal](https://portal.azure.com) erişimi olan kullanıcılar için, katılması Istedikleri belirli Azure purview hesabını arayabilir, "erişim denetımı (IAM)" sekmesine tıklayın ve sahiplerin veya Kullanıcı erişimi yöneticilerinin (UAAs) kim olduğunu görebilirsiniz. Ancak bazı durumlarda Azure Active Directory grupları veya hizmet sorumlularını sahipler veya UAAs olarak kullanılabileceğini unutmayın, bu durumda doğrudan iletişim kurmak mümkün olmayabilir. Bunun yerine, yardım için bir yönetici bulmayı içerir.

## <a name="who-should-be-assigned-to-what-role"></a>Hangi role kim atanmalıdır?

|Kullanıcı senaryosu|Uygun rol (ler)|
|-------------|-----------------|
|Yalnızca varlık buldum, herhangi bir şeyi düzenlemek istemiyorum|Purview veri okuyucu rolü|
|Varlıklar hakkındaki bilgileri düzenlemem, bunlara sınıflandırmalar yerleştirmem, sözlüğü sözlük girişleriyle ilişkilendirmem gerekiyor.|Data Curator rolünü takip etme|
|Sözlüğü düzenlemem veya yeni sınıflandırma tanımlarını ayarlaması gerekiyor|Data Curator rolünü takip etme|
|Uygulamamın hizmet sorumlusu, verileri Azure purview 'a itmesi gerekir|Data Curator rolünü takip etme|
|Purview Studio aracılığıyla taramalar ayarlama yapmam gerekir|Veri kaynağı Yöneticisi rolünü ve en az bir purview veri okuyucusu rolünü veya takip verileri Curator rolünü takip edin|
|Hizmet sorumlusu 'nı veya diğer programlı kimliği etkinleştirmek için, programlı kimliğin kataloğun bilgilerine erişmesine izin vermeden taramaları ayarlama ve izleme |Purview veri kaynağı Yöneticisi rolü|
|Kullanıcıları Azure purview 'da rollere yerleştirmem gerekiyor | Sahip veya Kullanıcı erişimi Yöneticisi |

Bir role güvenlik sorumlusu ekleme hakkında daha fazla bilgi için [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri içgörüleri](concept-insights.md)
