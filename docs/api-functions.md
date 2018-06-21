# API functions

The Credential Insertion SDK is a C/C++ library that must be consumed by external source code. The module name is libCredInject.so.
 The Credential Insertion API exposes four functions to enable the use of SSO as follows:

* LogonSSOUser
* LogonSSOUserWithPin
* LogoffSSOUser
* ErrorDescription

The function is available under the namespace CitrixSSOnSDK.

## LogonSSOUser

```
LOGONSSOUSER_ERROR_CODE LogonSsoUser (const wchar_t *username,
										const wchar_t *domain,
										const wchar_t *password);
```
This function is used to provide user credentials to SSO.


| Parameter | Description |
|---|---|
| username | The username |
| domain | The domain |
| password | The password |

| Return value | Description |
|---|---| 
| LOGONSSOUSER\_OK | Operation completed |
| LOGONSSOUSER\_INVALID\_PARAMETER | Invalid parameter passed to the function |
| LOGONSSOUSER\_INITIALIZATION\_FAILED | An error occurred initializing the SSO client |
| LOGONSSOUSER\_UNABLE\_TO\_CONNECT\_TO\_SSO | Unable to connect to the SSO service (AM)|
| LOGONSSOUSER\_UNABLE\_TO\_SEND\_REQUEST | Unable to send the request to the SSO service |
| LOGONSSOUSER\_UNABLE\_TO\_RECEIVE\_RESPONSE | Unable to receive the response from the SSO service |
| LOGONSSOUSER\_INVALID\_REQUEST\_TYPE | Invalid SSO request type |
| LOGONSSOUSER\_CONTAINER\_FULL | The SSO container is full and cannot store more credentials | 
| LOGONSSOUSER\_SERVER\_INTERNAL\_ERROR | An error has occurred in AM while processing the request |
| LOGONSSOUSER\_SERVER\_IPC\_ERROR | An error has occurred during the IPC communication with the server (AM) |

## LogonSsoUserWithPin

```
LOGONSSOUSER_ERROR_CODE LogonSsoUserWithPin(const wchar_t *pin)
```

This function is used to provide smart card user credentials to SSO.

| Parameter | Description |
|---|---|
| pin | The smart card PIN |

| Return value | Description |
|---|---|
| LOGONSSOUSER\_OK | Operation completed |
| LOGONSSOUSER\_INVALID\_PARAMETER | Invalid parameter passed to the function |
| LOGONSSOUSER\_INITIALIZATION\_FAILED | An error occurred initializing the SSO client |
| LOGONSSOUSER\_UNABLE\_TO\_CONNECT\_TO\_SSO | Unable to connect to the SSO service (AM) |
| LOGONSSOUSER\_UNABLE\_TO\_SEND\_REQUEST | Unable to send the request to the SSO service |
| LOGONSSOUSER\_UNABLE\_TO\_RECEIVE\_RESPONSE | Unable to receive the response from the SSO service |
| LOGONSSOUSER\_INVALID\_REQUEST\_TYPE | Invalid SSO request type |
| LOGONSSOUSER\_CONTAINER\_FULL | The SSO container is full and cannot store more credentials |
| LOGONSSOUSER\_SERVER\_INTERNAL\_ERROR | An error has occurred in AM while processing the request |
| LOGONSSOUSER\_SERVER\_IPC\_ERROR | An error has occurred during the IPC communication with the server (AM) |

## LogoffSsoUser

```
int LogoffSsoUser()
```
This function removes the credentials of the current SSO user and restores the previous user’s credentials if available.

| Return value | Description |
|---|---|
| LOGONSSOUSER\_OK | Operation completed |
| LOGONSSOUSER\_INVALID\_PARAMETER | Invalid parameter passed to the function |
| LOGONSSOUSER\_INITIALIZATION\_FAILED | An error occurred initializing the SSO client |
| LOGONSSOUSER\_UNABLE\_TO\_CONNECT\_TO\_SSO | Unable to connect to the SSO service (AM) |
| LOGONSSOUSER\_UNABLE\_TO\_SEND\_REQUEST | Unable to send the request to the SSO service |
| LOGONSSOUSER\_UNABLE\_TO\_RECEIVE\_RESPONSE | Unable to receive the response from the SSO service |
| LOGONSSOUSER\_INVALID\_REQUEST\_TYPE | Invalid SSO request type |
| LOGONSSOUSER\_UNAUTHORIZED | Trying to remove a set of credentials that was stored in SSO by AM itself |
| LOGONSSOUSER\_SERVER\_INTERNAL\_ERROR | An error has occurred in AM while processing the request |
| LOGONSSOUSER\_SERVER\_IPC\_ERROR | An error has occurred during the IPC communication with the server (AM) |

## ErrorDescription

```
const wchar_t *ErrorDescription(LOGONSSOUSER_ERROR_CODE errorCode)
```

| Parameter | Description |
|---|---|
| errorCode | The error code to get the description of |

| Return value | Description |
|---|---|
| The error description | The error description string, or an “Unknown error” message if the error code is unknown |












