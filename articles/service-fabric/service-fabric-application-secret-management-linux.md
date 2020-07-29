---
title: Linux kümelerinde bir şifreleme sertifikası ayarlama
description: Bir şifreleme sertifikası ayarlamayı ve Linux kümelerinde gizli dizileri şifrelemeyi öğrenin.
author: shsha
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: b8e0a19e3f654fc561e7c7e26c6a2da463e24d5f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78969025"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Bir şifreleme sertifikası ayarlama ve Linux kümelerinde gizli dizileri şifreleme
Bu makalede, bir şifreleme sertifikasının nasıl ayarlanacağı ve Linux kümelerinde gizli dizileri şifrelemek için nasıl kullanılacağı gösterilmektedir. Windows kümeleri için bkz. [şifreleme sertifikası ayarlama ve Windows kümelerinde gizli dizileri şifreleme][secret-management-windows-specific-link].

## <a name="obtain-a-data-encipherment-certificate"></a>Veri şifreleme sertifikası alma
Bir veri şifreleme sertifikası, bir hizmetin Settings.xml ve bir hizmetin ServiceManifest.xml [ortam değişkenlerinde][environment-variables-link] [parametrelerin][parameters-link] şifrelenmesi ve şifresinin çözülmesi için kesinlikle kullanılır. Şifre metninin kimlik doğrulaması veya imzalanması için kullanılmaz. Sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika bir özel anahtar içermelidir.
* Sertifika anahtarı kullanımı, veri şifreleme (10) içermelidir ve sunucu kimlik doğrulaması veya Istemci kimlik doğrulaması içermemelidir.

  Örneğin, aşağıdaki komutlar OpenSSL kullanarak gerekli sertifikayı oluşturmak için kullanılabilir:
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Sertifikayı kümenize yükler
Sertifikanın altındaki kümedeki her bir düğüme yüklenmesi gerekir `/var/lib/sfcerts` . Hizmetin üzerinde çalıştığı kullanıcı hesabı (varsayılan olarak, sfuser) yüklü sertifikaya (geçerli örnek için) **okuma erişimine sahip olmalıdır** `/var/lib/sfcerts/TestCert.pem` .

## <a name="encrypt-secrets"></a>Gizli dizileri şifreleme
Aşağıdaki kod parçacığı bir gizli dizi şifrelemek için kullanılabilir. Bu kod parçacığı yalnızca değeri şifreler; şifre metnini **imzalamaz** . Gizli değerler için şifreli değerleri oluşturmak üzere kümenize yüklenmiş aynı şifreleme sertifikasını **kullanmanız gerekir** .

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt | tr -d '\n' > plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
encrypted.txt için elde edilen temel 64 kodlu dize çıktısı hem gizli bir şifreli metin hem de bunu şifrelemek için kullanılan sertifikayla ilgili bilgiler içerir. OpenSSL ile şifresini çözerek geçerliliğini doğrulayabilirsiniz.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulamada şifrelenmiş gizli dizileri belirtmeyi öğrenin.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
