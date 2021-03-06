---
title: Autentiseringsmetoder för telefonen – Azure Active Directory
description: Lär dig mer om hur du använder autentiseringsmetoder i Azure Active Directory för att förbättra och säkra inloggnings händelser
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4510dac833d0576273472b6f925d546b55257614
ms.sourcegitcommit: 230d5656b525a2c6a6717525b68a10135c568d67
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/19/2020
ms.locfileid: "94886812"
---
# <a name="authentication-methods-in-azure-active-directory---phone-options"></a>Autentiseringsmetoder i Azure Active Directory telefon alternativ

För direkt autentisering med textmeddelande kan du [Konfigurera och aktivera användare för SMS-baserad autentisering (för hands version)](howto-authentication-sms-signin.md). SMS-baserad inloggning är perfekt för anställda. Med SMS-baserad inloggning behöver användarna inte känna till ett användar namn och lösen ord för att få åtkomst till program och tjänster. Användaren anger i stället sitt registrerade mobiltelefon nummer, tar emot ett SMS med en verifierings kod och anger det i inloggnings gränssnittet.

Användarna kan också kontrol lera att de använder en mobil telefon eller en arbets telefon som sekundär form av autentisering som används under Azure AD Multi-Factor Authentication eller lösen ords återställning via självbetjäning (SSPR).

För att fungera korrekt måste telefonnumret vara i formatet *+ CountryCode telefonnummer*, till exempel *+ 1 4251234567*.

> [!NOTE]
> Det måste finnas ett blank steg mellan lands-/region koden och telefonnumret.
>
> Lösen ords återställning stöder inte telefon tillägg. Även i formatet *+ 1 4251234567X12345* tas tilläggen bort innan anropet placeras.

## <a name="mobile-phone-verification"></a>Mobil telefon verifiering

För Azure AD Multi-Factor Authentication eller SSPR kan användarna välja att ta emot ett SMS med en verifierings kod för att ange inloggnings gränssnittet eller ta emot ett telefonsamtal.

Om användarna inte vill att deras mobiltelefon nummer ska synas i katalogen, men vill använda det för lösen ords återställning, behöver inte administratörerna fylla i telefonnumret i katalogen. I stället ska användarna fylla i sina attribut för **autentisering** via den kombinerade registreringen av säkerhets information på [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) . Administratörer kan se den här informationen i användarens profil, men den publiceras inte någon annan stans.

:::image type="content" source="media/concept-authentication-methods/user-authentication-methods.png" alt-text="Skärm bild av Azure Portal som visar autentiseringsmetoder med ett telefonnummer som är ifyllt":::

Microsoft garanterar inte konsekvent SMS eller Voice-baserad Azure AD Multi-Factor Authentication prompt med samma nummer. Vi kan när som helst lägga till eller ta bort korta koder när vi gör väg justeringar för att förbättra SMS-leveranser. Microsoft stöder inte korta koder för länder/regioner utöver USA och Kanada.

### <a name="text-message-verification"></a>Verifiering av textmeddelande

Med text meddelande verifiering under SSPR eller Azure AD Multi-Factor Authentication skickas ett SMS till det mobiltelefon nummer som innehåller en verifierings kod. För att slutföra inloggnings processen anges den angivna verifierings koden i inloggnings gränssnittet.

### <a name="phone-call-verification"></a>Verifiering av telefonsamtal

Med Telefonsamtals verifiering under SSPR eller Azure AD Multi-Factor Authentication görs ett automatiskt röst samtal till det telefonnummer som registrerats av användaren. Användaren uppmanas att trycka på # på deras knapps ATS för att slutföra inloggnings processen.

## <a name="office-phone-verification"></a>Office Phone-verifiering

Med Telefonsamtals verifiering under SSPR eller Azure AD Multi-Factor Authentication görs ett automatiskt röst samtal till det telefonnummer som registrerats av användaren. Användaren uppmanas att trycka på # på deras knapps ATS för att slutföra inloggnings processen.

## <a name="troubleshooting-phone-options"></a>Felsöka telefonalternativ

Om du har problem med autentiseringen av telefonen för Azure AD kan du läsa följande fel söknings steg:

* "Du har nått vår gräns för verifierings anrop" eller "du har nått vår gräns för text verifierings koder" fel meddelanden under inloggningen
   * Microsoft kan begränsa upprepade autentiseringsförsök som utförs av samma användare under en kort tids period. Den här begränsningen gäller inte för Microsoft Authenticator eller verifierings koden. Om du har nått dessa gränser kan du använda Authenticator-appen, verifierings koden eller försöka logga in igen om några minuter.
* Blockerat anropar-ID på en enskild enhet.
   * Granska eventuella blockerade tal som kon figurer ATS på enheten.
* Fel telefonnummer eller felaktig landskod, eller förvirring mellan personligt telefonnummer och arbets telefonnummer.
   * Felsök objektet användare och konfigurerade autentiseringsmetoder. Kontrol lera att rätt telefonnummer har registrerats.
* Fel PIN-kod har angetts.
   * Bekräfta att användaren har använt rätt PIN-kod som registrerad för sitt konto (endast MFA Server-användare).
* Samtalet har vidarebefordrats till röst meddelanden.
   * Se till att användaren har aktiverat telefonen och att tjänsten är tillgänglig i deras Area eller Använd en annan metod.
* Användaren har blockerats
   * Låt en Azure AD-administratör häva blockeringen av användaren i Azure Portal.
* SMS prenumererar inte på enheten.
   * Låt användaren ändra metoder eller aktivera SMS på enheten.
* Defekta tele leverantörer, till exempel inga telefonin uppgifter identifierade, problem med DTMF-toner som saknas, blockerat anrops-ID på flera enheter eller blockerat SMS över flera enheter.
   * Microsoft använder flera telekom-leverantörer för att skicka telefonsamtal och SMS-meddelanden för autentisering. Om du ser något av ovanstående problem, kan en användare försöka använda metoden minst fem gånger inom 5 minuter och ha den användarens information tillgänglig när de kontaktar Microsoft support.

## <a name="next-steps"></a>Nästa steg

Information om hur du kommer igång finns i [självstudien för självbetjäning för återställning av lösen ord (SSPR)][tutorial-sspr] och [Azure AD Multi-Factor Authentication][tutorial-azure-mfa].

Mer information om SSPR-koncept finns i [hur Azure AD självbetjäning för återställning av lösen ord fungerar][concept-sspr].

Mer information om MFA-koncept finns i [hur Azure AD Multi-Factor Authentication fungerar][concept-mfa].

Läs mer om hur du konfigurerar autentiseringsmetoder med hjälp av [Microsoft Graph REST API beta](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta&preserve-view=true).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
