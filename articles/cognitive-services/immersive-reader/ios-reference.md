---
title: Modern okuyucu iOS SDK başvurusu
titleSuffix: Azure Cognitive Services
description: Modern okuyucu iOS SDK 'Sı için başvuru
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 08/01/2019
ms.author: metan
ms.openlocfilehash: 615c09dd8a7287918bb009ce11854278b21554c1
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69899409"
---
# <a name="immersive-reader-sdk-reference"></a>Modern Okuyucu SDK başvurusu

Modern okuyucu iOS SDK 'Sı, tam ekran okuyucuyu iOS uygulamanızla tümleştirmenize olanak tanıyan bir Swift CocoaPod.

## <a name="functions"></a>İşlevler

SDK tek bir işlevi `launchImmersiveReader(navController, token, subdomain, content, options, onSuccess, onFailure)`sunar.

### <a name="launchimmersivereader"></a>launchImmersiveReader

İOS uygulamanızda bir görünüm denetleyicisi başlatarak, tam ekran okuyucuyu başlatır.

```swift
public func launchImmersiveReader(navController: UINavigationController, token: String, subdomain: String, content: Content, options: Options?, onSuccess: @escaping () -> Void, onFailure: @escaping (_ error: Error) -> Void)
```

#### <a name="parameters"></a>Parametreler

| Name | Tür | Açıklama |
| ---- | ---- |------------ |
| `navController` | Uıınavigationcontroller | İşlevin çağrıldığı iOS uygulaması için gezinti denetleyicisi. |
| `token` | Dize | Azure AD kimlik doğrulama belirteci. Bkz. [Azure AD kimlik doğrulaması nasıl yapılır](./azure-active-directory-authentication.md). |
| `subdomain` | Dize | Azure 'daki tam ekran okuyucu kaynağınızın özel alt etki alanı. Bkz. [Azure AD kimlik doğrulaması nasıl yapılır](./azure-active-directory-authentication.md). |
| `content` | [İçeriği](#content) | Tam ekran okuyucu 'da gösterilecek içeriği içeren nesne. |
| `options` | [Seçenekler](#options) | Modern okuyucunun belirli davranışlarını yapılandırmaya yönelik seçenekler. İsteğe bağlı. |
| `onSuccess` | ()-> void | Modern okuyucu başarıyla başlatıldığında çağrılan bir kapanış. |
| `onFailure` | (_ hata: [Hata](#error)) -> void | Modern okuyucu yükleme başarısız olduğunda çağrılan bir kapanış. Bu kapanış hata kodunu [`Error`](#error) temsil eden bir nesne ve hatayla ilişkili hata iletisini döndürür. Daha fazla bilgi için bkz. [hata kodları](#error-codes). |

## <a name="types"></a>Türler

### <a name="content"></a>İçerik

Tam ekran okuyucu 'da gösterilecek içeriği içerir.

```swift
struct Content: Encodable {
    var title: String
    var chunks: [Chunk]
}
```

#### <a name="supported-mime-types"></a>Desteklenen MIME türleri

| MIME Türü | Açıklama |
| --------- | ----------- |
| metin/düz | Düz metin. |
| Application/MathML + XML | Matematik biçimlendirme dili (MathML). [Daha fazla bilgi edinin](https://developer.mozilla.org/en-US/docs/Web/MathML).

### <a name="options"></a>Seçenekler

Modern okuyucunun belirli davranışlarını yapılandıran özellikler içerir.

```swift
struct Options {
    var uiLang: String?
    var timeout: TimeInterval?
}
```

### <a name="error"></a>Hata

Hata hakkındaki bilgileri içerir.

```swift
struct Error {
    var code: String
    var message: String
}
```

#### <a name="error-codes"></a>Hata kodları

| Kod | Açıklama |
| ---- | ----------- |
| BadArgument | Sağlanan bağımsız değişken geçersiz, Ayrıntılar `message` için bkz. |
| zaman aşımı | Tam ekran okuyucusu belirtilen zaman aşımı süresi içinde yüklenemedi. |
| Tokenaşımına uğradı | Sağlanan belirtecin geçerliliği zaman aşımına uğradı. |
| Sürecek | Çağrı hızı sınırı aşıldı. |
| InternalError | Modern okuyucu görünüm denetleyicisi içinde bir iç hata oluştu. Ayrıntılar `message` için bkz.|

## <a name="os-version-support"></a>İşletim sistemi sürümü desteği

Modern okuyucu iOS SDK 'Sı, iPad ve iPhone üzerinde iOS 9,0 veya üzeri için desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'Da modern okuyucu IOS SDK 'sını](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS) keşfet
* [Hızlı Başlangıç: Tam ekran okuyucuyu (Swift) başlatan bir iOS uygulaması oluşturma](./ios-quickstart.md)