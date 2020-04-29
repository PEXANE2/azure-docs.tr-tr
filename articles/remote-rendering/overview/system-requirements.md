---
title: Sistem gereksinimleri
description: Azure uzaktan Işleme için sistem gereksinimlerini listeler
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: article
ms.openlocfilehash: 8573a88d5371bbde07a541c789f52e6c44f1e279
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81411123"
---
# <a name="system-requirements"></a>Sistem gereksinimleri

> [!IMPORTANT]
> **Azure uzaktan işleme** Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu bölümde, *Azure uzaktan işleme* (ARR) ile çalışmak için en düşük sistem gereksinimleri listelenir.

## <a name="development-pc"></a>Geliştirme BILGISAYARı

* Windows 10 sürüm 1903 veya üzeri.
* Güncel grafik sürücüleri.
* İsteğe bağlı: uzaktan işlenmiş içeriğin yerel önizlemesini (örneğin Unity) kullanmak istiyorsanız, h265 Hardware video kod çözücüsü.

> [!IMPORTANT]
> Windows Update her zaman en son GPU sürücülerini teslim etmez, en son sürücüler için GPU üreticinizin Web sitesini kontrol edin:
>
> * [AMD Sürücüler](https://www.amd.com/en/support)
> * [Intel sürücüleri](https://www.intel.com/content/www/us/en/support/detect.html)
> * [NVıDıA sürücüleri](https://www.nvidia.com/Download/index.aspx)

Aşağıdaki tabloda, hangi GPU 'Ların h265 donanım video kodunu çözmesinin desteklediği listelenmiştir.

| GPU üreticisi | Desteklenen modeller |
|-----------|:-----------|
| NVIDIA | [Bu sayfanın en altındaki](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix) **NVDEC destek matrisini** kontrol edin. GPU 'larınızın **H. 265 4:2:0 8-bit** sütununda Yes olması gerekir. |
| AMD | En az AMD 'nin [Birleşik video kod çözücüsünün](https://en.wikipedia.org/wiki/Unified_Video_Decoder#UVD_6)6 sürümüne sahip GPU 'lar. |
| Intel | Ufuk Gölü ve daha yeni CPU 'Lar |

Doğru h265 codec bileşeni yüklenebilse de, codec dll 'Lerinde güvenlik özellikleri codec başlatma hatalarıyla karşılaşabilir. [Sorun giderme kılavuzu](../resources/troubleshoot.md#h265-codec-not-available) , bu sorunu çözme adımlarını açıklamaktadır. DLL sorunu yalnızca hizmet, Unity içindeki örneğin bir masaüstü uygulamasında kullanılırken olabilir.

## <a name="devices"></a>Cihazlar

Azure uzaktan Işleme Şu anda yalnızca bir hedef cihaz olarak **HoloLens 2** ve Windows Masaüstü 'nü desteklemektedir. [Platform sınırlamaları](../reference/limits.md#platform-limitations) bölümüne bakın.

Daha yeni sürümlerde gecikme süresi bakımından önemli iyileştirmeler olduğu için en son HEVC codec bileşeni kullanılması önemlidir. Cihazınızda hangi sürümün yüklü olduğunu denetlemek için:

1. **Microsoft Store**başlatın.
1. Sağ üst köşedeki **"..."** düğmesine tıklayın.
1. **İndirmeler ve Güncelleştirmeler '** i seçin.
1. **Cihaz üreticisinden HEVC video uzantıları**listesinde arama yapın.
1. Listelenen codec bileşeninin en az sürüm **1.0.21821.0**olduğundan emin olun.
1. **Güncelleştirmeleri al** düğmesine tıklayın ve yüklemesinin tamamlanmasını bekleyin.

## <a name="network"></a>Ağ

İyi bir kullanıcı deneyimi için kararlı, düşük gecikmeli bir ağ bağlantısı kritik öneme sahiptir.

Bkz. [ağ gereksinimleri](../reference/network-requirements.md)için adanmış bölüm.

Ağ sorunlarını gidermek için [sorun giderme kılavuzuna](../resources/troubleshoot.md#unstable-holograms)bakın.

## <a name="software"></a>Yazılım

Aşağıdaki yazılım yüklü olmalıdır:

* **Visual Studio 2019** ' un en son sürümü [(indirme)](https://visualstudio.microsoft.com/vs/older-downloads/)
* **Windows SDK 10.0.18362.0** [(İndir)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* **GIT** [(İndir)](https://git-scm.com/downloads)
* İsteğe bağlı: bir masaüstü bılgısayar üzerindeki sunucudan video akışını görüntülemek Için **HEVC video uzantılarına** [(Microsoft Store bağlantı)](https://www.microsoft.com/p/hevc-video-extensions/9nmzlz57r3t7)ihtiyacınız vardır.

## <a name="unity"></a>Unity

Unity ile geliştirme için, yüklemesi

* Unity 2019.3.1 [(indirme)](https://unity3d.com/get-unity/download)
* Bu modülleri Unity 'ye yükler:
  * **UWP** -Evrensel Windows platformu derleme desteği
  * **IL2CPP** -Windows derleme DESTEĞI (IL2CPP)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Unity ile model Işleme](../quickstarts/render-model.md)
