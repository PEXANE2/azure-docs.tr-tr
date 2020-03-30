---
title: Key Vault .NET 2.x API Yayın Notları| Microsoft Dokümanlar
description: .NET geliştiricileri bu API'yi Azure Key Vault için kod lamak için kullanır
services: key-vault
author: msmbaldwin
manager: rkarlin
editor: bryanla
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/02/2017
ms.author: mbaldwin
ms.openlocfilehash: 07502b4c4487a7517e2fe73b62eae0a237a6f22b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70883274"
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault .NET 2.0 - Sürüm Notları ve Geçiş Kılavuzu
Aşağıdaki bilgiler, C# ve .NET için Azure Key Vault kitaplığı 2.0 sürümüne geçiş yardımcı olur.  Önceki sürümler için yazılmış uygulamaların en son sürümü desteklemek için güncelleştirilmesi gerekir.  Bu değişiklikler, **Key Vault sertifikaları**gibi yeni ve geliştirilmiş özellikleri tam olarak desteklemek için gereklidir.

## <a name="key-vault-certificates"></a>Anahtar Kasa sertifikaları

Key Vault sertifikaları x509 sertifikalarını yönetir ve aşağıdaki davranışları destekler:  

* Anahtar Kasa oluşturma işlemi yle sertifika lar oluşturun veya varolan sertifikayı içe aktarın. Bu, hem kendi imzalı hem de Sertifika Yetkilisi (CA) tarafından oluşturulan sertifikaları içerir.
* Özel anahtar malzemeyi kullanarak etkileşim olmadan x509 sertifika depolamaalanını güvenli bir şekilde saklayın ve yönetin.  
* Sertifika yaşam döngüsünü yönetmek için Key Vault'u yönlendiren ilkeler tanımlayın.  
* Son kullanma bildirimleri ve yenileme bildirimleri gibi yaşam döngüsü olayları için iletişim bilgileri sağlayın.  
* Seçilen verenlerle sertifikaları otomatik olarak yeniler (Key Vault ortağı X509 sertifika sağlayıcıları ve sertifika yetkilileri).* Alternatif (ortak olmayan) destek sertifikası sağlar ve sertifika yetkilileri (otomatik yenilemeyi desteklemez).  

## <a name="net-support"></a>.NET desteği

* **.NET 4.0,** Azure Key Vault .NET kitaplığın 2.0 sürümü tarafından desteklenmiyor
* **.NET Framework 4.5.2,** Azure Key Vault .NET kitaplığın 2.0 sürümü tarafından desteklenir
* **.NET Standart 1.4,** Azure Key Vault .NET kitaplığın 2.0 sürümü tarafından desteklenir

## <a name="namespaces"></a>Ad Alanları

* **Modeller** için ad alanı **Microsoft.Azure.KeyVault'tan** **Microsoft.Azure.KeyVault.Models'e**değiştirilir.
* **Microsoft.Azure.KeyVault.Internal** ad alanı bırakılır.
* Aşağıdaki Azure SDK bağımlılıkları ad alanları 

    - **Hyak.Common** şimdi **Microsoft.Rest**olduğunu .
    - **Hyak.Common.Internals** artık **Microsoft.Rest.Serialization**olduğunu.

## <a name="type-changes"></a>Tür değişiklikleri

* *Secret* *SecretBundle* olarak değiştirildi
* *Sözlük* *IDictionary* olarak değiştirildi
* *Liste\<T>, string []* *\<IList T>* olarak değiştirildi
* *NextList* *NextPageLink* olarak değiştirildi

## <a name="return-types"></a>İade türleri

* **KeyList** ve **SecretList** şimdi *ListKeysResponseMessage* yerine *IPage\<T>* döndürür
* Oluşturulan **BackupKeyAsync** şimdi *Value* (yedekleme blob) içeren *BackupKeyResult*döndürür. Daha önce, yöntem sarılmış ve sadece değeri döndürüldü.

## <a name="exceptions"></a>Özel durumlar

* *KeyVaultClientException* *KeyVaultErrorException* olarak değiştirilir
* Hizmet hatası *özel durum değiştirildi. *Özel *durum hatası. Body.Error.Message*.
* **[JsonExtensionData]** için hata iletisinden ek bilgi kaldırıldı.

## <a name="constructors"></a>Oluşturucular

* Bir *HttpClient'ı* oluşturucu bağımsız değişken olarak kabul etmek yerine, oluşturucu yalnızca *HttpClientHandler* veya *DelegatingHandler'ı[]* kabul eder.

## <a name="downloaded-packages"></a>İndirilen paketler

Bir istemci Bir Anahtar Kasa sıyrıkını işlerse, aşağıdaki paketler karşıdan yüklenir:

### <a name="previous-package-list"></a>Önceki paket listesi

* `package id="Hyak.Common" version="1.0.2" targetFramework="net45"`
* `package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"`
* `package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"`
* `package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"`
* `package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"`
* `package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"`

### <a name="current-package-list"></a>Geçerli paket listesi

* `package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"`

## <a name="class-changes"></a>Sınıf değişiklikleri

* **UnixEpoch** sınıfı kaldırıldı.
* **Base64UrlConverter** sınıfı **Base64UrlJsonConverter**olarak yeniden adlandırıldı.

## <a name="other-changes"></a>Diğer değişiklikler

* API'nin bu sürümüne geçici hatalarla ilgili KV işlemi yeniden deneme ilkesinin yapılandırmasına destek eklendi.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Bir *kasa*döndüren işlemler için, iade türü **vault** özelliği içeren bir sınıftı. İade türü artık *Vault.*
* *İzinlerToKeys* ve *PermissionsToSecrets* şimdi *Permissions.Keys* ve *Permissions.Secrets* vardır
* Bazı iade türleri değişiklikleri kontrol düzlemi için de geçerlidir.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Paket, şifreleme işlemleri için **Microsoft.Azure.KeyVault.Extensions** ve **Microsoft.Azure.KeyVault.Cryptography'ye** ayrılmıştır.

