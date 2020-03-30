---
title: Azure Veri Kataloğu sorun giderme
description: Bu makalede, Azure Veri Kataloğu kaynakları için sık karşılaşılan sorun giderme kaygıları açıklanmaktadır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: troubleshooting
ms.date: 08/01/2019
ms.openlocfilehash: 84bd14f8ae18527b4f6e9d8509a12555baec8771
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68879547"
---
# <a name="troubleshooting-azure-data-catalog"></a>Azure Veri Kataloğu sorunlarını giderme

Bu makalede, Azure Veri Kataloğu kaynakları için sık karşılaşılan sorun giderme kaygıları açıklanmaktadır. 

## <a name="functionality-limitations"></a>İşlevsellik sınırlamaları

Azure Veri Kataloğu'nu kullanırken aşağıdaki işlevler sınırlıdır:

- **Konuk Rolü** türüne sahip hesaplar desteklenmez. Azure Veri Kataloğu kullanıcısı olarak konuk hesapları ekleyemezsiniz ve [https://www.azuredatacatalog.com](https://www.azuredatacatalog.com)konuk kullanıcılar portalı .

- Azure Kaynak Yöneticisi Şablonları veya Azure PowerShell komutlarını kullanarak Azure Veri Kataloğu kaynakları oluşturma desteklenmez.

- Azure Veri Kataloğu kaynağı Azure Kiracıları arasında taşınamaz.

## <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory ilke yapılandırması

Azure Veri Kataloğu portalında oturum açabildiğiniz, ancak veri kaynağı kayıt aracında oturum açmak istediğinizde oturum açmanızı engelleyen bir hata iletisiyle karşılaştığınız bir durum yaşayabilirsiniz. Bu hata, şirket ağındayken veya şirket ağının dışından bağlanırken oluşabilir.

Kayıt aracı, kullanıcının Azure Active Directory ile oturum açtığını doğrulamak için *form kimlik doğrulaması* kullanır. Başarılı bir oturum açma için Azure Active Directory yöneticisinin *genel kimlik doğrulama ilkesinde* form kimlik doğrulamasını etkinleştirmesi gerekir.

Genel kimlik doğrulama ilkesi ile aşağıdaki görüntüde gösterildiği gibi kimlik doğrulamasını intranet ve extranet bağlantıları için ayrı ayrı etkinleştirebilirsiniz. Bağlandığınız ağ için form kimlik doğrulaması etkinleştirilemezse oturum açma hataları oluşabilir.

 ![Azure Active Directory genel kimlik doğrulama ilkesi](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Daha fazla bilgi için bkz. [Kimlik doğrulama ilkelerini yapılandırma](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Kataloğu oluşturma](data-catalog-get-started.md)
