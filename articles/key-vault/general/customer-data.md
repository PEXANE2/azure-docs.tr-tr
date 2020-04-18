---
title: Azure Key Vault müşteri veri özellikleri - Azure Key Vault | Microsoft Dokümanlar
description: Key Vault'ta müşteri verileri hakkında bilgi edinin
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: reference
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: bceea53e6b177940305a2dc77f2a6ecfa37f277a
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617907"
---
# <a name="azure-key-vault-customer-data-features"></a>Azure Key Vault müşteri veri özellikleri

Azure Key Vault kasalar, anahtarlar, sırlar, sertifikalar ve yönetilen depolama hesaplarının oluşturulması veya güncellemi sırasında müşteri verilerini alır. Bu Müşteri verileri Doğrudan Azure portalında ve REST API'sinde görülebilir. Müşteri verileri, verileri içeren nesneyi güncelleştirerek veya silerek düzenlenebilir veya silinebilir.

Bir kullanıcı veya uygulama Key Vault'a eriştığında sistem erişim günlükleri oluşturulur. Azure Öngörüleri'ni kullanan müşteriler için ayrıntılı erişim günlükleri kullanılabilir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Müşteri verilerinin tanımlanması

Aşağıdaki bilgiler Azure Key Vault içindeki müşteri verilerini tanımlar:

- Azure Anahtar Atlamasına yönelik erişim ilkeleri, kullanıcıları, grupları veya uygulamaları temsil eden nesne-iT'ler içerir
- Sertifika konuları e-posta adreslerini veya diğer kullanıcı veya kuruluş tanımlayıcılarını içerebilir
- Sertifika kişileri kullanıcı e-posta adreslerini, adlarını veya telefon numaralarını içerebilir
- Sertifika yı düzenleyenler e-posta adresleri, adlar, telefon numaraları, hesap kimlik bilgileri ve kuruluş bilgileri içerebilir
- Azure Anahtar Kasası'ndaki Nesnelere rasgele etiketler uygulanabilir. Bu nesneler kasaları, anahtarları, sırları, sertifikaları ve depolama hesaplarını içerir. Kullanılan etiketler kişisel veriler içerebilir
- Azure Key Vault erişim günlükleri, her REST API çağrısı için nesne-iD'ler, [UPN'ler](../../active-directory/hybrid/plan-connect-userprincipalname.md)ve IP adresleri içerir
- Azure Key Vault tanı günlükleri REST API aramaları için nesne-iD'ler ve IP adresleri içerebilir

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Kasalar, anahtarlar, sırlar, sertifikalar ve yönetilen depolama hesapları oluşturmak için kullanılan aynı REST API'leri, Portal deneyimi ve SDK'lar da bu nesneleri güncelleyebilir ve silebilir.

Yumuşak silme, silinen verileri silindikten sonra 90 gün boyunca kurtarmanızı sağlar. Yumuşak silme kullanırken, 90 günlük saklama süresi bir temizleme işlemi gerçekleştirerek sona ermeden önce veriler kalıcı olarak silinebilir. Kasa veya abonelik temizleme işlemlerini engelleyecek şekilde yapılandırıldıysa, zamanlanan bekletme süresi geçene kadar verileri kalıcı olarak silmek mümkün değildir.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Kasalar, anahtarlar, sırlar, sertifikalar ve yönetilen depolama hesapları oluşturmak için kullanılan aynı REST API'leri, portal deneyimi ve SDK'lar da bu nesneleri görüntülemenize ve dışa aktarmanıza olanak sağlar.

Azure Key Vault erişim günlüğü, her REST API çağrısı için günlük oluşturmak için açılabilir isteğe bağlı bir özelliktir. Bu günlükler, kuruluşunuzun gereksinimlerini karşılayan bekletme ilkesini uyguladığınız aboneliğinizdeki bir depolama hesabına aktarılır.

Kişisel verileri içeren Azure Key Vault tanı günlükleri, Kullanıcı Gizliliği portalında bir dışa aktarma isteği nde bulunarak alınabilir. Bu istek kiracı yönetici tarafından yapılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasa Sıtkı Günlüğü](logging.md))

- [Azure Key Vault geçici silmeye genel bakış](soft-delete-cli.md)

- [Azure Key Vault anahtar işlemleri](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Azure Key Vault gizli işlemleri](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Azure Anahtar Kasası sertifikaları ve ilkeleri](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Azure Key Vault depolama hesabı işlemleri](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
