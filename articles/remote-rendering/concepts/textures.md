---
title: Doku
description: Doku kaynağı iş akışı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 09fa22d33377dfcbafd84f0caeb5f33a575b1bce
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681668"
---
# <a name="textures"></a>Doku

Dokular değişmez paylaşılan [bir kaynaktır.](../concepts/lifetime.md) Dokular [blob depolama](../how-tos/conversion/blob-storage.md) yüklenebilir ve doğrudan modellere uygulanabilir, Öğretici gösterildiği [gibi: Çevre ve malzeme değiştirme](../tutorials/unity/changing-environment-and-materials.md). En yaygın olsa da, dokular dönüştürülmüş bir [modelin](../how-tos/conversion/model-conversion.md)bir parçası olacak , onlar [malzemeleri](materials.md)ile başvurulan nerede .

## <a name="texture-types"></a>Doku türleri

Farklı doku türlerinin farklı kullanım örnekleri vardır:

* **2D Dokular** ağırlıklı olarak [malzemelerde](materials.md)kullanılır.
* **Küpler** [gökyüzü](../overview/features/sky.md)için kullanılabilir.

## <a name="supported-texture-formats"></a>Desteklenen doku biçimleri

ARR'a verilen tüm dokular [DDS formatında](https://en.wikipedia.org/wiki/DirectDraw_Surface)olmalıdır. Tercihen mipmaps ve doku sıkıştırma ile. Dönüştürme işlemini otomatikleştirmek istiyorsanız [TexConv komut satırı aracına](../resources/tools/tex-conv.md) bakın.

## <a name="loading-textures"></a>Dokuları yükleme

Bir doku yüklerken, beklenen türünü belirtmeniz gerekir. Tür uyuşmazlıkları varsa, doku yükü başarısız olur.
Aynı URI ile bir doku yüklemek paylaşılan bir [kaynak](../concepts/lifetime.md)olduğu gibi, aynı doku nesnesi döndürür.

Yükleme modellerine benzer şekilde, kaynak blob depolamasında bir doku kıymetini ele almanın iki çeşidi vardır:

* Doku varlığı, SAS URI tarafından ele alınabilir. İlgili yükleme `LoadTextureFromSASAsync` işlevi parametre `LoadTextureFromSASParams`ile. [Yerleşik dokuları](../overview/features/sky.md#built-in-environment-maps)yüklerken de bu varyantı kullanın.
* Doku blob depolama doğrudan blob depolama parametreleri tarafından ele alınabilir, durumda [blob depolama hesaba bağlı .](../how-tos/create-an-account.md#link-storage-accounts) Bu durumda ilgili yükleme `LoadTextureAsync` işlevi `LoadTextureParams`parametre ile.

Aşağıdaki örnek kod, bir dokunun SAS URI (veya yerleşik doku) üzerinden nasıl yüklendiğini gösterir - yalnızca yükleme işlevinin/parametresinin diğer servis talebi için farklılık gösterdiğini unutmayın:

``` cs
LoadTextureAsync _textureLoad = null;
void LoadMyTexture(AzureSession session, string textureUri)
{
    _textureLoad = session.Actions.LoadTextureAsync(new LoadTextureParams(textureUri, TextureType.Texture2D));
    _textureLoad.Completed +=
        (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
            _textureLoad = null;
        };
}
```

Dokunun ne için kullanılması gerektiğine bağlı olarak, doku türü ve içeriği için kısıtlamalar olabilir. Örneğin, bir [PBR malzemenin](../overview/features/pbr-materials.md) pürüzlülük haritası gri tonlama olmalıdır.

> [!CAUTION]
> ARR'daki tüm *Async* işlevleri eşsenkronize işlem nesnelerini döndürer. İşlem tamamlanana kadar bu nesnelere bir başvuru depolamanız gerekir. Aksi takdirde C# çöp toplayıcı işlemi erken silebilir ve asla bitiremez. Yukarıdaki örnek kodda üye değişken '_textureLoad' *tamamlanan* olay gelene kadar bir referans tutmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)
* [Gök -yüzü](../overview/features/sky.md)
