---
title: .NET ile ContentKeys oluşturma
description: Bu makalede, varlıklara güvenli erişim sağlayan içerik anahtarlarının nasıl oluşturulacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: aebd6dee9314d6e5641988767c024790b6b721f4
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74887706"
---
# <a name="create-contentkeys-with-net"></a>.NET ile ContentKeys oluşturma 
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services şifrelenmiş varlıklar oluşturmanıza ve sunmanıza olanak sağlar. Bir **Contentkey** , **varlığınıza güvenli**erişim sağlar. 

Yeni bir varlık oluşturduğunuzda (örneğin, [dosyaları karşıya](media-services-dotnet-upload-files.md)yüklemeden önce) aşağıdaki şifreleme seçeneklerini belirtebilirsiniz: **storageencryptıon**, **CommonEncryptionProtected**veya **EnvelopeEncryptionProtected**. 

İstemcilerinize varlıklar sunışınızda, varlıkların aşağıdaki iki şifreleden biriyle [dinamik olarak şifrelenmesini](media-services-dotnet-configure-asset-delivery-policy.md) sağlayabilirsiniz: **DynamicEnvelopeEncryption** veya **dynamiccommonencryption**.

Şifrelenmiş varlıkların, **Contentkey**s ile ilişkilendirilmesi gerekir. Bu makalede bir içerik anahtarının nasıl oluşturulacağı açıklanır.

> [!NOTE]
> Media Services .NET SDK kullanarak yeni bir **Storageşifrelenen** varlık oluştururken, **contentkey** otomatik olarak oluşturulup varlıkla bağlanır.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
İçerik anahtarı oluştururken ayarlamanız gereken değerlerden biri içerik anahtar türüdür. Aşağıdaki değerlerden birini seçin. 

```csharp
    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }
```

## <a id="envelope_contentkey"></a>Zarf türü ContentKey oluştur
Aşağıdaki kod parçacığı, zarf şifreleme türünün bir içerik anahtarını oluşturur. Daha sonra anahtarı belirtilen varlıkla ilişkilendirir.

```csharp
    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

call

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);
```


## <a id="common_contentkey"></a>Ortak tür ContentKey oluştur
Aşağıdaki kod parçacığı, ortak şifreleme türünün bir içerik anahtarını oluşturur. Daha sonra anahtarı belirtilen varlıkla ilişkilendirir.

```csharp
    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
call

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirim sağlayın
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

