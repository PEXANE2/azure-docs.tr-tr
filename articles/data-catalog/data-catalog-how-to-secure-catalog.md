---
title: Azure Veri Kataloğu'na erişim nasıl güvenli hale ilir?
description: Bu makalede, Azure Veri Kataloğu'nda bir veri kataloğunun ve veri varlıklarının nasıl güvenli hale verilebildiğini açıklanmaktadır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: b0972be2b8a6e05d3d90cde7354b4890ea95cbe4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68976757"
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Veri kataloğuna ve veri varlıklarına erişim nasıl güvenli hale ilir?

> [!IMPORTANT]
> Bu özellik yalnızca Azure Veri Kataloğu'nun standart sürümünde kullanılabilir.

Azure Veri Kataloğu, veri kataloğuna kimlerin erişebileceğini ve katalogdaki meta verilerüzerinde hangi işlemleri gerçekleştirebileceklerini (kayıt, ek açıklama, sahiplik alma) belirtmenize olanak tanır. 

## <a name="catalog-users-and-permissions"></a>Katalog kullanıcıları ve izinleri

Bir kullanıcıya veya gruba veri kataloğuna erişim sağlamak ve izinleri ayarlamak için:

1. Veri [kataloğunuzun ana sayfasında](https://www.azuredatacatalog.com)araç çubuğundaki **Ayarlar'ı** tıklatın.

   ![Azure Veri Kataloğu ana sayfa ayarları düğmesi](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)

2. Ayarlar sayfasında **Katalog Kullanıcıları** bölümünü genişletin.

   ![Azure Veri Kataloğu kullanıcıları Ekle düğmesi](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)

3. **Ekle**’ye tıklayın.

4. Katalogla ilişkili Azure Etkin Dizini'ne (AAD) tam nitelikli **kullanıcı adını** veya **güvenlik grubunun** adını girin. Birden fazla kullanıcı veya grup ekliyorsanız, ayırıcı olarak virgül (',') kullanın.

   ![Azure Veri Kataloğu kullanıcıları - kullanıcılar veya gruplar](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)

5. Metin kutusundan **ENTER** veya **TAB** tuşuna basın. 

6. Tüm izinlerin **(Açıklama,** **Kaydol**ve **Sahipliği Al)** varsayılan olarak bu kullanıcılara veya gruplara atandığını doğrulayın. Diğer bir zamanda, kullanıcı veya grup [veri varlıklarını kaydedebilir,]( data-catalog-how-to-register.md) [veri varlıklarına açıklama ekleyebilir]( data-catalog-how-to-annotate.md)ve [veri varlıklarının sahipliğini alabilir.]( data-catalog-how-to-manage.md) 

   ![Azure Veri Kataloğu kullanıcıları - varsayılan izinler](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)

7. Bir kullanıcıya veya gruba yalnızca katalogiçin okuma erişimi vermek için, söz kullanıcı veya grup için **açıklama ek açıklama** seçeneğini temizleyin. Bunu yaptığınızda, kullanıcı veya grup katalogdaki veri varlıklarına açıklama ekleyemez, ancak bunları görüntüleyebilir. 

8. Bir kullanıcının veya grubun veri varlıklarını kaydetmesini engellemek için, söz kullanıcı veya grup için **kayıt** seçeneğini temizleyin.

9. Bir kullanıcının bir veri varlığının sahipliğini almasını engellemek için, söz kullanıcı veya grup için **sahiplik seçeneğini** temizleyin. 

10. Katalog kullanıcılarından bir kullanıcı/grubu silmek için listenin altındaki kullanıcı/grup için **x'i** tıklatın. 

   ![Azure Veri Kataloğu kataloğu kullanıcıları - kullanıcı X simgesini silme](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

   > [!IMPORTANT]
   > Kullanıcıları doğrudan eklemek ve izinler atamak yerine kullanıcıları kataloglamak için güvenlik grupları eklemenizi öneririz. Ardından, rolleri ve kataloğuna gerekli erişimleriyle eşleşen güvenlik gruplarına kullanıcıları ekleyin.

## <a name="special-considerations"></a>Özel hususlar

- Güvenlik gruplarına atanan izinler ek maddedir. Diyelim ki, bir kullanıcı iki gruptadır. Bir grubun açıklama izni vardır ve diğer grubun açıklama izni yoktur. Ardından, kullanıcının açıklama izni vardır. 
- Kullanıcıya açıkça atanan izinler, kullanıcının ait olduğu gruplara atanan izinleri geçersiz kılar. Önceki örnekte, örneğin, kullanıcıyı açıkça katalog kullanıcılarına eklediniz ve açıklama lı izinler atamadınız. Kullanıcı açıklama izni olan bir grubun üyesi olsa bile, kullanıcı veri varlıklarına açıklama ekleyemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md)
