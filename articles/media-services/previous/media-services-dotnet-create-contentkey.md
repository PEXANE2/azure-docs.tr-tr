---
title: .NET ile İçerik Tuşları Oluşturma
description: Bu makalede, varlıklara güvenli erişim sağlayan içerik anahtarlarının nasıl oluşturulacak gösteriş yaptığı gösterilmiştir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251160"
---
# <a name="create-contentkeys-with-net"></a>.NET ile İçerik Tuşları Oluşturma 
> [!div class="op_single_selector"]
> * [Geri kalanı](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Medya Hizmetleri, şifreli varlıklar oluşturmanıza ve teslim etmenizi sağlar. **ContentKey,** Varlık'ınıza **Asset**güvenli erişim sağlar. 

Yeni bir varlık oluşturduğunuzda (örneğin, [dosya yüklemeden](media-services-dotnet-upload-files.md)önce), aşağıdaki şifreleme seçeneklerini belirtebilirsiniz: **StorageEncrypted**, **CommonEncryptionProtected**veya **EnvelopeEncryptionProtected**. 

Müşterilerinize varlık teslim ettiğinizde, varlıkların aşağıdaki iki şifrelemeden biriyle dinamik olarak şifrelenecek şekilde [yapılandırabilirsiniz:](media-services-dotnet-configure-asset-delivery-policy.md) **DynamicEnvelopeEncryption** veya **DynamicCommonEncryption**.

Şifrelenmiş varlıkların **ContentKey**s ile ilişkilendirilmesi gerekir. Bu makalede, içerik anahtarının nasıl oluşturulacak olduğu açıklanmaktadır.

> [!NOTE]
> Media Services .NET SDK'yı kullanarak yeni bir **StorageEncrypted** varlık oluştururken **ContentKey** otomatik olarak oluşturulur ve varlıkla bağlantılıdır.
> 
> 

## <a name="contentkeytype"></a>İçerikKeyType
İçerik anahtarı oluştururken ayarlamanız gereken değerlerden biri içerik anahtarı türüdür. Aşağıdaki değerlerden birini seçin. 

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

## <a name="create-envelope-type-contentkey"></a><a id="envelope_contentkey"></a>Zarf türü oluşturma İçerikAnahtar
Aşağıdaki kod parçacığı zarf şifreleme türünün bir içerik anahtarı oluşturur. Daha sonra anahtarı belirtilen varlıkla ilişkilendirer.

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


## <a name="create-common-type-contentkey"></a><a id="common_contentkey"></a>Ortak tür oluşturma ContentKey
Aşağıdaki kod parçacığı, ortak şifreleme türünün bir içerik anahtarını oluşturur. Daha sonra anahtarı belirtilen varlıkla ilişkilendirer.

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

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

