---
title: SQL temsilci yönetici izinlerini kullanarak Azure AD Connect'i yükleme | Microsoft Dokümanlar
description: Bu konu, Yalnızca SQL dbo izinleri olan bir hesabı kullanarak yüklemeye olanak tanıyan Azure AD Connect güncelleştirmesini açıklar.
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6269d00c9a6a8f827a4e31044d9d20efb0f8471b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60243533"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme
En son Azure AD Connect yapısından önce, SQL gerektiren yapılandırmaları dağıtırken yönetim delegasyonu desteklenmedi.  Azure AD Connect'i yüklemek isteyen kullanıcıların SQL sunucusunda sunucu yöneticisi (SA) izinlerine sahip olması gerekir.

Azure AD Connect'in en son sürümüyle, veritabanının sağlanması artık SQL yöneticisi tarafından bant dışında gerçekleştirilebilir ve daha sonra veritabanı sahibi haklarıyla Azure AD Connect yöneticisi tarafından yüklenebilir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu özelliği kullanmak için, birkaç hareketli parça olduğunu ve her birinin kuruluşunuzda farklı bir yönetici içerebileceğini fark etmeniz gerekir.  Aşağıdaki tablo, azure AD Connect'i bu özellik ile dağıtmadaki bireysel rolleri ve ilgili görevlerini özetley.

|Rol|Açıklama|
|-----|-----|
|Etki alanı veya Orman AD yöneticisi|Eşitleme hizmetini çalıştırmak için Azure AD Connect tarafından kullanılan etki alanı düzeyinde hizmet hesabı oluşturur.  Hizmet hesapları hakkında daha fazla bilgi [için, Bkz. Hesaplar ve izinler.](reference-connect-accounts-permissions.md)
|SQL yöneticisi|ADSync veritabanını oluşturur ve Azure AD Connect yöneticisine ve etki alanı/orman yöneticisi tarafından oluşturulan hizmet hesabına giriş + dbo erişimi sağlar.|
Azure AD Connect yöneticisi|Azure AD Connect'i yükler ve özel yükleme sırasında servis hesabını belirtir.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>SQL temsilci izinlerini kullanarak Azure AD Connect'i yükleme adımları
Veritabanını bant dışında sağlamak ve Azure AD Connect'i veritabanı sahibi izinleriyle yüklemek için aşağıdaki adımları kullanın.

>[!NOTE]
>Gerekmese de, veritabanı oluşturulurken Latin1_General_CI_AS harmanlamanın seçilmesi **önerilir.**


1. SQL Administrator'un ADSync veritabanını duyarsız harmanlama sırası **(Latin1_General_CI_AS)** ile oluşturmasını sorun.  Veritabanı **ADSync**olarak adlandırılmalıdır.  Kurtarma modeli, uyumluluk düzeyi ve kapsama türü, Azure AD Connect yüklendiğinde doğru değerlere güncelleştirilir.  Ancak harmanlama sırasının SQL yöneticisi tarafından doğru şekilde ayarlanması gerekir, aksi takdirde Azure AD Connect yüklemeyi engeller.  SA kurtarmak için silmek ve veritabanını yeniden gerekir.
 
   ![Harmanlama](./media/how-to-connect-install-sql-delegation/sql4.png)
2. Azure AD Connect yöneticisine ve etki alanı hizmeti hesabına aşağıdaki izinleri ver:
   - SQL Oturum Açma 
   - **veritabanı sahibi (dbo)** hakları.
 
   ![İzinler](./media/how-to-connect-install-sql-delegation/sql3a.png)

   >[!NOTE]
   >Azure AD Connect iç içe üyelikle girişleri desteklemez.  Bu, Azure AD Connect yönetici hesabınızın ve etki alanı hizmet hesabınızın dbo haklarına sahip bir girişe bağlanması gerektiği anlamına gelir.  Sadece dbo hakları ile bir giriş atanmış bir grubun üyesi olamaz.

3. Azure AD Connect'i yüklerken kullanılması gereken SQL sunucusu nu ve örnek adını belirten Bir e-postayı Azure AD Connect yöneticisine gönderin.

## <a name="additional-information"></a>Ek bilgiler
Veritabanı sağlandıktan sonra, Azure AD Connect yöneticisi şirket içi eşitlemeişlemlerini kendi kolaylıklarına göre yükleyebilir ve yapılandırabilir.

SQL Administrator'un önceki bir Azure AD Connect yedeklemesinden ADSync veritabanını geri yüklemesi durumunda, varolan bir veritabanını kullanarak yeni Azure AD Connect sunucusunu yüklemeniz gerekir. Varolan bir veritabanıyla Azure AD Connect'i yükleme hakkında daha fazla bilgi için, [varolan bir ADSync veritabanını kullanarak Azure AD Connect'i yükle'ye](how-to-connect-install-existing-database.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](how-to-connect-install-express.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
- [Azure AD Connect'i mevcut bir AD Eşitleme veritabanını kullanarak yükleme](how-to-connect-install-existing-database.md)  
