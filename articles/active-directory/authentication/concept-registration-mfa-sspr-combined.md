---
title: Kombinierte Registrierung für SSPR und Azure Multi-Factor Authentication – Azure Active Directory
description: Erfahren Sie mehr über die Umgebung für die kombinierte Registrierung für Azure Active Directory, mit der sich Benutzer sowohl für Azure Multi-Factor Authentication (MFA) als auch für die Self-Service-Kennwortzurücksetzung registrieren können
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71900bc3741c25299402e31057ea2237cd0a4637
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91965147"
---
# <a name="combined-security-information-registration-for-azure-active-directory-overview"></a>Übersicht über die kombinierte Registrierung von Sicherheitsinformationen in Azure Active Directory

Vor der kombinierten Registrierung registrierten Benutzer Authentifizierungsmethoden für Azure Multi-Factor Authentication (MFA) und die Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) getrennt voneinander. Benutzer waren verwirrt, dass ähnliche Methoden für Multi-Factor Authentication und für SSPR verwendet wurden, sie sich jedoch für beide Funktionen registrieren mussten. Mit der kombinierten Registrierung können sich Benutzer jetzt einmalig registrieren und die Vorteile von mehrstufiger Authentifizierung und SSPR nutzen.

> [!NOTE]
> Ab dem 15. August 2020 werden alle neuen Azure AD-Mandanten automatisch für die kombinierte Registrierung aktiviert.

In diesem Artikel wird beschrieben, was die kombinierte Registrierung von Sicherheitsinformationen ist. Erste Informationen zur kombinierten Registrierung von Sicherheitsinformationen finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Aktivieren der kombinierten Registrierung von Sicherheitsinformationen](howto-registration-mfa-sspr-combined.md)

![Eigenes Profil zeigt registrierte Sicherheitsinformationen für einen Benutzer an](media/concept-registration-mfa-sspr-combined/combined-security-info-defualts-registered.png)

Lesen Sie vor dem Aktivieren der neuen Funktion diese an Administratoren gerichtete Dokumentation und die benutzerorientierte Dokumentation, um sich mit der Funktionalität und den Auswirkungen dieser Funktion vertraut zu machen. Ziehen Sie für Ihre Schulungen die [Benutzerdokumentation](../user-help/security-info-setup-signin.md) heran, um Ihre Benutzer auf die neue Oberfläche vorzubereiten und so einen erfolgreichen Rollout sicherzustellen.

Die kombinierte Azure AD-Registrierung von Sicherheitsinformationen ist für nationale Clouds wie Azure US Government, Azure Deutschland oder Azure China 21Vianet derzeit nicht verfügbar.

