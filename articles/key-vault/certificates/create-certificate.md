---
title: Sertifika oluşturma yöntemleri
description: Key Vault'ta sertifika oluşturmanın yolları.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 21bcaf8636638153ad5e5659ef3e1bff0631d9b8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430961"
---
# <a name="certificate-creation-methods"></a>Sertifika oluşturma yöntemleri

 Anahtar Kasa (KV) sertifikası oluşturulabilir veya anahtar kasasına aktarılabilir. KV sertifikası oluşturulduğunda, anahtar kasasının içinde özel anahtar oluşturulur ve sertifika sahibine asla maruz kalmaz. Anahtar Kasa'da sertifika oluşturmanın yolları şunlardır:  

-   **Kendi imzalı bir sertifika oluşturun:** Bu, bir ortak-özel anahtar çifti oluşturur ve bir sertifika ile ilişkilendirmek. Sertifika kendi anahtarı yla imzalanır.  

-    **El ile yeni bir sertifika oluşturun:** Bu, genel-özel anahtar çifti oluşturur ve bir X.509 sertifika imzalama isteği oluşturur. İmza isteği kayıt yetkilisiniz veya sertifika yetkilisi niz tarafından imzalanabilir. İmzalı x509 sertifikası, Anahtar Kasa'daki KV sertifikasını tamamlamak için bekleyen anahtar çifti ile birleştirilebilir. Bu yöntem daha fazla adım gerektirmesine rağmen, özel anahtar Anahtar Kasası'nda oluşturulduğundan ve anahtar kasasıyla sınırlı olduğundan size daha fazla güvenlik sağlar. Bu aşağıdaki diyagramda açıklanmıştır.  

![Kendi sertifika yetkinizle sertifika oluşturma](../media/certificate-authority-1.png)  

Aşağıdaki açıklamalar, önceki diyagramdaki yeşil harfli adımlara karşılık gelir.

1. Yukarıdaki diyagramda, uygulamanız anahtar kasanızda bir anahtar oluşturarak dahili olarak başlayan bir sertifika oluşturmaktadır.
2. Key Vault başvurunuza sertifika imzalama isteği (CSR) döner
3. Uygulamanız CSR'yi seçtiğiniz CA'ya geçirir.
4. Seçtiğiniz CA, X509 Sertifikası ile yanıt verir.
5. Başvurunuz, ca'nızdan X509 Sertifikası'nın birleşmesi ile yeni sertifika oluşturmayı tamamlar.

-   **Bilinen bir veren sağlayıcıile sertifika oluşturun:** Bu yöntem, bir veren nesnesi oluşturma bir kerelik görev yapmak gerektirir. Anahtar kasanızda bir ihraççı nesnesi oluşturulduktan sonra, adı KV sertifikası ilkesinde başvurulabilir. Böyle bir KV sertifikası oluşturmak için bir istek kasada anahtar çifti oluşturmak ve x509 sertifikası almak için başvurulan veren nesnedeki bilgileri kullanarak veren sağlayıcı hizmeti ile iletişim kurar. x509 sertifikası veren hizmetten alınır ve KV sertifikası oluşturmayı tamamlamak için anahtar çifti ile birleştirilir.  

![Key Vault ortak sertifika yetkilisi ne sahip bir sertifika oluşturma](../media/certificate-authority-2.png)  

Aşağıdaki açıklamalar, önceki diyagramdaki yeşil harfli adımlara karşılık gelir.

1. Yukarıdaki diyagramda, uygulamanız anahtar kasanızda bir anahtar oluşturarak dahili olarak başlayan bir sertifika oluşturmaktadır.
2. Key Vault CA'ya TLS/SSL Sertifika İsteği gönderir.
3. Uygulamanız bir döngü ve bekleme sürecinde sertifikanın tamamlanıp tamamlanmadığını öğrenmek için Anahtar Kasanızı sorgular. Anahtar Kasası x509 sertifikasıyla CA yanıtını aldığında sertifika oluşturma işlemi tamamlanır.
4. CA, Key Vault'un TLS/SSL Sertifika İsteğine TLS/SSL X.509 sertifikası ile yanıt verir.
5. Yeni sertifika oluşturma nız, CA için TLS/SSL X.509 sertifikasının birleşmesi ile tamamlar.

