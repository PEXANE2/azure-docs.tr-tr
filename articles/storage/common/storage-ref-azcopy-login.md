---
title: azcopy giriş | Microsoft Dokümanlar
description: Bu makalede, azcopy giriş komutu için referans bilgileri verilmektedir.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d07d1a706635a7f269a9a51769ae6f8bbf57df3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295399"
---
# <a name="azcopy-login"></a>azcopy oturum açma

Azure Depolama kaynaklarına erişmek için Azure Etkin Dizini'nde oturum açın.

## <a name="synopsis"></a>Özet

Azure Depolama kaynaklarına erişmek için Azure Etkin Dizini'nde oturum açın.

Azure Depolama hesabınıza yetki verilabilmesi için, Depolama hesabı, ana kaynak grubu veya üst abonelik bağlamında Kullanıcı hesabınıza **Depolama Blob Veri Katılımcısı** rolünü atamanız gerekir.

Bu komut, işletim sistemi yerleşik mekanizmalarını kullanarak geçerli kullanıcı için şifrelenmiş giriş bilgilerini önbelleğe alacaktır.

Daha fazla bilgi için lütfen örneklere bakın.

> [!IMPORTANT]
> Komut satırını kullanarak bir ortam değişkeni ayarlarsanız, bu değişken komut satırı geçmişinizde okunabilir. Komut satırı geçmişinizden kimlik bilgilerini içeren değişkenleri temizlemeyi düşünün. Değişkenlerin geçmişinizde görünmesini engellemek için, kullanıcıdan kimlik bilgilerini almak ve ortam değişkenini ayarlamak için bir komut dosyası kullanabilirsiniz.

```azcopy
azcopy login [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

Varsayılan AAD kiracı kimliği ortak olarak ayarlanmış ile etkileşimli giriş yapın:

```azcopy
azcopy login
```

Belirtilen kiracı kimliğiyle etkileşimli olarak oturum açın:

```azcopy
azcopy login --tenant-id "[TenantID]"
```

Sanal Makinenin (VM) sistem tarafından atanan kimliğini kullanarak oturum açın:

```azcopy
azcopy login --identity
```

Bir VM'nin kullanıcı tarafından atanan kimliğini ve hizmet kimliğinin İstemci Kimliğini kullanarak oturum açın:

```azcopy
azcopy login --identity --identity-client-id "[ServiceIdentityClientID]"
```

Bir VM'nin kullanıcı tarafından atanan kimliğini ve hizmet kimliğinin Nesne Kimliğini kullanarak oturum açın:

```azcopy
azcopy login --identity --identity-object-id "[ServiceIdentityObjectID]"
```

Bir VM'nin kullanıcı tarafından atanan kimliğini ve hizmet kimliğinin Kaynak Kimliğini kullanarak oturum açın:

```azcopy
azcopy login --identity --identity-resource-id "/subscriptions/<subscriptionId>/resourcegroups/myRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID"
```

İstemci sırrını kullanarak servis sorumlusu olarak oturum açın. Gizli tabanlı hizmet ana auth için istemci gizli AZCOPY_SPA_CLIENT_SECRET ortam değişkeni ayarlayın.

```azcopy
azcopy login --service-principal
```

Sertifika ve parola kullanarak hizmet sorumlusu olarak giriş yapın. Cert tabanlı hizmet temel yetkilendirmesi için ortam değişkenini AZCOPY_SPA_CERT_PASSWORD sertifikanın parolasına ayarlayın.

```azcopy
azcopy login --service-principal --certificate-path /path/to/my/cert
```

PeM veya PKCS12 dosyasına giden bir yol olarak /path/to/my/cert'i ele aldığından emin olun. AzCopy sertifikanızı almak için sistem sertifika deposuna ulaşmaz.

--sertifika tabanlı hizmet ana para sıfatı yapılırken sertifika-yol zorunludur.

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--aad uç noktası|Kullanılacak Azure Etkin Dizin bitiş noktası. Varsayılan (`https://login.microsoftonline.com`) herkese açık Azure bulutu için doğrudur. Ulusal bir bulutta kimlik doğrulaması yaparken bu parametreyi ayarlayın. Bkz. [Azure AD kimlik doğrulama bitiş noktaları.](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints)
Yönetilen Hizmet Kimliği için bu bayrak gerekmez.|
|--uygulama-id dizesi|Kullanıcı tarafından atanan kimliğin başvuru kimliği. Hizmet müdürü auth için gereklidir.|
|--sertifika-yol dizesi|SPN kimlik doğrulaması için sertifikaya giden yol. Sertifika tabanlı hizmet ana para sıfatı için gereklidir.|
|-h, --yardım|Giriş komutu için yardım içeriğini göster.|
|--kimlik|yönetilen hizmet kimliği (MSI) olarak da bilinen sanal makinenin kimliğini kullanarak oturum açın.|
|--kimlik-istemci-kimlik dizesi|Kullanıcı tarafından atanan kimliğin istemci kimliği.|
|--kimlik-nesne-id dizesi|Kullanıcı tarafından atanan kimliğin nesne kimliği.|
|--kimlik-kaynak-id dizesi|Kullanıcı tarafından atanan kimliğin kaynak kimliği.|
|--hizmet-anapara|Bir sertifika veya gizli kullanarak SPN (Hizmet Ana Adı) üzerinden giriş yapın. İstemci gizli veya sertifika parolası uygun ortam değişkenine yerleştirilmelidir. Çevre `AzCopy env` değişkenlerinin adlarını ve açıklamalarını görmek için yazın.|
|--kiracı-id dizesi| OAuth aygıt etkileşimli giriş için kullanılacak Azure etkin dizin kiracı kimliği.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
