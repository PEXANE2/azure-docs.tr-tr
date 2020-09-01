---
title: Azure kanıtlama ilkesini yazma ve imzalama
description: Bir kanıtlama ilkesini yazmak ve imzalamak için bir açıklama.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: fe901960c08c83484d9446c9030a273fa185b456
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89237354"
---
# <a name="how-to-author-and-sign-an-attestation-policy"></a>Kanıtlama ilkesi yazma ve imzalama

Kanıtlama ilkesi, Microsoft Azure kanıtlama için karşıya yüklenen bir dosyadır. Azure kanıtlama, bir ilkeyi kanıtlamayı özgü bir ilke biçiminde karşıya yükleme esnekliği sunar. Alternatif olarak, JSON Web Imzasında, ilkenin kodlanmış bir sürümü de karşıya yüklenebilir. İlke Yöneticisi, kanıtlama ilkesini yazmadan sorumludur. Çoğu kanıtlama senaryosunda, bağlı olan taraf ilke Yöneticisi olarak görev yapar. Kanıtlama çağrısını yapan istemci, hizmetin gelen taleplere (özellikler, değer kümesi) ayrıştırdığı ve dönüştürdüğü kanıtlama kanıtını gönderir. Daha sonra hizmet, ilkede tanımlandığına göre talepleri işler ve hesaplanan sonucu döndürür.

İlke, yetkilendirme ölçütlerini, özelliklerini ve kanıtlama belirtecinin içeriğini belirleme kuralları içerir. Örnek ilke dosyası aşağıda gösterildiği gibi görünür:

```
version=1.0;
authorizationrules
{
   c:[type="secureBootEnables", issuer=="AttestationService"]=> permit()
};

issuancerules
{
  c:[type="secureBootEnables", issuer=="AttestationService"]=> issue(claim=c)
  c:[type="notSafeMode", issuer=="AttestationService"]=> issue(claim=c)
};
```
 
İlke dosyası, yukarıda görüldüğü gibi üç parçaya sahiptir:

- **Sürüm**: sürüm, izlenen dilbilgisinin sürüm numarasıdır. 

    ```
    version=MajorVersion.MinorVersion   
    ```

    Şu anda desteklenen tek sürüm 1,0 sürümüdür.

- **authorizationrules**: Azure kanıtlama 'nın **ıssuancerules**'e ilerlemeniz gerekip gerekmediğini belirlemek için, ilk olarak denetlenecek talep kuralları koleksiyonu. Talep kuralları tanımlandıkları sırada geçerlidir.

- **ıssuancerules**: ilkede tanımlanan kanıtlama sonucuna ek bilgi eklemek için değerlendirilecek bir talep kuralları koleksiyonu. Talep kuralları tanımlandıkları sırada uygulanır ve ayrıca isteğe bağlıdır.

Daha fazla bilgi için bkz. [talep ve talep kuralları](claim-rule-grammar.md) .
   
## <a name="drafting-the-policy-file"></a>İlke dosyasını taslak

1. Yeni bir dosya oluşturun.
1. Sürüme dosya ekleyin.
1. **Authorizationrules** ve **ıssuancerules**için bölüm ekleyin.

  ```
  version=1.0;
  authorizationrules={
  =>deny();
  };
  
  issuancerules={
  };
  ```

  Yetkilendirme kuralları, hiçbir verme kuralı işlenmediğinden emin olmak için herhangi bir koşul olmadan Deny () eylemini içerir. Alternatif olarak, yetkilendirme kuralı verme kurallarının işlenmesine izin vermek için izin ver () eylemini de içerebilir.
  
4. Yetkilendirme kurallarına talep kuralları ekleme

  ```
  version=1.0;
  authorizationrules={
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules={
  };
  ```

  Gelen talep kümesi, tür, değer ve veren ile eşleşen bir talep içeriyorsa, Permit () eylemi ilke altyapısına **ıssuancerules**'yi işlemesini söyler.
  
