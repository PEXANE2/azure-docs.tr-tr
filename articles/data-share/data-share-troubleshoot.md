---
title: Azure veri paylaşımının önizlemesine sorun giderme
description: Azure veri paylaşımının Önizlemeiyle ilgili sorunları nasıl giderebileceğinizi öğrenin
services: data-share
author: joannapea
ms.service: data-share
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 386a5e34dccafa61859cd13c3e0ad88cd3a7ffac
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68421454"
---
# <a name="troubleshoot-common-issues-in-azure-data-share-preview"></a>Azure veri paylaşımında genel sorunları giderme Önizleme

Bu makalede, Azure veri paylaşımının önizlemesi için sık karşılaşılan sorunların nasıl giderileceği gösterilmektedir. 

## <a name="azure-data-share-invitations"></a>Azure Data Share davetleri 

Bazı durumlarda, yeni bir kullanıcı gönderilen e-posta **davetinde daveti kabul et** ' i tıklattığında, bu kişiler boş bir davetiye listesi ile sunulabilir. 

![Davetiye yok](media/no-invites.png)

Yukarıdaki hata, hizmette bilinen bir sorundur ve şu anda giderilmekte. Geçici bir çözüm olarak, aşağıdaki adımları izleyin. 

1. Azure portal **abonelikler** ' e gidin
1. Azure veri paylaşımında kullandığınız aboneliği seçin
1. **Kaynak sağlayıcıları** ' na tıklayın
1. Microsoft. DataShare araması yapın
1. **Kaydet** 'e tıklayın

Bu adımları tamamlayabilmeniz için [Azure KATıLıMCıSı RBAC rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) sahip olmanız gerekir. 

Hala bir veri paylaşma daveti göremiyorsanız, veri sağlayıcınızla iletişim kurun ve daveti e-posta diğer adınızla *değil* Azure oturum açma e-posta adresinize gönderdiklerinden emin olun. 

> [!IMPORTANT]
> Azure Data Share davetini zaten kabul etmiş ve depolamayı yapılandırmadan önce hizmetten çıkdıysanız, alınan veri paylaşımınızı yapılandırmayı nasıl tamamlayacağınızı öğrenmek için [bir veri kümesi eşleme](how-to-configure-mapping.md) ile nasıl yapılır Kılavuzu ' nda açıklanan yönergeleri izleyin ve veri almaya başlayın. 

## <a name="error-when-creating-or-receiving-a-new-data-share"></a>Yeni veri paylaşımının oluşturulması veya alınması sırasında hata oluştu

Hatayla İşlem geçersiz bir ' BadRequest ' durum kodu döndürdü

Hatayla AuthorizationFailed "

"Hata: depolama hesabına rol ataması"

![Ayrıcalık hatası](media/error-write-privilege.png)

Yeni veri paylaşımı oluştururken veya alırken yukarıdaki hatalardan birini alırsanız, bunun nedeni depolama hesabı üzerinde yeterli izinlerin olmamasıdır. Gerekli izinler, depolama sahibi rolünde bulunan veya özel bir role atanabilecek *Microsoft. Authorization/role atamaları/yazma*' dır. Depolama hesabını siz oluşturmuş olsanız bile, bu sizi otomatik olarak depolama hesabının sahibi yapmaz. Kendinize depolama hesabı sahipliği vermek için şu adımları izleyin. Alternatif olarak, ' ye ' ye ekleyebileceğiniz Bu izinle özel bir rol oluşturulabilir.  

1. Azure portalında Depolama hesabına gidin
1. **Erişim denetimi (IAM)** seçeneğini belirleyin
1. **Ekle** 'ye tıklayın
1. Sahibini sahip olarak içine ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.

