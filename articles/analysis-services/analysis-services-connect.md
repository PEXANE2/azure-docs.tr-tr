---
title: Azure Analiz Hizmetleri sunucularına bağlanma| Microsoft Dokümanlar
description: Azure'daki bir Analiz Hizmetleri sunucusuna nasıl bağlanabileceğinizi ve veri alacağınızı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3231b46060cbb755ada000473c8fbe873cc51ef1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73147304"
---
# <a name="connecting-to-servers"></a>Sunuculara bağlanma

Bu makalede, SQL Server Management Studio (SSMS) veya Visual Studio gibi veri modelleme ve yönetim uygulamalarını Analiz Hizmetleri projeleri ile kullanarak veya Microsoft Excel, Power BI gibi istemci raporlama uygulamaları yla bağlantı kurma Masaüstü veya özel uygulamalar. Azure Analiz Hizmetleri bağlantıları HTTPS'yi kullanır.

## <a name="client-libraries"></a>İstemci kitaplıkları

[En son İstemci kitaplıklarını alın](analysis-services-data-providers.md)

Türüne bakılmaksızın bir sunucuya yapılan tüm bağlantılar, bir Analiz Hizmetleri sunucusuna bağlanmak ve arabirim kurmak için güncelleştirilmiş AMO, ADOMD.NET ve OLEDB istemci kitaplıklarını gerektirir. SSMS, Visual Studio, Excel 2016 ve sonraki sürümler ve Power BI için en son istemci kitaplıkları aylık sürümlerle yüklenir veya güncelleştirilir. Ancak, bazı durumlarda, bir uygulama nın en son olmayabilir. Örneğin, ilkeler güncelleştirmeleri geciktirdiğinde veya Office 365 güncelleştirmeleri Ertelenmiş Kanal'da olduğunda.

## <a name="server-name"></a>Sunucu adı

Azure'da bir Çözümleme Hizmetleri sunucusu oluşturduğunuzda, benzersiz bir ad ve sunucunun oluşturulacağı bölge belirtirsiniz. Bağlantıda sunucu adı belirtilirken, sunucu adlandırma düzeni:

```
<protocol>://<region>/<servername>
```
 Protokolün string **asazure**olduğu yerde, bölge sunucunun oluşturulduğu Uri'dir (örneğin, westus.asazure.windows.net) ve sunucu adı bölge içindeki benzersiz sunucunuzun adıdır.

### <a name="get-the-server-name"></a>Sunucu adını alma

**Azure portalı>** sunucusunda **Genel Bakış** > **Sunucusu adı**>, sunucu adının tamamını kopyalayın. Kuruluşunuzdaki diğer kullanıcılar da bu sunucuya bağlanıyorsa, bu sunucu adını onlarla paylaşabilirsiniz. Bir sunucu adı belirtilirken, tüm yolun kullanılması gerekir.

![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

> [!NOTE]
> Doğu ABD 2 bölgesi için protokol **aspaaseastus2**olduğunu.

## <a name="connection-string"></a>Bağlantı dizesi

Tabular Nesne Modeli'ni kullanarak Azure Çözümleme Hizmetleri'ne bağlanırken aşağıdaki bağlantı dize biçimlerini kullanın:

###### <a name="integrated-azure-active-directory-authentication"></a>Tümleşik Azure Active Directory kimlik doğrulaması

Tümleşik kimlik doğrulama, varsa Azure Active Directory kimlik önbelleğini alır. Değilse, Azure giriş penceresi gösterilir.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Kullanıcı adı ve parola içeren Azure Active Directory kimlik doğrulaması

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows kimlik doğrulama (Tümleşik güvenlik)

Geçerli işlemi çalıştıran Windows hesabını kullanın.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>.odc dosyakullanarak bağlanma

Excel'in eski sürümlerinde, kullanıcılar bir Office Veri Bağlantısı (.odc) dosyasını kullanarak Azure Çözümleme Hizmetleri sunucusuna bağlanabilir. Daha fazla bilgi için [bkz.](analysis-services-odc.md)


## <a name="next-steps"></a>Sonraki adımlar

[Excel ile bağlan](analysis-services-connect-excel.md)    
[Power BI ile bağlanın](analysis-services-connect-pbi.md)   
[Sunucunuzu yönetme](analysis-services-manage.md)   

