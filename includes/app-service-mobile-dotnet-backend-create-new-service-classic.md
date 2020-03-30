---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: e087a1db008422aeec8fd4e073a7476eebe4d54b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67189053"
---
1. [Azure portalda] oturum açın.
2. **+Yenİ** > **Web + Mobil** > **Mobil Uygulama'yı**seçin ve ardından Mobil Uygulamalarınız için bir ad sağlayın.
3. **Kaynak Grubu**için varolan bir kaynak grubu seçin veya yeni bir kaynak oluşturun (uygulamanızla aynı adı kullanarak). 
4. **Uygulama Hizmeti planı**için varsayılan plan [(Standart katmanda)](https://azure.microsoft.com/pricing/details/app-service/)seçilir. Aynı zamanda başka bir plan da seçebilir veya [yeni bir tane oluşturabilirsiniz](../articles/app-service/app-service-plan-manage.md#create-an-app-service-plan). 

   Uygulama Hizmeti planının ayarları, uygulamanızla ilişkili [konumu, özellikleri, maliyeti ve bilgi işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) belirler. Uygulama Hizmeti planları ve farklı bir fiyatlandırma katmanında ve istediğiniz konumda yeni bir planın nasıl oluşturulacağına ilişkin daha fazla bilgi için Azure [Uygulama Hizmeti planlarına ayrıntılı bir bakış](../articles/app-service/overview-hosting-plans.md)bakın.
   
5. **Oluştur'u**seçin. Bu adım, Mobil Uygulamalar arka uç oluşturur. 
6. Yeni Mobil Uygulamalar arka uç için **Ayarlar** bölmesinde, **bir veritabanına bağlanmak**> istemci uygulama platformunuzu > hızlı **başlat'ı** seçin. 
   
   ![Veritabanıbağlama için seçimler](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
7. Veri **bağlantısı ekle** bölmesinde, **SQL Veritabanı'nı** > seçin**yeni bir veritabanı oluşturun.** Veritabanı adını girin, bir fiyatlandırma katmanı seçin ve ardından **Sunucu'yu**seçin. Bu yeni veritabanını yeniden kullanabilirsiniz. Aynı konumda zaten bir veritabanınız varsa, bunun yerine **Mevcut veritabanlarından birini kullanın**’ı seçebilirsiniz. Bant genişliği maliyetleri ve daha yüksek gecikme gecikmesi nedeniyle veritabanının farklı bir konumda kullanılmasını önermiyoruz.
   
   ![Veritabanı seçme](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
8. Yeni **sunucu** bölmesine, **Sunucu adı** kutusuna benzersiz bir sunucu adı girin, oturum açma ve parola sağlayın, **sunucuya erişmek için Azure hizmetlerine İzin Ver'i**seçin ve **Tamam'ı**seçin. Bu adım yeni veritabanını oluşturur.
9. **Veri bağlantısı ekle** bölmesinde Bağlantı **dizesini**seçin, veritabanınızın giriş ve parola değerlerini girin ve **Tamam'ı**seçin. 

   Devam etmeden önce veritabanının başarıyla dağıtılması için birkaç dakika bekleyin.

<!-- URLs. -->
[Azure portalında]: https://portal.azure.com/
