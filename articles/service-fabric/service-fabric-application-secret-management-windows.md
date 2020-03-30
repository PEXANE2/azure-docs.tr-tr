---
title: Windows kümelerinde şifreleme sertifikası ayarlama
description: Windows kümelerinde şifreleme sertifikası nasıl ayarlayıp sırları şifreleyebilirsiniz öğrenin.
author: vturecek
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: d9413a37be221adc375836719dc1f467a5571fa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610191"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-windows-clusters"></a>Windows kümelerinde şifreleme sertifikası ayarlama ve sırları şifreleme
Bu makalede, bir şifreleme sertifikası nın nasıl ayarlanıp Windows kümelerinde sırları şifrelemek için kullanılacağı gösterilmektedir. Linux kümeleri için [bkz.][secret-management-linux-specific-link]

[Azure Key Vault][key-vault-get-started] burada sertifikalar için güvenli bir depolama konumu ve Azure'daki Hizmet Kumaşı kümelerine sertifika yüklemenin bir yolu olarak kullanılır. Azure'a dağıtım yapmıyorsanız, Service Fabric uygulamalarındaki sırları yönetmek için Key Vault'u kullanmanız gerekmez. Ancak, bir uygulamada sırları *kullanarak* bulut platformu-agnostik uygulamalar her yerde barındırılan bir kümeye dağıtılmak için izin verir. 

## <a name="obtain-a-data-encipherment-certificate"></a>Veri çözme sertifikası alma
Veri çözme sertifikası, bir hizmetin ServiceManifest.xml'indeki Ayarlar.xml ve [ortam değişkenlerinde][environment-variables-link] [parametrelerin][parameters-link] şifresini şifrelemek ve çözmek için kullanılır. Kimlik doğrulama veya şifreleme metninin imzalanması için kullanılmaz. Sertifika aşağıdaki gereksinimleri karşılamalıdır:

* Sertifika özel bir anahtar içermelidir.
* Sertifika anahtar değişimi için oluşturulmalıdır, Kişisel Bilgi Alışverişi (.pfx) dosyasına ihraç edilebilir.
* Sertifika anahtar kullanımı Veri Şifreleme (10) içermelidir ve Sunucu Kimlik Doğrulaması veya İstemci Kimlik Doğrulaması içermemelidir. 
  
  Örneğin, PowerShell kullanarak kendi imzalı bir sertifika `KeyUsage` oluştururken, `DataEncipherment`bayrak aşağıdaki şekilde ayarlanmalıdır:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Sertifikayı kümenize yükleme
Bu sertifika kümedeki her düğüme yüklenmelidir. Kurulum yönergeleri için [Azure Kaynak Yöneticisi'ni kullanarak küme oluşturmayı][service-fabric-cluster-creation-via-arm] görün. 

## <a name="encrypt-application-secrets"></a>Uygulama sırlarını şifreleme
Aşağıdaki PowerShell komutu bir sırrı şifrelemek için kullanılır. Bu komut yalnızca değeri şifreler; şifreleme metnini **imzalamaz.** Gizli değerler için şifreleme metni oluşturmak için kümenizde yüklenen aynı şifreleme sertifikasını kullanmanız gerekir:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Elde edilen base-64 kodlu dize, hem gizli şifreleme metnini hem de şifrelemek için kullanılan sertifika hakkında bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar
Bir uygulamada şifrelenmiş sırları nasıl [belirtin öğrenin.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-overview.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
