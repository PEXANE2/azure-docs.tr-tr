---
title: AzCopy oturum açma | Microsoft Docs
description: Bu makale, AzCopy login komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 2938d85becbea738acc21fc7b15991301eef759f
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195730"
---
# <a name="azcopy-login"></a>AzCopy oturum açma

Azure depolama kaynaklarına erişmek için Azure Active Directory 'de oturum açar.

## <a name="synopsis"></a>Özeti

Azure depolama kaynaklarına erişmek için Azure Active Directory oturum açın.

Azure depolama hesabınıza yetki vermek için, depolama alanı, üst kaynak grubu veya üst abonelik bağlamında, Kullanıcı hesabınıza **Depolama Blobu veri katılımcısı** rolünü atamanız gerekir.

Bu komut, işletim sistemi yerleşik mekanizmalarını kullanarak geçerli kullanıcı için şifrelenmiş oturum açma bilgilerini önbelleğe alacak.

Daha fazla bilgi için lütfen örneklere bakın.

> [!IMPORTANT]
> Bir ortam değişkenini komut satırını kullanarak ayarlarsanız, bu değişken komut satırı geçmişinizde okunabilir olacaktır. Komut satırı geçmişinizden kimlik bilgilerini içeren değişkenleri temizlemeyi göz önünde bulundurun. Değişkenlerin geçmişinizde görünmesini önlemek için, kullanıcıya kimlik bilgilerini istemek ve ortam değişkenini ayarlamak için bir komut dosyası kullanabilirsiniz.

```azcopy
azcopy login [flags]
```

## <a name="examples"></a>Örnekler

Varsayılan AAD kiracı KIMLIĞI ortak olarak ayarlanan şekilde etkileşimli olarak oturum açın:

```azcopy
azcopy login
```

Belirtilen kiracı KIMLIĞIYLE etkileşimli olarak oturum açın:

```azcopy
azcopy login --tenant-id "[TenantID]"
```

Bir VM 'nin sistem tarafından atanan kimliğini kullanarak oturum açın:

```azcopy
azcopy login --identity
```

VM 'nin Kullanıcı tarafından atanan kimliğini hizmet kimliğinin istemci KIMLIĞIYLE kullanarak oturum açın:

```azcopy
azcopy login --identity --identity-client-id "[ServiceIdentityClientID]"
```

VM 'nin Kullanıcı tarafından atanan kimliğini hizmet kimliğinin nesne KIMLIĞIYLE oturum açın:

```azcopy
azcopy login --identity --identity-object-id "[ServiceIdentityObjectID]"
```

VM 'nin Kullanıcı tarafından atanan kimliğini hizmet kimliğinin kaynak KIMLIĞIYLE oturum açın:

```azcopy
azcopy login --identity --identity-resource-id "/subscriptions/<subscriptionId>/resourcegroups/myRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID"
```

İstemci parolasını kullanarak hizmet sorumlusu olarak oturum açın. AZCOPY_SPA_CLIENT_SECRET ortam değişkenini gizli tabanlı hizmet sorumlusu kimlik doğrulaması için istemci parolası olarak ayarlayın.

```azcopy
azcopy login --service-principal
```

Bir sertifika ve parola kullanarak hizmet sorumlusu olarak oturum açın. AZCOPY_SPA_CERT_PASSWORD ortam değişkenini, sertifika tabanlı hizmet sorumlusu yetkilendirmesi için sertifikanın parolasıyla ayarlayın.

```azcopy
azcopy login --service-principal --certificate-path /path/to/my/cert
```

/Path/to/My/CERT ' i bir PEM veya PKCS12 dosyasının yolu olarak kabul ettiğinizden emin olun. AzCopy, sertifikanızı almak için sistem sertifika deposuna ulaşmıyor.

Sertifika tabanlı hizmet sorumlusu kimlik doğrulaması yapılırken sertifika-yol zorunludur.

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--Uygulama kimliği dizesi|Kullanıcı tarafından atanan kimliğin uygulama KIMLIĞI. Hizmet sorumlusu kimlik doğrulaması için gereklidir.|
|--Sertifika-yol dizesi|SPN kimlik doğrulaması için sertifika yolu. Sertifika tabanlı hizmet sorumlusu kimlik doğrulaması için gereklidir.|
|-h,--yardım|Oturum açma komutu için yardım içeriğini göster.|
|--kimlik|yönetilen hizmet kimliği (MSI) olarak da bilinen sanal makinenin kimliğini kullanarak oturum açın.|
|--Identity-Client-id dizesi|Kullanıcı tarafından atanan kimliğin istemci KIMLIĞI.|
|--Identity-Object-id dizesi|Kullanıcı tarafından atanan kimliğin nesne KIMLIĞI.|
|--Identity-Resource-id dizesi|Kullanıcı tarafından atanan kimliğin kaynak KIMLIĞI.|
|--hizmet-asıl|Bir sertifika veya gizli dizi kullanarak SPN (hizmet sorumlusu adı) aracılığıyla oturum açın. İstemci parolası veya sertifika parolasının uygun ortam değişkenine yerleştirilmesi gerekir. Ortam `AzCopy env` değişkenlerinin adlarını ve açıklamalarını görmek için yazın.|
|--Kiracı kimliği dizesi| OAuth cihaz etkileşimli oturum açma için kullanılacak Azure Active Directory kiracı KIMLIĞI.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
