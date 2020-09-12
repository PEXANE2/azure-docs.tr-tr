---
title: Azure Analysis Services sunucularına bağlanma | Microsoft Docs
description: Azure 'da bir Analysis Services sunucusuna bağlanmayı ve bu sunucudan veri almayı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: owend
ms.reviewer: minewiskan
ms.custom: references_regions
ms.openlocfilehash: 71caad8ce650b86f4350b32974bb8d980538b223
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489026"
---
# <a name="connecting-to-servers"></a>Sunuculara bağlanma

Bu makalede, SQL Server Management Studio (SSMS) veya Visual Studio gibi Analysis Services projelerine ya da Microsoft Excel, Power BI Desktop ya da özel uygulamalar gibi istemci raporlama uygulamalarıyla birlikte veri modelleme ve yönetim uygulamaları kullanarak bir sunucuya bağlanma açıklanır. Azure Analysis Services bağlantıları HTTPS kullanır.

## <a name="client-libraries"></a>İstemci kitaplıkları

[En son Istemci kitaplıklarını al](https://docs.microsoft.com/analysis-services/client-libraries?view=azure-analysis-services-current)

Türüne bakılmaksızın sunucuya yapılan tüm bağlantılar, bir Analysis Services sunucusuyla bağlantı kurmak için güncelleştirilmiş AMO, ADOMD.NET ve OLEDB istemci kitaplıklarını gerektirir. SSMS, Visual Studio, Excel 2016 ve üzeri ve Power BI için en son istemci kitaplıkları, aylık yayınlar yüklenir veya güncelleştirilir. Ancak bazı durumlarda, bir uygulamanın en son sürümü bulunmayabilir. Örneğin, ilkelerin güncelleştirmeleri ertelenmesi veya Office 365 güncelleştirmeleri ertelenmiş kanaldır.

> [!NOTE]
> İstemci kitaplıkları, Kullanıcı adı ve parola gerektiren proxy sunucuları aracılığıyla Azure Analysis Services bağlanamaz. 

## <a name="server-name"></a>Sunucu adı

Azure 'da bir Analysis Services sunucusu oluşturduğunuzda, benzersiz bir ad ve sunucunun oluşturulacağı bölgeyi belirtirsiniz. Sunucu adını bir bağlantıda belirtirken, sunucu adlandırma şeması şu şekilde olur:

```
<protocol>://<region>/<servername>
```
 Protokol, bir dize **asazure**olduğunda, bölgesi sunucunun oluşturulduğu URI 'dir (örneğin, westus.asazure.Windows.net) ve ServerName bölge içindeki benzersiz sunucunuzun adıdır.

### <a name="get-the-server-name"></a>Sunucu adını al

**Azure Portal** > Server > **genel bakış**  >  **sunucu adı**' nda, tüm sunucu adını kopyalayın. Kuruluşunuzdaki diğer kullanıcılar da bu sunucuya bağlanıyorsa, bu sunucu adını bunlarla paylaşabilirsiniz. Bir sunucu adı belirtirken, yolun tamamının kullanılması gerekir.

![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

> [!NOTE]
> Doğu ABD 2 bölgenin Protokolü **aspaaseastus2**.

## <a name="connection-string"></a>Bağlantı dizesi

Tablosal nesne modeli kullanarak Azure Analysis Services bağlanırken aşağıdaki bağlantı dizesi biçimlerini kullanın:

###### <a name="integrated-azure-active-directory-authentication"></a>Tümleşik Azure Active Directory kimlik doğrulaması

Tümleşik kimlik doğrulaması, varsa Azure Active Directory kimlik bilgisi önbelleğini seçer. Aksi takdirde, Azure oturum açma penceresi gösterilir.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Kullanıcı adı ve parolayla kimlik doğrulaması Azure Active Directory

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows kimlik doğrulaması (tümleşik güvenlik)

Geçerli işlemi çalıştıran Windows hesabını kullanın.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>. Odc dosyası kullanarak bağlanma

Excel 'in daha eski sürümleriyle, kullanıcılar bir Office veri bağlantısı (. odc) dosyası kullanarak bir Azure Analysis Services sunucusuna bağlanabilir. Daha fazla bilgi için bkz. [Office veri bağlantısı (. odc) dosyası oluşturma](analysis-services-odc.md).

## <a name="connect-as-a-linked-server-from-sql-server"></a>SQL Server bağlı sunucu olarak bağlanma

SQL Server, veri kaynağı sağlayıcısı olarak MSOLAP belirterek, bir Azure Analysis Services kaynağına [bağlı sunucu](https://docs.microsoft.com/sql/relational-databases/linked-servers/create-linked-servers-sql-server-database-engine) olarak bağlanabilir. Bağlı bir sunucu bağlantısını yapılandırmadan önce, en son [MSOLAP istemci kitaplığını](https://docs.microsoft.com/analysis-services/client-libraries?view=azure-analysis-services-current) (sağlayıcı) yüklediğinizden emin olun. 

Azure Analysis Services bağlı sunucu bağlantıları için, MSOLAP sağlayıcısı SQL Server işleminin dışında oluşturulmalıdır. Bağlı sunucu seçeneklerini yapılandırırken, **InProcess 'e Izin ver** seçeneğinin **Seçili**olmadığından emin olun.

**InProcess 'e Izin ver** seçilirse ve sağlayıcı SQL Server işlemde örneklenmiştir, aşağıdaki hata döndürülür:

```
OLE DB provider "MSOLAP" for linked server "(null)" returned message "The following system error occurred: ".

OLE DB provider "MSOLAP" for linked server "(null)" returned message "The connection failed because user credentials are needed and Sign-In UI is not allowed.".

Msg 7303, Level 16, State 1, Line 2
Cannot initialize the data source object of OLE DB provider "MSOLAP" for linked server "(null)".
```



## <a name="next-steps"></a>Sonraki adımlar

[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI bağlanma](analysis-services-connect-pbi.md)   
[Sunucunuzu Yönetin](analysis-services-manage.md)   

