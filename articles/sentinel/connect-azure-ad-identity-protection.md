---
title: Azure AD Kimlik Koruması verilerini Azure Sentinel'e bağlayın
description: Azure AD Kimlik Koruması verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
author: yelevin
manager: rkarlin
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 11/17/2019
ms.author: yelevin
ms.openlocfilehash: b82ddfef57efaaca0ae43750cd306a63a772b911
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616819"
---
# <a name="connect-data-from-azure-ad-identity-protection"></a>Azure AD Kimlik Koruması'ndan veri bağlama



Panoları görüntülemek, özel uyarılar oluşturmak ve araştırmayı iyileştirmek için uyarıları Azure Sentinel'e aktarmak için [Azure AD Identity Protection'dan](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection) Azure Sentinel'e günlük akışı sağlayabilirsiniz. Azure Active Directory Identity Protection, risk kullanıcılarına, risk algılamalarına ve güvenlik açıklarına yönelik konsolide bir görünüm sağlar, riskleri hemen düzeltebilir ve gelecekteki olayları otomatik olarak düzeltmek için ilkeler belirler. Hizmet, Microsoft'un tüketici kimliklerini koruma deneyimi üzerine kuruludur ve günde 13 milyardan fazla oturum açma sinyalinden muazzam doğruluk elde eder. 


## <a name="prerequisites"></a>Ön koşullar

- Azure Active [Directory Premium P1 veya P2 lisansın](https://azure.microsoft.com/pricing/details/active-directory/) olmalıdır
- Genel yönetici veya güvenlik yöneticisi izinleri olan kullanıcı


## <a name="connect-to-azure-ad-identity-protection"></a>Azure AD Kimlik Koruması'na bağlanın

Azure AD Kimlik Koruması zaten varsa, [ağınızda etkinleştirildiğinden](../active-directory/identity-protection/overview-identity-protection.md)emin olun.
Azure AD Kimlik Koruması dağıtılıyorsa ve veri alıyorsanız, uyarı verileri kolayca Azure Sentinel'e aktarılabilir.


1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **Azure AD Kimlik Koruması** döşemesini tıklatın.

2. Azure AD Kimlik Koruması etkinliklerini Azure Sentinel'e aktarmaya başlamak için **Bağlan'ı** tıklatın.


6. Azure AD Kimlik Koruması uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **SecurityAlert'i**arayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure AD Kimlik Koruması'nı Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