5. **Issuancerules**'e talep kuralları ekleyin.

  ```
  version=1.0;
  authorizationrules={
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules={
  => issue(type="SecurityLevelValue", value=100);
  };
  ```
  
  Giden talep kümesinde şu bir talep bulunur:

  ```
  [type="SecurityLevelValue", value=100, valueType="Integer", issuer="AttestationPolicy"]
  ```

  Karmaşık ilkeler benzer bir şekilde oluşturulabilir. Daha fazla bilgi için bkz. [kanıtlama ilkesi örnekleri](policy-examples.md).
  
6. Dosyayı kaydedin.

## <a name="creating-the-policy-file-in-json-web-signature-format"></a>JSON Web Imza biçiminde ilke dosyası oluşturma

Bir ilke dosyası oluşturduktan sonra, bir ilkeyi JWS biçiminde karşıya yüklemek için aşağıdaki adımları izleyin.

1. Yük olarak ilkeyle (UTF-8 kodlu) JWS, RFC 7515 oluşturun
     - Base64Url kodlamalı ilke için yük tanımlayıcısı "AttestationPolicy" olmalıdır.
     
     Örnek JWT:
     ```
     Header: {"alg":"none"}
     Payload: {"AttestationPolicy":" Base64Url (policy)"}
     Signature: {}

     JWS format: eyJhbGciOiJub25lIn0.XXXXXXXXX.
     ```

2. Seçim İlkeyi imzalayın. Azure kanıtlama aşağıdaki algoritmaları destekler:
     - **Hiçbiri**: ilke yükünü imzamayın.
     - **RS256**: ilke yükünü imzalamak için desteklenen algoritma

3. JWS 'yi karşıya yükleyin ve ilkeyi doğrulayın.
     - İlke dosyası sözdizimi hatalarından muaf değilse, ilke dosyası hizmet tarafından kabul edilir.
     - İlke dosyası sözdizimi hataları içeriyorsa, ilke dosyası hizmet tarafından reddedilir.

## <a name="signing-the-policy"></a>İlke imzalanıyor

Aşağıda, ilke imzalama işleminin nasıl gerçekleştirileceği hakkında örnek bir Python betiği verilmiştir

```python
from OpenSSL import crypto
import jwt
import getpass
       
def cert_to_b64(cert):
              cert_pem = crypto.dump_certificate(crypto.FILETYPE_PEM, cert)
              cert_pem_str = cert_pem.decode('utf-8')
              return ''.join(cert_pem_str.split('\n')[1:-2])
       
print("Provide the path to the PKCS12 file:")
pkcs12_path = str(input())
pkcs12_password = getpass.getpass("\nProvide the password for the PKCS12 file:\n")
pkcs12_bin = open(pkcs12_path, "rb").read()
pkcs12 = crypto.load_pkcs12(pkcs12_bin, pkcs12_password.encode('utf8'))
ca_chain = pkcs12.get_ca_certificates()
ca_chain_b64 = []
for chain_cert in ca_chain:
   ca_chain_b64.append(cert_to_b64(chain_cert))
   signing_cert_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM, pkcs12.get_privatekey())
signing_cert_b64 = cert_to_b64(pkcs12.get_certificate())
ca_chain_b64.insert(0, signing_cert_b64)

print("Provide the path to the policy text file:")
policy_path = str(input())
policy_text = open(policy_path, "r").read()
encoded = jwt.encode({'text': policy_text }, signing_cert_pkey, algorithm='RS256', headers={'x5c' : ca_chain_b64})
print("\nAttestation Policy JWS:")
print(encoded.decode('utf-8'))
```

## <a name="next-steps"></a>Sonraki adımlar
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)
- [Kod örneklerini kullanarak bir SGX kuşatma atturuntest](https://docs.microsoft.com/samples/browse/?expanded=azure&terms=attestation)
