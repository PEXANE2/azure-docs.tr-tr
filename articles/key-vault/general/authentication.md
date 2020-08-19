---
title: Azure Key Vault kimlik doğrulaması
description: Azure Key Vault için kimlik doğrulaması yapmayı öğrenin
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 06/08/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: 7645600a476a1c2294ddd4a24fe01e2ffe51d5ac
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589992"
---
# <a name="authenticate-to-azure-key-vault"></a>Azure Key Vault kimlik doğrulaması

## <a name="overview"></a>Genel Bakış

Azure Key Vault, uygulama gizli dizilerini merkezileştirmenizi ve bunların dağıtımını denetlemenize olanak tanıyan bir gizli dizi yönetim çözümüdür. Azure Key Vault, kimlik bilgilerini uygulamalarda depolama gereksinimini ortadan kaldırır. Uygulamanız, gerekli kimlik bilgilerini almak için anahtar kasasında kimlik doğrulaması yapabilir. Bu belge, Anahtar Kasası kimlik doğrulamasının temellerini kapsar.

Bu belge, Anahtar Kasası kimlik doğrulamasının nasıl çalıştığını anlamanıza yardımcı olur. Bu belgede kimlik doğrulama akışı açıklanacaktır, anahtar kasanıza erişim izni verme ve örnek bir Python uygulamasından Anahtar Kasası 'nda depolanan gizli dizi listesini alma hakkında bir öğretici bulunur.

Bu belge şu şekilde ele alınacaktır:

* Önemli Kavramlar
* Güvenlik sorumlusu kaydı
* Key Vault kimlik doğrulama akışını anlama
* Key Vault için hizmet sorumlusu erişimi verme
* Öğretici (Python)

## <a name="key-concepts"></a>Önemli Kavramlar

### <a name="azure-active-directory-concepts"></a>Azure Active Directory kavramlar

* Azure Active Directory (AAD)-Azure Active Directory (Azure AD), çalışanlarınızın oturum açıp kaynaklara erişmesine yardımcı olan bulut tabanlı kimlik ve erişim yönetimi hizmetidir

* Rol tanımı-rol tanımı bir izin koleksiyonudur.  AAD 'de, bir Azure kaynağında okuma, yazma ve silme gibi işlemleri gerçekleştirmek için izin düzeylerini içeren standart roller (sahip, katkıda bulunan veya okuyucu) vardır. Roller Ayrıca, kullanıcılar tarafından özel ayrıntılı izinlerle oluşturulmuş özel tanımlar da olabilir.

* Uygulama kaydı-bir Azure AD uygulaması kaydettiğinizde, Azure AD kiracınızda iki nesne oluşturulur, bir uygulama nesnesi ve bir hizmet sorumlusu nesnesi. Uygulama nesnesini, tüm kiracılarda kullanılmak üzere uygulamanızın genel temsili olarak ve hizmet sorumlusu, belirli bir kiracıda kullanılmak üzere yerel gösterim olarak göz önünde bulundurun.

### <a name="security-principal-concepts"></a>Güvenlik sorumlusu kavramları

* Güvenlik sorumlusu-güvenlik sorumlusu, Azure kaynaklarına erişim isteyen bir Kullanıcı, Grup, hizmet sorumlusu veya yönetilen kimliği temsil eden bir nesnedir.

* Kullanıcı: Azure Active Directory'de bir profile sahip olan kişidir.

* Grup: Azure Active Directory'de oluşturulan kullanıcı kümesidir. Bir gruba rol atadığınızda ilgili gruptaki tüm kullanıcılar o role sahip olur.

* Hizmet sorumlusu: Uygulamalar veya hizmetler tarafından belirli Azure kaynaklarına erişmek için kullanılan güvenlik kimliğidir. Bunu bir uygulamanın kullanıcı kimliği (kullanıcı adı ve parola veya sertifika) olarak düşünebilirsiniz.

* Yönetilen kimlik-Azure tarafından otomatik olarak yönetilen Azure Active Directory bir kimlik.

* Nesne KIMLIĞI (Istemci KIMLIĞI)-Azure AD tarafından oluşturulan ve ilk sağlama sırasında bir hizmet sorumlusuna bağlı benzersiz bir tanımlayıcı.

## <a name="security-principal-registration"></a>Güvenlik sorumlusu kaydı

1. Yönetici, bir kullanıcı veya uygulamayı (hizmet sorumlusu) Azure Active Directory kaydeder.

