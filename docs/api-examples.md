# API examples

The following are simple examples that show how to consume the Fast Connect API.

## Example 1: Log on a user

The following code provides user credentials to SSO to log on a user.

```
bool DoLogonSsoUser(const std::wstring &username,
                    const std::wstring &domain,
                    const std::wstring &password)
{
    if (username.empty()
            || password.empty()
            || domain.empty())
    {
        // Credentials unavailable or incomplete.
        return false;
    }

    // Credentials are available, inject them to SSO.
    CitrixSSOnSDK::LOGONSSOUSER_ERROR_CODE result =
        CitrixSSOnSDK::LogonSsoUser(username.c_str(),
                                    domain.c_str(),
                                    password.c_str());

    // Check whether the credential injection was successful.
    if (result != CitrixSSOnSDK::LOGONSSOUSER_OK)    {
        // Get the result description.
        const wchar_t *result_description = CitrixSSOnSDK::ErrorDescription(result);

        // Report the error.
        wprintf(L"LogonSsoUser() failed: %d %ls\n", result, result_description);

        return false;
    }

    // Success.
    return true;
}
```
## Example 2: Log on a user with a smart card

The following code provides user credentials to SSO to log on a user with a smart card.

```
bool DoLogonSsoUserWithPin(const std::wstring &pin)
{
    if (pin.empty())
    {
        // PIN unavailable.
        return false;
    }

    // PIN is available, inject it to SSO.
    CitrixSSOnSDK::LOGONSSOUSER_ERROR_CODE result =
        CitrixSSOnSDK::LogonSsoUserWithPin(pin.c_str());

    // Check whether the PIN injection was successful.
    if (result != CitrixSSOnSDK::LOGONSSOUSER_OK)
    {
        // Get the result description.
        const wchar_t *result_description = CitrixSSOnSDK::ErrorDescription(result);

        // Report the error.
        wprintf(L"LogonSsoUserWithPin() failed: %d %ls\n", result, result_description);

        return false;
    }

    // Success.
    return true;
}
```

## Example 3: Log off a user

This function performs a logoff from a SSO perspective, removing the user’s credentials from the system for the purpose of future SSO-based authorization. Existing authorized sessions can still be used. If the previous user was not logged off, that user’s credentials are restored (see example 4).

```
void DoLogoffSsoUser()
{
    if (CitrixSSOnSDK::LogoffSsoUser() == LOGONSSOUSER_OK)
        printf("\nUser logged off\n");
    else
        printf("\nError logging off user\n");
}
```

## Example 4: Managing an exclusive endpoint

If the current user is not logged off and a new user is logged on, the previous user’s credentials are saved by the system to be restored when the current user logs off. Only a single level of restore is supported. Logging off a second time without an intervening logon results in clearing the SSO credentials and requires authentication before launching new sessions. This supports the following scenario using Controller, which is a custom application using CredInject:

1.	A kiosk is auto-logged on with a default/generic user (Controller calls LogonSsoUser())
2.	The kiosk is then available with generic credentials for customer use.
3. Sales Rep A logs on with his own credentials:
	4. Controller calls LogonSsoUser().
	5. Controller disconnects generic user’s sessions.
	6. Controller roams existing sessions for Sales Rep A.
7. The kiosk can launch new sessions using Sales Rep A’s credentials.
8. Sales Rep A logs off:
	9. 	Controller calls LogoffSsoUser(), removing Sales Rep A’s credentials and restoring the generic user.
	10. Controller restores generic user’s sessions.
2.	The kiosk is back to its initial state and is available for use with generic credentials.
3.	Sales Rep B logs on (repeat Steps 3-5 for Sales Rep B).
4.	The kiosk is back to its initial state.

This scenario can be repeated infinitely.

## Example 5: Managing user restore and shared endpoints

If the current user is not logged off and a new user is logged on, the previous user’s credentials are saved by the system to be restored when the current user logs off. Only a single level of restore is supported. Logging off a second time without an intervening logon results in clearing the SSO credentials and requires authentication before launching new sessions. This supports the following scenario using Controller, which is a custom application using CredInject:

1.	A nurse logs on normally and launches sessions. (Controller calls LogonSsoUser()).
2.	A doctor “taps in”:
	3. Controller executes LogonSsoUser() with the doctor’s credentials.
	4. Controller does not log off the nurse or disconnect sessions.
	5. Controller causes any existing doctor’s sessions to be roamed to this endpoint.
3.	At this point both the nurse’s and the doctor’s sessions are active. New sessions will be launched using the doctor’s credentials.
4.	The doctor “taps out”:
	5. Controller calls LogoffSsoUser(), removing the doctor’s credentials and restoring the nurse’s.
	6. Controller disconnects the doctor’s sessions.
	7. The nurse’s sessions are still active.
8. The nurse resumes as at Step 1.



 


