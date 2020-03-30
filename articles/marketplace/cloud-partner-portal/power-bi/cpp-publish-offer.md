---
title: Power BI App teklifini yayınlayın | Azure Marketi
description: Microsoft AppSource pazarda power bi uygulaması teklifi yayımlayın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: dsindona
ms.openlocfilehash: 89d437fe74d209e0dc04ffc590a1e32426b28732
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275875"
---
# <a name="publish-a-power-bi-app-offer"></a>Power BI App teklifi yayınlama

Bulut İş Ortağı Portalı'nda bir teklif tanımladıktan ve ilişkili teknik varlıkları oluşturduktan sonra son adım, teklifi yayımlama için göndermektir. Bu işlemi başlatmak için, **Yeni Teklif** penceresinin sol bölmesinde **Yayımla'yı**seçin. Daha fazla bilgi için bkz: [Azure Marketi ve AppSource tekliflerini yayımla.](../manage-offers/cpp-publish-offer.md)


## <a name="publishing-steps"></a>Yayımlama adımları

Yayımlama işleminin ana adımları şunlardır:

![Power BI App teklifi için yayımlama süreci adımları](./media/publishing-process-steps.png)

Bu tablo her adımı açıklar ve tahmini tamamlanma süresini sağlar:

|   Yayımlama adımı            |   Zaman     |   Açıklama                                                                  |
| --------------------         |------------| ----------------                                                               |
| Ön koşulları doğrula       | 15 dakika     | Teklif bilgileri ve teklif ayarları doğrulanır.                            |
| Sertifika                | 1-7 gün   | Power BI Belgelendirme Ekibi teklifinizi analiz eder. Ekip, power bi uygulamanızı sağlanan yükleme URL'si üzerinden uygulamayı yükleyerek manuel doğrulama testi ile çalıştırın. Birincil doğrulamalar uygulama sertifika sürecinin bir parçası olarak gerçekleştirilir (daha sonra bu belgede açıklanmıştır).         |
| Paketleme                    | \<1 saat  | Teklifin teknik varlıkları müşteri kullanımı için paketlenmiştir.                        |
| Müşteri adayı oluşturma kaydı | \<1 saat  | Müşteri adayı sistemleri yapılandırılır ve dağıtılır.                                      |
| Yayıncı oturum kapatma            | \-         | Teklif yayından önce son bir incelemeyi ve onayı tamamlarsınız. Ayrıca artık teklifinizi önizlemek için bir bağlantınız olacak. Önizlemenizin görünüşünden memnun olduktan sonra **Durum** sekmesinde **Canlı'yı** seçin. Bu, uygulamanızı AppSource'ta listelemek için binen ekibe bir istek gönderir.    |
| Canlı                         | \<3 saat | Teklifiniz artık AppSource'ta herkese açık ("canlı") listelenmiştir ve müşteriler uygulamanızı görüntüleyebilir ve Power BI aboneliklerinde dağıtabilirler. Ayrıca bir onay e-postası alırsınız. **Tüm teklifler** sekmesindeki sağ sütunda, tüm tekliflerinizin durumunu görebilirsiniz. **Durum** sekmesinde, teklifiniz için ayrıntılı yayımlama akışı durumunu görebilirsiniz. |
|   |   |

Bu işlemin tamamlanması için sekiz güne kadar bekleyin. Bu yayımlama adımlarını inceledikten sonra, Power BI App teklifiniz [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) Power BI uygulamaları bölümünde listelenir.


### <a name="app-certification-process"></a>Uygulama sertifika süreci

Microsoft onboarding ekibi Power BI App teklif inizi doğrulamak için bu işlemi kullanır:

1. Yasal belgeleri ve yardım bağlantılarını gözden geçirin.
2. Destek kişi bilgilerini doğrulayın.
3. Uygun yüklemeyi doğrulamak için yükleyici URL'sini kullanın.
4. Uygulamayı kötü amaçlı yazılımlar ve diğer kötü amaçlı içerikler için tarar.
5. Görüntülenen içeriğin uygulamanın açıklamasıyla eşleştiğini doğrulayın.
6. Uygulamayla ilgili işlemlerin Power BI'de beklendiği gibi çalıştığını doğrulayın. Takım, örnek verilerle raporlar ve panolar açar, özel veri kaynaklarına bağlanır, verileri yeniler ve benzeri.

Sertifika Ekibi herhangi bir sorun bulurlarsa geri bildirim sağlar.  Power BI uygulama gereksinimleri hakkında daha fazla bilgi için [Power BI uygulama belgelerine](https://go.microsoft.com/fwlink/?linkid=2028636)bakın.


## <a name="next-steps"></a>Sonraki adımlar

[Uygulamanızı AppSource pazar yerinde](https://appsource.microsoft.com)düzenli olarak izlemenizi öneririz.  [Ayrıca,](https://cloudpartner.azure.com/#insights) pazar yeri müşterileriniz ve uygulama kullanımınız hakkında bilgi almak için Bulut İş Ortağı Portalı'nın [Satıcı Öngörüleri](../../cloud-partner-portal-orig/si-getting-started.md) özelliğini de kullanmalısınız. Son olarak, [teklifinizi güncelleyebilirsiniz.](./cpp-update-existing-offer.md)