2. Yönetici bir Azure Key Vault oluşturur ve erişim ilkelerini (ACL 'Ler) yapılandırır.

3. Seçim Yönetici Azure Key Vault güvenlik duvarını yapılandırır.

![GÖRÜNTÜ](../media/authentication-1.png)

## <a name="understand-the-key-vault-authentication-flow"></a>Key Vault kimlik doğrulama akışını anlayın

1. Hizmet sorumlusu, AAD 'ye kimlik doğrulaması yapmak için bir çağrı yapar. Bu işlem birkaç şekilde gerçekleşebilir:
    * Kullanıcı Kullanıcı adı ve parola kullanarak Azure portal oturum açabilir.
    * Bir uygulama, istemci KIMLIĞINI kullanır ve AAD 'ye bir istemci parolası veya istemci sertifikası sunar
    * Bir sanal makine gibi bir Azure kaynağında atanmış MSI bulunur ve erişim belirteci almak için ıMDS REST uç noktasıyla iletişim kurar.

2. AAD kimlik doğrulaması başarılı olursa, hizmet sorumlusuna bir OAuth belirteci verilecektir.
3. Hizmet sorumlusu Key Vault için bir çağrı yapar.
4. Azure Key Vault güvenlik duvarı, çağrıya izin verilip verilmeyeceğini belirler.
    * Senaryo 1: Key Vault güvenlik duvarı devre dışı bırakıldı, anahtar kasasının genel uç noktasına (URI) genel İnternet üzerinden erişilebilir. Çağrıya izin verilir.
    * Senaryo 2: arayan, güvenilir bir Azure Key Vault hizmetidir. Belirli Azure Hizmetleri, seçenek işaretliyse Anahtar Kasası güvenlik duvarını atlayabilir. [Key Vault güvenilir hizmet listesi](https://docs.microsoft.com/azure/key-vault/general/overview-vnet-service-endpoints#trusted-services)
    * Senaryo 3: arayan, IP adresi, sanal ağ veya hizmet uç noktası tarafından Azure Key Vault güvenlik duvarında listelenir.
    * Senaryo 4: arayan, yapılandırılmış bir özel bağlantı bağlantısı üzerinden Azure Key Vault ulaşabilir.
    * Senaryo 5: arayan yetkilendirilmemiş ve yasaklanmış bir yanıt döndürülür.
5. Key Vault, hizmet sorumlusunun erişim belirtecini doğrulamak için AAD 'ye bir çağrı yapar.
6. Key Vault, hizmet sorumlusunun istenen işlemi gerçekleştirmek için yeterli erişim ilkesi izinlerine sahip olup olmadığını denetler, bu örnekte işlem gizli alır.
7. Key Vault, hizmet sorumlusu için gizli dizi sağlar.

![GÖRÜNTÜ](../media/authentication-2.png)

## <a name="grant-a-service-principal-access-to-key-vault"></a>Key Vault için hizmet sorumlusu erişimi verme

1. Henüz yoksa bir hizmet sorumlusu oluşturun. [Hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)
2. Azure Key Vault ıAM ayarlarında hizmet sorumlusuna bir rol ataması ekleyin. Sahip, katkıda bulunan veya okuyucu için önceden atanmış roller ekleyebilirsiniz. Hizmet sorumlusu için özel roller de oluşturabilirsiniz. En az ayrıcalık sorumlusunu izlemeniz ve yalnızca hizmet sorumlusu için gereken en düşük erişimi sağlamanız gerekir. 
3.  Anahtar Kasası güvenlik duvarını yapılandırın. Anahtar Kasası güvenlik duvarını devre dışı tutabilir ve genel İnternet 'ten erişime izin verebilirsiniz (daha az güvenli, daha kolay yapılandırılabilir). Ayrıca belirli IP aralıklarına, hizmet uç noktalarına, sanal ağlara veya özel uç noktalara erişimi kısıtlayabilirsiniz (daha güvenli).
4.  Hizmet sorumlusu için bir erişim ilkesi ekleyin, bu, hizmet sorumlunun anahtar kasasında gerçekleştirebileceği işlemlerin bir listesidir. En az ayrıcalık sorumlusunu kullanmalı ve hizmet sorumlusunun gerçekleştirebileceği işlemleri sınırlayabilirsiniz. Bununla birlikte, yeterli izinleri sağlamazsanız hizmet sorumlunun erişimi reddedilir.

## <a name="tutorial"></a>Öğretici

Bu öğreticide, anahtar kasasında kimlik doğrulaması yapmak ve bir parola almak için bir hizmet sorumlusu ayarlamayı öğreneceksiniz. 

### <a name="part-1--create-a-service-principal-in-the-azure-portal"></a>1. kısım: Azure portal hizmet sorumlusu oluşturma

1. Azure portalında oturum açma
1. Azure Active Directory arayın
1. "Uygulama kayıtları" sekmesine tıklayın
1. "+ Yeni kayıt" seçeneğine tıklayın
1. Hizmet sorumlusu için bir ad oluşturun
1. Kaydol ' u seçin

Bu noktada, kayıtlı bir hizmet sorumlusu vardır. Bunu, "uygulama kayıtları" seçerek görüntüleyebilirsiniz. Hizmet sorumlusuna artık bir istemci KIMLIĞI GUID atanacak, bunu hizmet sorumlusu için bir "Kullanıcı adı" olarak düşünecektir. Artık hizmet sorumlusu için bir "parola" oluşturmanız gerekiyor, bir istemci gizli dizisi veya istemci sertifikası kullanabilirsiniz. Not, kimlik doğrulaması için bir istemci sırrı kullanılması güvenli değildir ve yalnızca sınama amacıyla kullanılmalıdır. Bu öğreticide, bir istemci sertifikasının nasıl kullanılacağı gösterilir.

### <a name="part-2-create-a-client-certificate-for-your-service-principal"></a>2. Bölüm: hizmet sorumlusu için bir istemci sertifikası oluşturma

1. Sertifika oluşturma

    * Seçenek 1: [OpenSSL](https://www.openssl.org/) kullanarak sertifika oluşturma (yalnızca test amaçlı olarak, üretimde otomatik olarak imzalanan sertifikalar kullanmayın)

    ```console
    openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
    ```

    * 2. seçenek: Anahtar Kasası kullanarak bir sertifika oluşturun. [Azure Key Vault bir sertifika oluşturun](https://docs.microsoft.com/azure/key-vault/certificates/certificate-scenarios#creating-your-first-key-vault-certificate)

1. Sertifikayı pek biçiminde indirin
1. Azure portal oturum açın ve Azure Active Directory gidin
1. "Uygulama kayıtları" na tıklayın
1. Bölüm 1 ' de oluşturduğunuz hizmet sorumlusunu seçin.
1. Hizmet sorumlunuz için "Sertifikalar ve gizlilikler" sekmesine tıklayın
1. "Sertifikayı karşıya yükle" düğmesini kullanarak sertifikayı karşıya yükleyin

### <a name="part-3-configure-an-azure-key-vault"></a>3. kısım: Azure Key Vault yapılandırma

1. Azure Key Vault [bağlantısı](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault) oluşturma

2. Key Vault ıAM izinlerini yapılandırma
    1. Anahtar kasanıza gidin
    1. "Access Control (ıAM)" sekmesini seçin
    1. Rol ataması Ekle ' ye tıklayın
    1. Açılan listeden "katkıda bulunan" rolü seçin
    1. Oluşturduğunuz hizmet sorumlusunun adını veya istemci KIMLIĞINI girin
    1. Hizmet sorumlunun listelendiğini doğrulamak için "rol atamalarını görüntüle" ye tıklayın

3. Key Vault erişim Ilkesi izinlerini yapılandırma
    1. Anahtar kasanıza gidin
    1. "Ayarlar" altındaki "erişim Ilkeleri" sekmesini seçin.
    1. "+ Erişim Ilkesi Ekle" bağlantısını seçin
    1. Gizli Izinler açılan menüsünde "Al" ve "List" izinlerini denetleyin.
    1. Hizmet sorumlusunu ada veya istemci KIMLIĞINE göre seçin.
    1. "Ekle" yi seçin
    1. "Kaydet" i seçin

4. Anahtar Kasanızda bir gizli dizi oluşturun.
    1. Anahtar kasanıza gidin
    1. Ayarlar altındaki "gizlilikler" sekmesine tıklayın
    1. "+ Oluştur/Al" seçeneğine tıklayın
    1. Gizli dizi için bir ad oluşturun, bu örnekte gizli dizi "test" olarak ad
    1. Gizli anahtar için bir değer oluşturun, bu örnekte "password123" değerini ayarlayacağım

Artık yerel makinenizden kodu çalıştırdığınızda, istemci KIMLIĞINI ve sertifikaya bir yolu sunarak bir erişim belirteci alarak anahtar kasasında kimlik doğrulaması yapabilirsiniz.

### <a name="part-4-retrieve-the-secret-from-your-azure-key-vault-in-an-application-python"></a>4. Bölüm: bir uygulamadaki (Python) Azure Key Vault gizli dizi alma

Uygulamanızın yapılandırdığınız hizmet sorumlusunu kullanarak anahtar kasasından gizli dizi alıp almamadığını test etmek için aşağıdaki kod örneğini kullanın. 

```python
from azure.keyvault.secrets import SecretClient
from azure.identity import CertificateCredential


tenant_id = ""                                             ##ENTER AZURE TENANT ID
vault_url = "https://{VAULT NAME}.vault.azure.net/"        ##ENTER THE URL OF YOUR KEY VAULT
client_id = ""                                             ##ENTER CLIENT ID OF SERVICE PRINCIPAL
cert_path = r"C:\Users\{USERNAME}\{PATH}\{CERT_NAME}.pem"  ##ENTER PATH TO CERTIFICATE

def main():

    #AUTHENTICATION TO AAD USING CLIENT ID AND CLIENT CERTIFICATE
    token = CertificateCredential(tenant_id= tenant_id, client_id=client_id, certificate_path=cert_path)

    #AUTHENTICATION TO KEY VAULT PRESENTING AAD TOKEN
    client = SecretClient(vault_url=vault_url, credential=token)

    #CALL TO KEY VAULT TO GET SECRET
    secret = client.get_secret('{SECRET_NAME}')            ##ENTER NAME OF SECRET IN KEY VAULT

    #GET PLAINTEXT OF SECRET
    print(secret.value)

#CALL MAIN()
if __name__ == "__main__":
    main()
```

![GÖRÜNTÜ](../media/authentication-3.png)


## <a name="next-steps"></a>Sonraki Adımlar

1. Anahtar Kasası kimlik doğrulama hatalarıyla ilgili sorunları nasıl giderebileceğinizi öğrenin. [Key Vault sorun giderme kılavuzu](https://docs.microsoft.com/azure/key-vault/general/rest-error-codes)