> [!IMPORTANT]
> Benutzern, die für die ursprüngliche Vorschau und für die erweiterte kombinierte Registrierungsumgebung aktiviert sind, wird das neue Verhalten angezeigt. Benutzern, die für beide Umgebungen aktiviert sind, wird nur die neue Umgebung „Mein Profil“ angezeigt. Die neue Oberfläche *Mein Profil* entspricht dem Erscheinungsbild der kombinierten Registrierung und bietet Benutzern eine nahtlose Umgebung. Benutzer können „Mein Profil“ anzeigen, indem sie zu [https://myprofile.microsoft.com](https://myprofile.microsoft.com) navigieren.
>
> Möglicherweise erhalten Sie eine Fehlermeldung (z. B. „Anmeldung nicht möglich“), wenn Sie versuchen, auf die Option „Sicherheitsinformation“ zuzugreifen. Vergewissern Sie sich in diesem Fall, dass Cookies von Drittanbietern im Webbrowser nicht durch ein Konfigurations- oder Gruppenrichtlinienobjekt blockiert werden.

Die Seiten von *Mein Profil* sind entsprechend den Spracheinstellungen auf dem Computer lokalisiert, der für den Zugriff auf die Seite verwendet wird. Microsoft speichert die aktuell verwendete Sprache im Browsercache, sodass nachfolgende Zugriffsversuche auf die Seiten weiterhin in der zuletzt verwendeten Sprache angezeigt werden. Wenn Sie den Cache löschen, werden die Seiten neu gerendert.

Wenn Sie eine bestimmte Sprache erzwingen möchten, können Sie am Ende der URL `?lng=<language>` hinzufügen. Dabei ist `<language>` der Code der Sprache, in der Sie rendern möchten.

![Einrichten von SSPR oder anderen Sicherheitsüberprüfungsmethoden](media/howto-registration-mfa-sspr-combined/combined-security-info-my-profile.png)

## <a name="methods-available-in-combined-registration"></a>Verfügbare Methoden bei der kombinierten Registrierung

Bei der kombinierten Registrierung werden die folgenden Authentifizierungsmethoden und -aktionen unterstützt:

| Methode | Register | Change | Löschen |
| --- | --- | --- | --- |
| Microsoft Authenticator | Ja (maximal 5) | Nein | Ja |
| Andere Authenticator-App | Ja (maximal 5) | Nein | Ja |
| Hardwaretoken | Nein | Nein | Ja |
| Phone | Ja | Ja | Ja |
| Alternatives Telefon | Ja | Ja | Ja |
| Bürotelefon | Nein | Nein | Nein |
| Email | Ja | Ja | Ja |
| Sicherheitsfragen | Ja | Nein | Ja |
| App-Kennwörter | Ja | Nein | Ja |
| FIDO2-Sicherheitsschlüssel<br />*Verwalteter Modus nur auf der Seite [Sicherheitsinformation](https://mysignins.microsoft.com/security-info)“*| Ja | Ja | Ja |

> [!NOTE]
> App-Kennwörter sind nur für Benutzer verfügbar, für die Multi-Factor Authentication erzwungen wurde. App-Kennwörter sind nicht verfügbar für Benutzer, für die Multi-Factor Authentication über eine Richtlinie für bedingten Zugriff aktiviert wurde.

Benutzer können eine der folgenden Optionen als Standardmethode für die mehrstufige Authentifizierung festlegen:

- Microsoft Authenticator – Benachrichtigung
- Authenticator-App oder Hardwaretoken – Code
- Telefonanruf
- Textnachricht

Da wir weiterhin weitere Authentifizierungsmethoden zu Azure AD hinzufügen, werden dann auch diese Methoden bei der kombinierten Registrierung verfügbar sein.

## <a name="combined-registration-modes"></a>Modi der kombinierten Registrierung

Zwei Modi der kombinierten Registrierung sind verfügbar: Interruptmodus und Verwaltungsmodus.

- Beim **Interruptmodus** handelt es sich um eine assistentenähnliche Oberfläche, die für Benutzer angezeigt wird, wenn sie ihre Sicherheitsinformationen bei der Anmeldung registrieren oder aktualisieren.
- Der **Verwaltungsmodus** ist Teil des Benutzerprofils und ermöglicht Benutzern die Verwaltung ihrer Sicherheitsinformationen.

In beiden Modi müssen Benutzer, die bereits eine Methode registriert haben, die für die mehrstufige Authentifizierung verwendet werden kann, eine mehrstufige Authentifizierung ausführen, bevor sie auf ihre Sicherheitsinformationen zugreifen können. Benutzer müssen ihre Informationen bestätigen, bevor sie ihre zuvor registrierten Methoden weiterhin verwenden können. 

### <a name="interrupt-mode"></a>Interruptmodus

Bei der kombinierten Registrierung werden sowohl MFA- (Multi-Factor Authentication) als auch SSPR-Richtlinien eingehalten, wenn beide für Ihren Mandanten aktiviert sind. Über diese Richtlinien wird gesteuert, ob die Anmeldung eines Benutzers unterbrochen und dieser aufgefordert wird, die Registrierung durchzuführen, und welche Methoden für die Registrierung verfügbar sind.

Im Folgenden sind mehrere Beispielszenarien aufgeführt, in denen Benutzer möglicherweise aufgefordert werden, ihre Sicherheitsinformationen zu registrieren oder zu aktualisieren:

- *Durch Identity Protection erzwungene MFA-Registrierung:* Benutzer werden bei der Anmeldung aufgefordert, die Registrierung durchzuführen. Sie registrieren MFA-Methoden (Multi-Factor Authentication) und SSPR-Methoden (sofern sie für SSPR aktiviert sind).
- *Durch benutzerspezifische mehrstufige Authentifizierung erzwungene MFA-Registrierung:* Benutzer werden bei der Anmeldung aufgefordert, die Registrierung durchzuführen. Sie registrieren MFA-Methoden (Multi-Factor Authentication) und SSPR-Methoden (sofern sie für SSPR aktiviert sind).
- *Durch Richtlinien für bedingten Zugriff oder andere Richtlinien erzwungene MFA-Registrierung:* Benutzer werden zur Registrierung aufgefordert, wenn sie eine Ressource verwenden, die Multi-Factor Authentication erfordert. Sie registrieren MFA-Methoden (Multi-Factor Authentication) und SSPR-Methoden (sofern sie für SSPR aktiviert sind).
- *Erzwungene SSPR-Registrierung:* Benutzer werden bei der Anmeldung aufgefordert, die Registrierung durchzuführen. Sie registrieren nur SSPR-Methoden.
- *Erzwungene SSPR-Aktualisierung:* Benutzer werden in einem vom Administrator festgelegten Intervall aufgefordert, ihre Sicherheitsinformationen zu überprüfen. Die Benutzer prüfen ihre Informationen und können diese bestätigen oder bei Bedarf ändern.

Wenn die Registrierung erzwungen wird, wird den Benutzern die Mindestanzahl der Methoden angezeigt (angefangen bei der sichersten bis zur am wenigsten sicheren Methode), die erforderlich sind, um den MFA- (Multi-Factor Authentication) sowie den SSPR-Richtlinien zu entsprechen.

Betrachten Sie das folgende Beispielszenario:

- Ein Benutzer ist für SSPR aktiviert. Die SSPR-Richtlinie erfordert zwei Methoden zum Zurücksetzen und aktiviert den Code der mobilen App sowie E-Mail und Telefon.
- Der Benutzer muss zwei Methoden registrieren.
   - Standardmäßig werden dem Benutzer die Authenticator-App und das Telefon angezeigt.
   - Der Benutzer hat die Möglichkeit, statt der Authenticator-App oder des Telefons die E-Mail-Adresse zu registrieren.

Im folgenden Flussdiagramm sind die Methoden beschrieben, die einem Benutzer angezeigt werden, wenn seine Anmeldung unterbrochen und er aufgefordert wird, die Registrierung durchzuführen:

![Flussdiagramm für kombinierte Sicherheitsinformationen](media/concept-registration-mfa-sspr-combined/combined-security-info-flow-chart.png)

Wenn Sie Multi-Factor Authentication und SSPR aktiviert haben, sollten Sie die MFA-Registrierung erzwingen.

Wenn Benutzer über die SSPR-Richtlinie in regelmäßigen Abständen aufgefordert werden, ihre Sicherheitsinformationen zu überprüfen, wird die Anmeldung der Benutzer unterbrochen, und es werden alle registrierten Methoden angezeigt. Sie können die aktuellen Informationen bestätigen, wenn diese auf dem neuesten Stand sind, oder ggf. Änderungen vornehmen. Benutzer müssen eine mehrstufige Authentifizierung ausführen, wenn sie auf diese Seite zugreifen.

### <a name="manage-mode"></a>Verwaltungsmodus

Benutzer haben Zugriff auf den Verwaltungsmodus, indem sie zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) navigieren oder unter „Mein Profil“ die Option **Sicherheitsinformation** auswählen. Dort können Benutzer Methoden hinzufügen, vorhandene Methoden löschen oder ändern, die verwendete Standardmethode ändern und vieles mehr.

## <a name="key-usage-scenarios"></a>Wichtige Verwendungsszenarien

### <a name="set-up-security-info-during-sign-in"></a>Einrichten der Sicherheitsinformationen bei der Anmeldung

Ein Administrator hat die Registrierung erzwungen.

Ein Benutzer hat nicht alle erforderlichen Sicherheitsinformationen eingerichtet und navigiert zum Azure-Portal. Nach der Eingabe des Benutzernamens und Kennworts wird der Benutzer aufgefordert, die Sicherheitsinformationen einzurichten. Der Benutzer führt dann die im Assistenten angezeigten Schritte zum Einrichten der erforderlichen Sicherheitsinformationen aus. Wenn es Ihre Einstellungen zulassen, kann der Benutzer auch andere Methoden als die standardmäßig angezeigten einrichten. Nach Abschluss des Assistenten überprüfen die Benutzer die eingerichteten Methoden sowie ihre Standardmethode für die mehrstufige Authentifizierung. Um den Einrichtungsvorgang abzuschließen, bestätigt der Benutzer die Informationen und navigiert dann zum Azure-Portal.

### <a name="set-up-security-info-from-my-profile"></a>Einrichten der Sicherheitsinformationen unter „Mein Profil“

Ein Administrator hat die Registrierung nicht erzwungen.

Ein Benutzer, der noch nicht alle erforderlichen Sicherheitsinformationen eingerichtet hat, navigiert zu [https://myprofile.microsoft.com](https://myprofile.microsoft.com). Der Benutzer wählt im linken Bereich **Sicherheitsinformation** aus. Dort gibt der Benutzer das Hinzufügen einer Methode an, wählt eine der verfügbaren Methoden aus und führt die Schritte zum Einrichten dieser Methode aus. Nach Abschluss des Vorgangs wird dem Benutzer die eingerichtete Methode auf der Seite „Sicherheitsinformation“ angezeigt.

### <a name="delete-security-info-from-my-profile"></a>Löschen der Sicherheitsinformationen unter „Mein Profil“

Ein Benutzer, der zuvor mindestens eine Methode eingerichtet hat, navigiert zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo). Der Benutzer löscht eine der zuvor registrierten Methoden. Anschließend wird diese Methode nicht mehr auf der Seite „Sicherheitsinformation“ angezeigt.

### <a name="change-the-default-method-from-my-profile"></a>Ändern der Standardmethode unter „Mein Profil“

Ein Benutzer, der zuvor mindestens eine Methode eingerichtet hat, die für die mehrstufige Authentifizierung verwendet werden kann, navigiert zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo). Der Benutzer ändert die aktuell verwendete Standardmethode in eine andere Standardmethode. Anschließend wird die neue Standardmethode auf der Seite „Sicherheitsinformation“ angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Erste Informationen finden Sie unter [Aktivieren der Self-Service-Kennwortzurücksetzung](tutorial-enable-sspr.md) und [Aktivieren der Azure Multi-Factor Authentication](tutorial-enable-azure-mfa.md).

Erfahren Sie, wie Sie die [kombinierte Registrierung in Ihrem Mandanten aktivieren](howto-registration-mfa-sspr-combined.md) oder [die erneute Registrierung von Authentifizierungsmethoden durch Benutzer erzwingen](howto-mfa-userdevicesettings.md#manage-user-authentication-options).

Sie können auch den Artikel [Verfügbare Methoden für Multi-Factor Authentication und SSPR](concept-authentication-methods.md) lesen.