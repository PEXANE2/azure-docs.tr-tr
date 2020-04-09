---
title: Azure Blockchain Hizmeti güvenliği
description: Azure Blockchain Hizmeti veri erişimi ve güvenlik kavramları
ms.date: 11/22/2019
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 989d9f2afad30517a85185878d694c0b6640e987
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879607"
---
# <a name="azure-blockchain-service-security"></a>Azure Blockchain Hizmeti güvenliği

Azure Blockchain Hizmeti, verilerinizi güvende ve kullanılabilir tutmak için çeşitli Azure özellikleri kullanır. Veriler yalıtım, şifreleme ve kimlik doğrulama kullanılarak güvenlidir.

## <a name="isolation"></a>Yalıtım

Azure Blockchain Hizmet kaynakları özel bir sanal ağda yalıtılır. Her hareket ve doğrulama düğümü sanal bir makinedir (VM). Bir sanal ağdaki VM'ler farklı bir sanal ağdaki VM'lere doğrudan iletişim kuramaz. Yalıtım, iletişimin sanal ağ içinde gizli kalmasını sağlar. Azure sanal ağ yalıtımı hakkında daha fazla bilgi için [Azure Genel Bulutu'nda yalıtıma](../../security/fundamentals/isolation-choices.md#networking-isolation)bakın.

![VNET diyagramı](./media/data-security/vnet.png)

## <a name="encryption"></a>Şifreleme

Kullanıcı verileri Azure depolama alanında depolanır. Kullanıcı verileri güvenlik ve gizlilik için hareket halinde ve istirahatte şifrelenir. Daha fazla bilgi için bkz: [Azure Depolama güvenlik kılavuzu.](../../storage/blobs/security-recommendations.md)

## <a name="authentication"></a>Kimlik Doğrulaması

Hareketler bir RPC bitiş noktası üzerinden blockchain düğümlerine gönderilebilir. İstemciler, kullanıcı kimlik doğrulamasını işleyen ve TLS üzerinden verileri şifreleyen bir ters proxy sunucusu kullanarak bir işlem düğümüyle iletişim kurar.

![Kimlik doğrulama diyagramı](./media/data-security/authentication.png)

RPC erişimi için üç kimlik doğrulama modu vardır.

### <a name="basic-authentication"></a>Temel kimlik doğrulama

Temel kimlik doğrulama, kullanıcı adı ve parolayı içeren bir HTTP kimlik doğrulama üstbilgisini kullanır. Kullanıcı adı blockchain düğümünün adıdır. Parola, bir üye veya düğümün sağlanması sırasında ayarlanır. Parola, Azure portalı veya CLI kullanılarak değiştirilebilir.

### <a name="access-keys"></a>Erişim tuşları

Erişim anahtarları, bitiş noktası URL'sinde bulunan rasgele oluşturulan bir dize kullanır. İki erişim anahtarı, anahtar döndürmeyi etkinleştirmenize yardımcı olur. Anahtarlar Azure portalından ve CLI'den yeniden oluşturulabilir.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure Etkin Dizin (Azure AD), kullanıcının Azure AD kullanıcı kimlik bilgilerini kullanarak Azure AD tarafından kimlik doğrulaması yaptığı bir talep tabanlı kimlik doğrulama mekanizması kullanır. Azure AD bulut tabanlı kimlik yönetimi sağlar ve müşterilerin tüm kuruluş genelinde tek bir kimlik kullanmasına ve buluttaki uygulamalara erişmesine olanak tanır. Azure Blockchain Hizmeti, Kimlik federasyonu, tek oturum açma ve çok faktörlü kimlik doğrulama sağlayan Azure AD ile tümleşir. Blockchain üyesi ve düğüm erişimi için kuruluşunuzdaki kullanıcıları, grupları ve uygulama rollerini atayabilirsiniz.

Azure AD istemci proxy'si [GitHub'da](https://github.com/Microsoft/azure-blockchain-connector/releases)kullanılabilir. İstemci proxy, kullanıcıyı Azure AD oturum açma sayfasına yönlendirir ve başarılı kimlik doğrulaması üzerine bir taşıyıcı belirteci elde eder. Daha sonra, kullanıcı Geth veya Truffle gibi bir Ethereum istemci uygulamasını istemci proxy'nin bitiş noktasına bağlar. Son olarak, bir işlem gönderildiğinde, istemci proxy taşıyıcı belirteci http üstbilgide enjekte eder ve ters proxy OAuth protokolü kullanarak belirteci doğrular.

## <a name="keys-and-ethereum-accounts"></a>Anahtarlar ve Ethereum hesapları

Bir Azure Blockchain Hizmeti üyesi sağlarken, bir Ethereum hesabı ve ortak ve özel anahtar çifti oluşturulur. Özel anahtar, blockchain'e işlem göndermek için kullanılır. Ethereum hesabı, ortak anahtarın esrarının son 20 baytı. Ethereum hesabına cüzdan da denir.

Özel ve ortak anahtar çifti JSON formatında bir anahtar dosyası olarak depolanır. Blockchain genel muhasebe hizmeti oluşturulduğunda girilen parola kullanılarak özel anahtar şifrelenir.

İşlemleri dijital olarak imzalamak için özel anahtarlar kullanılır. Özel blockchain'lerde, özel bir anahtar tarafından imzalanmış akıllı bir sözleşme imzalayankişinin kimliğini temsil eder. İmzanın geçerliliğini doğrulamak için alıcı, imzalayankişinin ortak anahtarını imzadan hesaplanan adresle karşılaştırabilir.

Takımyıldız tuşları, bir Quorum düğümini benzersiz olarak tanımlamak için kullanılır. Takımyıldız anahtarları düğüm sağlama sırasında oluşturulur ve Quorum'daki özel bir işlemin parametresi için özel olarak belirtilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain Hizmeti için Azure Etkin Dizin erişimini nasıl yapılandırabilirsiniz.](configure-aad.md)
