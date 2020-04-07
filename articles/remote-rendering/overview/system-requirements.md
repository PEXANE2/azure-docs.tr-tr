---
title: Sistem gereksinimleri
description: Azure Uzaktan İşleme için sistem gereksinimlerini listeler
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: article
ms.openlocfilehash: c239f7062dc39492a0cf63ac3aadbaf94acbf032
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680446"
---
# <a name="system-requirements"></a>Sistem gereksinimleri

> [!IMPORTANT]
> **Azure Uzaktan İşleme** şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bu bölümde, *Azure Uzaktan İşleme* (ARR) ile çalışmak için minimum sistem gereksinimleri listelenir.

## <a name="development-pc"></a>Geliştirme PC

* Windows 10 sürüm 1903 veya daha yüksek.
* Güncel grafik sürücüleri.
* İsteğe bağlı: Uzaktan işlenen içeriğin yerel önizlemesini kullanmak istiyorsanız H265 donanım video kod çözücüsü (örneğin Unity'de).

> [!IMPORTANT]
> Windows güncellemesi her zaman en son GPU sürücülerini sağlamaz, En son sürücüler için GPU üreticinizin web sitesini kontrol edin:
>
> * [AMD sürücüleri](https://www.amd.com/en/support)
> * [Intel sürücüleri](https://www.intel.com/content/www/us/en/support/detect.html)
> * [NVIDIA sürücüleri](https://www.nvidia.com/Download/index.aspx)

Aşağıdaki tabloda GPU'ların H265 donanım video çözmeyi desteklediği listeleilmektedir.

| GPU üreticisi | Desteklenen modeller |
|-----------|:-----------|
| Nvidia | Bu sayfanın altındaki **NVDEC Destek** Matrisi'ni kontrol [edin.](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix) GPU'nuzun **H.265 4:2:0 8-bit** sütununda EVET'e ihtiyacı vardır. |
| Amd | AMD'nin [Birleşik Video Kod Çözücü'nün](https://en.wikipedia.org/wiki/Unified_Video_Decoder#UVD_6)en az 6 sürümüne sahip GPU'lar. |
| ıntel | Skylake ve yeni CPU'lar |

Doğru H265 codec yüklü olsa da, codec DLs güvenlik özellikleri codec başlatma hatalarıneden olabilir. [Sorun giderme kılavuzu,](../resources/troubleshoot.md#h265-codec-not-available) bu sorunun nasıl çözüleceğiniz adımlarını açıklar. DLL sorunu yalnızca bir masaüstü uygulamasında, örneğin Unity'de hizmeti kullanırken oluşabilir.

## <a name="devices"></a>Cihazlar

Azure Uzaktan İşleme şu anda yalnızca **HoloLens 2** ve Windows masaüstü UWP'yi hedef aygıt olarak destekler.

Yeni sürümlerde gecikme gecikmesinde önemli iyileştirmeler olduğundan, en son HEVC codec'ini kullanmak önemlidir. Cihazınızda hangi sürümün yüklü olduğunu kontrol etmek için:

1. Microsoft **Mağazası'nı**başlatın.
1. Sağ üstteki **"..."** düğmesine tıklayın.
1. **İndirme ler ve Güncelleştirmeler'i**seçin.
1. **Aygıt Üreticisinden HEVC Video Uzantıları**listesini arayın.
1. Listelenen codec en az sürüm **1.0.21821.0**olduğundan emin olun.
1. Güncellemeleri **Al** düğmesini tıklatın ve yüklenmesini bekleyin.

## <a name="network"></a>Ağ

İyi bir kullanıcı deneyimi için kararlı, düşük gecikmeli ağ bağlantısı çok önemlidir.

[Ağ gereksinimleri](../reference/network-requirements.md)için özel bölüme bakın.

Sorun giderme ağı sorunları için [Sorun Giderme Kılavuzu'na](../resources/troubleshoot.md#unstable-holograms)bakın.

## <a name="software"></a>Yazılım

Aşağıdaki yazılım yüklenmelidir:

* Visual Studio **2019'un** en son sürümü [(indir)](https://visualstudio.microsoft.com/vs/older-downloads/)
* **Windows SDK 10.0.18362.0** [(indir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* **GIT** [(indir)](https://git-scm.com/downloads)
* İsteğe bağlı: Bir masaüstü bilgisayarda sunucudan video akışını görüntülemek için **HEVC Video Uzantıları** [(Microsoft Store bağlantısı)](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7)gerekir.

## <a name="unity"></a>Unity

Unity ile geliştirme için,

* Birlik 2019.3.1 [(indir)](https://unity3d.com/get-unity/download)
* Bu modülleri Birlik olarak yükleyin:
  * **UWP** - Universal Windows Platform Build Desteği
  * **IL2CPP** - Windows Yapı Desteği (IL2CPP)

## <a name="next-steps"></a>Sonraki adımlar

* [Quickstart: Unity ile bir model oluşturma](../quickstarts/render-model.md)
