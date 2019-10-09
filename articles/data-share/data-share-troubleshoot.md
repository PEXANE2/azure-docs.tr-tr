---
title: Azure veri paylaşımının önizlemesine sorun giderme
description: Azure veri paylaşımı önizlemesi ile veri paylaşımları oluştururken veya alırken davetiye ve hatalarla ilgili sorunları nasıl giderebileceğinizi öğrenin.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.openlocfilehash: 592a2d464aed8c39dfd11734beccbd0399d75fd9
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169220"
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

"Hata: Işlem geçersiz bir durum kodu döndürdü: ' BadRequest '"

"Hata: AuthorizationFailed"

"Hata: depolama hesabına rol ataması"

![Ayrıcalık hatası](media/error-write-privilege.png)

Yeni bir veri paylaşımının oluşturulması veya yeni bir veri paylaşımının alınması sırasında yukarıdaki hatalardan birini alırsanız, bu, depolama hesabı için yeterli izin olmadığından oluşur. Gerekli izinler, depolama sahibi rolünde bulunan veya özel bir role atanabilecek *Microsoft. Authorization/role atamaları/yazma*' dır. Depolama hesabı oluşturmuş olsanız bile, depolama hesabının sahibini otomatik olarak yapmaz. Depolama hesabının sahibini sağlamak için aşağıdaki adımları izleyin. Alternatif olarak, ' ye ' ye ekleyebileceğiniz Bu izinle özel bir rol oluşturulabilir.  

1. Azure portal depolama hesabına gidin
1. **Erişim denetimi (IAM)** seçeneğini belirleyin
1. **Ekle** 'ye tıklayın
1. Sahibini sahip olarak içine ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.

