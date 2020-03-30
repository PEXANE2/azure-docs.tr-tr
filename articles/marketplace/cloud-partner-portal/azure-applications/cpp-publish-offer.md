---
title: Azure uygulama teklifini yayımla | Azure Marketi
description: Azure Marketi'nde bir Azure uygulama teklifi yayımlama işlemini ve adımları açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: dsindona
ms.openlocfilehash: 9f89e31c2d17ef74971d2057ba58e9572e92184c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280108"
---
# <a name="publish-azure-application-offer"></a>Azure uygulama teklifini yayımlama

**Yeni Teklif** sayfasındaki bilgileri sağlayarak bir teklif oluşturduktan sonra teklifi yayımlayabilirsiniz. Yayımlama işlemini başlatmak için **Yayımla'yı** seçin.

Aşağıdaki diyagram, "canlı yayına geçmek" için yayımlama işlemindeki ana adımları gösterir.

![Yayımlama adımlarını sunun](./media/offer-publishing-steps.png)


## <a name="detailed-description-of-publishing-steps"></a>Yayımlama adımlarının ayrıntılı açıklaması

Aşağıdaki tablo listeler ve her yayımlama adımı açıklar ve her adımı tamamlamak için bir zaman tahmini sağlar.  "Gün" deki Times tahminleri, hafta sonlarını ve tatilleri hariç tutan iş günleri olarak tanımlanır.

|  **Yayımlama Adımı**           | **Zaman**    | **Açıklama**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Ön koşulları doğrula         | < 15 dk    | Teklif bilgileri ve teklif ayarları doğrulanır.                        |
| Etkilenen gelir ayarlarını doğrulama | < 15 dk  | Teklif için Azure kaynak kullanımı atıf denetlenir.             |
| Sertifika                  | < 1 gün     | Teklif, Azure Sertifika Ekibi tarafından analiz edilir. Teklifte virüs, kötü amaçlı yazılım, güvenlik uyumluluğu ve güvenlik sorunu taraması yapılır. Teklifin tüm uygunluk ölçütlerini karşılayıp karşılamadığı kontrol edilir. Daha fazla bilgi için [ön koşullara](./cpp-prerequisites.md)bakın. Bir sorun bulunursa geri bildirim sağlanır. |
| Test Sürüşü doğrulaması          | < 2 saat   | (İsteğe bağlı) Bir Test Sürüşü varsa, Microsoft bunun dağıtılabilir ve çoğaltılabileceğini doğrular.  |
| Ambalaj ve kurşun üretimi kaydı | < 1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenir ve müşteri adayı sistemleri yapılandırılır ve dağıtılır. |
|  Yayıncı oturum kapatma             |  El ile    | Teklif yayına girmeden önce son yayıncı incelemesi ve onayı. Teklif artık önizleme için kullanılabilir.  Teklifinizi, tüm gereksinimlerinizi karşıladığını doğrulamak için seçili aboneliklerde (teklif bilgileri adımlarında) dağıtabilirsiniz.  Teklifi doğruladıktan sonra, teklifinizin bir sonraki adıma geçebilmeleri için **Canlı Git'i** seçin. |
| Microsoft incelemesi                | 7 - 14 gün arası | Microsoft, Azure uygulamanızı tümsel olarak inceler ve sorunlar tespit edilirse size e-postalar gönderebilirsiniz.  Bu adımın uzunluğu, uygulamanın karmaşıklığına, ortaya çıkarılan sorunlara ve bunlara ne kadar hızlı yanıt verilebildiğinize bağlıdır.  |
| Canlı                           | < 1 gün | Teklif serbest bırakılır, belirtilen bölgelere çoğaltılır ve kamuya açık hale getirilir. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |

Bulut İş Ortağı Portalı'ndaki teklifiniz için **Durum** sekmesinde yayımlama işlemini izleyebilirsiniz.

![Azure uygulaması teklifi için durum sekmesi](./media/offer-status-tab.png)

Yayımlama işlemini tamamladıktan sonra teklifiniz [Microsoft Azure Marketi uygulama kategorisinde](https://azuremarketplace.microsoft.com/marketplace/apps/)listelenir.

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](../../cloud-solution-providers.md) bakın.

## <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

**Teklifinizin** yayımlama durumunu görüntülemenin yanı sıra Durum sekmesi, bir sorunla karşılaşılan tüm yayımlama adımlarından gelen hata iletilerini ve geri bildirimleri de görüntüler.  Sorun kritikse, yayımlama iptal edilir.  Daha sonra bildirilen sorunu düzeltmeniz ve teklifi yeniden yayınlamanız gerekir.  Microsoft **gözden geçirme** adımı teklifinizin ve ilişkili teknik varlıklarının (özellikle Azure Kaynak Yöneticisi şablonu) kapsamlı bir incelemesini temsil ettiği için, sorunlar genellikle çekme isteği (PR) bağlantıları olarak sunulur.  Bu PR'lerin nasıl görüntülenilip yanıtverileceklerine ilişkin bir açıklama, [bkz.](./cpp-handling-review-feedback.md)


## <a name="next-steps"></a>Sonraki adımlar

Yayımlama adımlarından birinde veya daha fazlasında hatalarla karşılaştıysanız, bunları düzeltmeniz ve teklifinizi yeniden yayımlamalısınız.  **Microsoft gözden geçirme** adımında kritik sorunlarla karşılaşılırsa, Microsoft inceleme ekibinin Azure DevOps deposuna erişerek gözden geçirme geri [bildirimini işlemeniz](./cpp-handling-review-feedback.md) gerekir.

Bir Azure uygulaması başarıyla yayımlandıktan sonra, değişen iş veya teknik gereksinimleri yansıtacak şekilde [varolan teklifi güncelleştirebilirsiniz.](./cpp-update-existing-offer.md) 
