---
title: Windows kümelerinde şifreleme sertifikası ayarlama
description: Windows kümelerinde şifreleme sertifikası ayarlama ve gizli dizileri şifreleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/04/2019
ms.openlocfilehash: eb4909d62a2627c368f24dab572b25c6f1df30ec
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583280"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-windows-clusters"></a>Windows kümelerinde şifreleme sertifikası ayarlama ve gizli dizileri şifreleme
Bu makalede, bir şifreleme sertifikasının nasıl ayarlanacağı ve Windows kümelerinde gizli dizileri şifrelemek için nasıl kullanılacağı gösterilmektedir. Linux kümeleri için bkz [. bir şifreleme sertifikası ayarlama ve Linux kümelerinde gizli dizileri şifreleme.][secret-management-linux-specific-link]

[Azure Key Vault][key-vault-get-started] , sertifikalar için güvenli bir depolama konumu olarak ve Azure 'daki Service Fabric kümelerine yüklü sertifikaları almanın bir yolu olarak kullanılır. Azure 'a dağıtmayın, Service Fabric uygulamalardaki gizli dizileri yönetmek için Key Vault kullanmanız gerekmez. Ancak, bir uygulamadaki gizli dizileri *kullanmak* , uygulamaların herhangi bir yerde barındırılan bir kümeye dağıtılmasına olanak tanımak için bulut platformudur. 

## <a name="obtain-a-data-encipherment-certificate"></a>Veri şifreleme sertifikası alma
Bir veri şifreleme sertifikası, bir hizmetin ServiceManifest. xml dosyasındaki bir hizmetin Settings. xml ve [ortam değişkenlerinde][environment-variables-link] [parametrelerin][parameters-link] şifrelenmesi ve şifresinin çözülmesi için kesinlikle kullanılır. Şifre metninin kimlik doğrulaması veya imzalanması için kullanılmaz. Sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika bir özel anahtar içermelidir.
* Sertifika, anahtar değişimi için, kişisel bilgi değişimi (. pfx) dosyasına verilebilir şekilde oluşturulmalıdır.
* Sertifika anahtarı kullanımı, veri şifreleme (10) içermelidir ve sunucu kimlik doğrulaması veya Istemci kimlik doğrulaması içermemelidir. 
  
  Örneğin, PowerShell kullanarak kendinden imzalı bir sertifika oluştururken, `KeyUsage` bayrağın şu şekilde `DataEncipherment`ayarlanması gerekir:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Sertifikayı kümenize yükler
Bu sertifikanın kümedeki her düğüme yüklenmesi gerekir. Kurulum yönergeleri için [Azure Resource Manager kullanarak küme oluşturma][service-fabric-cluster-creation-via-arm] bölümüne bakın. 

## <a name="encrypt-application-secrets"></a>Uygulama gizli dizilerini şifreleyin
Bir gizli dizi şifrelemek için aşağıdaki PowerShell komutu kullanılır. Bu komut yalnızca değeri şifreler; şifre metnini **imzalamaz** . Gizli değerler için şifreli değerleri oluşturmak üzere kümenize yüklenmiş aynı şifreleme sertifikasını kullanmanız gerekir:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Elde edilen temel 64 kodlu dize hem gizli şifreli metin hem de bunu şifrelemek için kullanılan sertifikayla ilgili bilgileri içerir.

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulamada şifrelenmiş gizli dizileri belirtmeyi öğrenin.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[key-vault-get-started]:../key-vault/general/overview.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