## <a name="asynchronous-process"></a>Asenkron süreç
KV sertifika oluşturma asynchronous bir işlemdir. Bu işlem bir KV sertifika isteği oluşturur ve 202 (Kabul) http durum kodunu döndürür. İsteğin durumu, bu işlem tarafından oluşturulan bekleyen nesneyi yoklayarak izlenebilir. Bekleyen nesnenin tam URI'si KONUM üstbilgisinde döndürülür.  

KV sertifikası oluşturma isteği tamamlandığında, bekleyen nesnenin durumu "devam ediyor"dan "tamamlanmış" olarak değiştirilir ve KV sertifikasının yeni bir sürümü oluşturulur. Bu geçerli sürümü olacak.  

## <a name="first-creation"></a>İlk oluşturma
 Bir KV sertifikası ilk kez oluşturulduğunda, adreslenebilir bir anahtar ve gizli de sertifikanın ki ile aynı ada sahip oluşturulur. Ad zaten kullanılıyorsa, işlem 409 (çakışma) http durum koduyla başarısız olur.
Adreslenebilir anahtar ve gizli KV sertifika öznitelikleri kendi öznitelikleri ni almak. Bu şekilde oluşturulan adreslenebilir anahtar ve gizli, yaşam süresi Key Vault tarafından yönetilen yönetilen anahtarlar ve sırlar olarak işaretlenir. Yönetilen anahtarlar ve sırlar salt okunur. Not: Bir KV sertifikasının süresi dolduğunda veya devre dışı bırakılırsa, ilgili anahtar ve gizli çalışamaz hale gelir.  

 Bu, KV sertifikası oluşturmak için ilk işlemse, bir ilke gerekir.  İlke, ilke kaynağını değiştirmek için birbirini izleyen oluşturma işlemleriyle de sağlanabilir. Bir ilke sağlanmıyorsa, hizmetteki ilke kaynağı KV sertifikasının bir sonraki sürümünü oluşturmak için kullanılır. Bir sonraki sürüm oluşturma isteği devam ederken, geçerli KV sertifikası ve karşılık gelen adreslenebilir anahtar ve gizli değişmeden kalır unutmayın.  

## <a name="self-issued-certificate"></a>Kendi kendine verilen sertifika
 Kendi kendine verilen bir sertifika oluşturmak için, sertifika ilkesinden aşağıdaki snippet'te gösterildiği gibi, verenin adını sertifika ilkesinde "Self" olarak ayarlayın.  

```  
"issuer": {  
       "name": "Self"  
    }  

```  

 İhraççı adı belirtilmemişse, verenin adı "Bilinmiyor" olarak ayarlanır. İhraççı "Bilinmiyor" olduğunda, sertifika sahibinin kendi seçtiği kuruluştan x509 sertifikasını el ile alması, ardından sertifika oluşturmayı tamamlamak için bekleyen anahtar kasa sertifikası ile ortak x509 sertifikasını birleştirmesi gerekir.

```  
"issuer": {  
       "name": "Unknown"  
    }  

```  

## <a name="partnered-ca-providers"></a>Ortak CA Sağlayıcıları
Sertifika oluşturma el ile veya bir "Self" veren kullanılarak tamamlanabilir. Key Vault, sertifikaların oluşturulmasını kolaylaştırmak için belirli ihraççı sağlayıcılarla da iş ortağıdır. Bu ortak ihraççı sağlayıcılarla birlikte anahtar kasası için aşağıdaki sertifika türleri sipariş edilebilir.  

|Sağlayıcı|Sertifika türü|  
|--------------|----------------------|  
|DigiCert|Key Vault DigiCert ile OV veya EV SSL sertifikaları sunuyor|
|Globalsign|Key Vault GlobalSign ile OV veya EV SSL sertifikaları sunar|

 Sertifika veren kuruluş, Azure Key Vault'ta (KV) Sertifika Veren kaynağı olarak temsil edilen bir varlıktır. Bir KV sertifikasının kaynağı hakkında bilgi sağlamak için kullanılır; verenin adı, sağlayıcı, kimlik bilgileri ve diğer yönetim ayrıntıları.

İhraççı sağlayıcıya bir sipariş verildiğinde, sertifika türüne bağlı olarak x509 sertifika uzantılarını ve sertifika geçerlilik süresini onurlandırabileceğini veya geçersiz kılabilir.  

 Yetkilendirme: Sertifikalar/oluşturma izni gerektirir.

## <a name="see-also"></a>Ayrıca Bkz.

 - [Sertifika oluşturmayı izleme ve yönetme](create-certificate-scenarios.md)
