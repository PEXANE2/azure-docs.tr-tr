---
title: Azure Analiz Hizmetlerini Yönetme | Microsoft Dokümanlar
description: Bu makalede, bir Azure Çözümleme Hizmetleri sunucusunun yönetim ve yönetim görevlerini yönetmek için kullanılan araçlar açıklanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 28d7b2955c84833841760e441cd2919181e22bc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572804"
---
# <a name="manage-analysis-services"></a>Analysis Services’ı yönetme
Azure'da bir Çözümleme Hizmetleri sunucusu oluşturduktan sonra, hemen veya bir süre sonra gerçekleştirmeniz gereken bazı yönetim ve yönetim görevleri olabilir. Örneğin, verileri yenilemek için işleme çalıştırın, sunucunuzdaki modellere kimlerin erişebileceğini kontrol edin veya sunucunuzun durumunu izleyin. Bazı yönetim görevleri yalnızca Azure portalında, diğerleri SQL Server Management Studio 'da (SSMS) ve bazı görevler de yapılabilir.

## <a name="azure-portal"></a>Azure portalında
[Azure portalı,](https://portal.azure.com/) sunucular oluşturup silebileceğiniz, sunucu kaynaklarını izleyebileceğiniz, boyutunu değiştirebileceğiniz ve sunucularınıza kimlere erişimi olan kişileryönetebileceğiniz yerdir.  Bazı sorunlar yaşıyorsanız, bir destek isteği de gönderebilirsiniz.

![Azure'da sunucu adını alma](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Azure'da sunucunuza bağlanmak, tıpkı kendi kuruluşunuzdaki bir sunucu örneğine bağlanmak gibidir. SSMS'den, işlem verileri veya işleme komut dosyası oluşturma, rolleri yönetme ve PowerShell gibi aynı görevlerin çoğunu gerçekleştirebilirsiniz.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>SSMS’yi indirme ve yükleme
Azure Analiz Hizmetleri sunucunuza bağlanırken en son özellikleri ve en sorunsuz deneyimi elde etmek için SSMS'in en son sürümünü kullandığınızdan emin olun. 

[SQL Server Management Studio'u indirin.](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)


### <a name="to-connect-with-ssms"></a>SSMS ile bağlanmak için
 SSMS kullanırken, sunucunuza ilk kez bağlanmadan önce, kullanıcı adınızın Analiz Hizmetleri Yöneticileri grubuna dahil edildiğinden emin olun. Daha fazla bilgi edinmek için bu makalenin ilerleyen saatlerinde [Sunucu yöneticileri ve veritabanı kullanıcılarına](#server-administrators-and-database-users) bakın.

1. Bağlanmadan önce sunucu adını almanız gerekir. **Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
    ![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. SSMS > **Nesne Gezgini**'nde **Bağlan** > **Analysis Services**'e tıklayın.
3. **Sunucuya Bağlan** iletişim kutusunda, sunucu adına yapıştırın, ardından **Kimlik Doğrulama'da**aşağıdaki kimlik doğrulama türlerinden birini seçin:   
    > [!NOTE]
    > Kimlik doğrulama türü, **Active Directory - MFA desteği ile evrensel**, önerilir.

    > [!NOTE]
    > Bir Microsoft Hesabı, Live ID, Yahoo, Gmail vb. ile oturum açın, parola alanını boş bırakın. Bağlan'ı tıklattıktan sonra bir parola istenir.

    Windows etki alanı\kullanıcı adı ve parola kimlik bilgilerinizi kullanmak için **Windows Kimlik Doğrulama.**

    Kuruluş hesabı kullanmak için **Etkin Dizin Parola Kimlik Doğrulaması.** Örneğin, etki alanı olmayan bir bilgisayardan bağlanırken.

    Active Directory - [Etkileşimli olmayan veya çok faktörlü kimlik doğrulamasını](../sql-database/sql-database-ssms-mfa-authentication.md)kullanmak için **MFA desteği ile evrensel.** 
   
    ![SSMS'de bağlanma](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Sunucu yöneticileri ve veritabanı kullanıcıları
Azure Çözümleme Hizmetleri'nde iki tür kullanıcı, sunucu yöneticileri ve veritabanı kullanıcıları vardır. Her iki kullanıcı türü de Azure Etkin Dizininiz'de olmalı ve kuruluş e-posta adresi veya UPN tarafından belirtilmelidir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Bağlantı sorunlarını giderme
SSMS kullanarak bağlanırken, sorunlarla karşılaştıysanız, oturum açma önbelleğini temizlemeniz gerekebilir. Diske hiçbir şey önbelleğe alınmış değil. Önbelleği temizlemek için bağlantı işlemini kapatın ve yeniden başlatın. 

## <a name="next-steps"></a>Sonraki adımlar
Yeni sunucunuza zaten bir tabular modeli dağıtmadıysanız, şimdi iyi bir zaman. Daha fazla bilgi için bkz. [Azure Analysis Services’a Dağıtma](analysis-services-deploy.md).

Sunucunuza bir model dağıttıysanız, bir istemci veya tarayıcı kullanarak bu modele bağlanmaya hazırsınız. Daha fazla bilgi için azure [analiz hizmetleri sunucusundan veri alın'](analysis-services-connect.md)a bakın.

