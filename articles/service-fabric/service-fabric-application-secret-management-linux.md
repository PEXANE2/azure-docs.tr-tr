---
title: Linux kümelerinde şifreleme sertifikası ayarlama
description: Bir şifreleme sertifikası nı nasıl ayarlayıp Linux kümelerinde sırları şifreleyin öğrenin.
author: shsha
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: b8e0a19e3f654fc561e7c7e26c6a2da463e24d5f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969025"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Linux kümelerinde şifreleme sertifikası ayarlayın ve sırları şifreleyin
Bu makalede, bir şifreleme sertifikası nın nasıl ayarlanıp Linux kümelerinde sırları şifrelemek için nasıl kullanılacağı gösterilmektedir. Windows kümeleri için [bkz.][secret-management-windows-specific-link]

## <a name="obtain-a-data-encipherment-certificate"></a>Veri çözme sertifikası alma
Veri çözme sertifikası, bir hizmetin ServiceManifest.xml'indeki Ayarlar.xml ve [ortam değişkenlerinde][environment-variables-link] [parametrelerin][parameters-link] şifresini şifrelemek ve çözmek için kullanılır. Kimlik doğrulama veya şifreleme metninin imzalanması için kullanılmaz. Sertifika aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika özel bir anahtar içermelidir.
* Sertifika anahtar kullanımı Veri Şifreleme (10) içermelidir ve Sunucu Kimlik Doğrulaması veya İstemci Kimlik Doğrulaması içermemelidir.

  Örneğin, OpenSSL kullanarak gerekli sertifikayı oluşturmak için aşağıdaki komutlar kullanılabilir:
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Sertifikayı kümenize yükleme
Sertifika, kümedeki her düğüme . `/var/lib/sfcerts` Hizmetin çalıştırıldığı kullanıcı hesabının (varsayılan olarak sfuser) yüklü sertifikaya **erişimi okuması gerekir** (diğer bir şekilde geçerli `/var/lib/sfcerts/TestCert.pem` örnek).

## <a name="encrypt-secrets"></a>Gizli dizileri şifreleme
Aşağıdaki parçacık bir sırrı şifrelemek için kullanılabilir. Bu parçacık yalnızca değeri şifreler; şifreleme metnini **imzalamaz.** Gizli değerler için şifreleme metni oluşturmak için kümenizde yüklenen aynı şifreleme sertifikasını **kullanmanız gerekir.**

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt | tr -d '\n' > plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
Elde edilen base-64 şifreli string çıkışı encrypt.txt hem gizli şifreleme metni ni hem de şifrelemek için kullanılan sertifika hakkında bilgi içerir. OpenSSL ile şifresini çözerek geçerliliğini doğrulayabilirsiniz.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Sonraki adımlar
Bir uygulamada şifrelenmiş sırları nasıl [belirtin öğrenin.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
