---
title: SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect 'yi | Microsoft Docs
description: Bu konu, yalnızca SQL DBO iznine sahip olan bir hesap kullanılarak yüklemeye izin veren Azure AD Connect güncelleştirmesini açıklamaktadır.
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
ms.topic: how-to
ms.date: 02/26/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f082ec896bf0542b63c8c1d0257679681334050
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85358676"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme
En son Azure AD Connect derlemeden önce, SQL gerektiren yapılandırmaların dağıtımı sırasında yönetici temsilcileri desteklenmez.  SQL Server üzerinde Sunucu Yöneticisi (SA) izinlerinin yüklenmesi Azure AD Connect yüklemek isteyen kullanıcılar.

Azure AD Connect en son sürümü sayesinde, veritabanının sağlanması artık SQL Yöneticisi tarafından bant dışında gerçekleştirilebilir ve ardından veritabanı sahibi haklarıyla Azure AD Connect Yöneticisi tarafından yüklenebilir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu özelliği kullanmak için, birkaç hareketli parça olduğunu ve her birinin kuruluşunuzda farklı bir yöneticiyi içerebileceği hakkında fark etmeniz gerekir.  Aşağıdaki tabloda, bu özellik ile Azure AD Connect dağıtımı sırasında bireysel roller ve ilgili görevler özetlenmektedir.

|Rol|Description|
|-----|-----|
|Etki alanı veya orman AD Yöneticisi|Eşitleme hizmetini çalıştırmak için Azure AD Connect tarafından kullanılan etki alanı düzeyi hizmet hesabını oluşturur.  Hizmet hesapları hakkında daha fazla bilgi için bkz. [hesaplar ve izinler](reference-connect-accounts-permissions.md).
|SQL Yöneticisi|ADSync veritabanını oluşturur ve Azure AD Connect yöneticisine ve etki alanı/orman Yöneticisi tarafından oluşturulan hizmet hesabına oturum açma + dbo erişimi verir.|
Azure AD Connect Yöneticisi|, Azure AD Connect yüklenir ve özel yükleme sırasında hizmet hesabını belirtir.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>Azure AD Connect SQL temsilcisi izinleri kullanarak yükleme adımları
Veritabanını bant dışı sağlamak ve veritabanı sahibi izinleriyle Azure AD Connect yüklemek için aşağıdaki adımları kullanın.

>[!NOTE]
>Gerekli olmasa da, veritabanı oluşturulurken Latin1_General_CI_AS harmanlama **highly recommended** seçilmelidir.


1. SQL Yöneticisi 'nin, büyük/küçük harfe duyarsız harmanlama sırası **(Latin1_General_CI_AS)** Ile ADSync veritabanını oluşturmasını sağlayabilirsiniz.  Veritabanının **ADSync**olarak adlandırılması gerekir.  Kurtarma modeli, uyumluluk düzeyi ve içerme türü Azure AD Connect yüklenirken doğru değerlere güncelleştirilir.  Ancak harmanlama sırasının SQL Yöneticisi tarafından doğru şekilde ayarlanması gerekir, aksi takdirde Azure AD Connect yüklemeyi engeller.  Bu SA 'yı kurtarmak için, veritabanını silip yeniden oluşturmanız gerekir.
 
   ![Harmanlama](./media/how-to-connect-install-sql-delegation/sql4.png)
2. Azure AD Connect yöneticisine ve etki alanı hizmeti hesabına aşağıdaki izinleri verin:
   - SQL Oturum Açma 
   - **veritabanı sahibi (dbo)** hakları.
 
   ![İzinler](./media/how-to-connect-install-sql-delegation/sql3a.png)

   >[!NOTE]
   >Azure AD Connect, iç içe üyelik ile oturum açmaları desteklemez.  Bu, Azure AD Connect Yönetici hesabınızın ve etki alanı hizmet hesabının dbo hakları verilen bir oturum ile bağlantılı olması gerektiği anlamına gelir.  Yalnızca dbo haklarıyla bir oturum açmaya atanan bir grubun üyesi olamaz.

3. Azure AD Connect yüklenirken kullanılması gereken SQL Server ve örnek adını belirten Azure AD Connect yöneticisine bir e-posta gönderin.

## <a name="additional-information"></a>Ek bilgiler
Veritabanı sağlandıktan sonra Azure AD Connect Yöneticisi, şirket içi eşitlemeyi kolay bir şekilde yükleyebilir ve yapılandırabilir.

SQL Yöneticisi, ADSync veritabanını önceki bir Azure AD Connect yedeğinden geri yüklerse, mevcut bir veritabanını kullanarak yeni Azure AD Connect sunucusunu yüklemeniz gerekir. Mevcut bir veritabanıyla Azure AD Connect yükleme hakkında daha fazla bilgi için, bkz. [ınstall Azure AD Connect in an bir ADSync Database](how-to-connect-install-existing-database.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](how-to-connect-install-express.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
- [Azure AD Connect'i mevcut bir AD Eşitleme veritabanını kullanarak yükleme](how-to-connect-install-existing-database.md)  
