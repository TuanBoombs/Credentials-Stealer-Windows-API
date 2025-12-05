# 🔐 GetWindowsCredentials

A Windows credential harvesting tool that leverages the legitimate Windows API to prompt users for their credentials.

[![Platform](https://img.shields.io/badge/platform-Windows-blue.svg)](https://www.microsoft.com/windows)
[![Language](https://img.shields.io/badge/language-C%2B%2B-00599C.svg)](https://isocpp.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## Overview

GetWindowsCredentials uses the Windows API function [CredUIPromptForWindowsCredentialsW](https://learn.microsoft.com/en-us/windows/win32/api/wincred/nf-wincred-creduipromptforwindowscredentialsw) to display a native Windows credential dialog box. When users enter their credentials, the tool validates them against the system and saves successful login attempts to a file.

![Example](./images/example.png)

## Features

- **Native Windows Dialog** - Uses legitimate Windows credential UI
- **Credential Validation** - Verifies credentials against the domain/local system
- **Persistent Prompting** - Continues to prompt until valid credentials are entered
- **Domain Support** - Handles both domain and local accounts
- **Silent Logging** - Saves credentials to a log file without user notification

## How It Works

1. Displays a Windows credential prompt using `CredUIPromptForWindowsCredentialsW`
2. Captures username, password, and domain information
3. Validates credentials using `LogonUserW` API
4. If validation fails, prompts again until successful
5. Saves valid credentials to `C:\Windows\Temp\creds.log`

## Technical Details

### APIs Used

- `CredUIPromptForWindowsCredentialsW` - Display credential dialog
- `CredUnPackAuthenticationBufferW` - Extract credentials from buffer
- `CredUIParseUserNameW` - Parse username and domain
- `LogonUserW` - Validate credentials against the system

### Output Format

Credentials are saved in the following format:
```
[+]Username: DOMAIN\username , Password: password123
```

## Building

### Prerequisites

- Visual Studio 2015 or later
- Windows SDK
- C++ compiler with Windows API support

### Compilation

**Using Visual Studio:**
1. Open Developer Command Prompt
2. Navigate to the source directory
3. Run the following command:

```cmd
cl /EHsc GetWindowsCredentials.cpp /link Credui.lib
```

**Using MinGW:**
```bash
g++ GetWindowsCredentials.cpp -o GetWindowsCredentials.exe -lCredui -mwindows
```

## Usage

### Basic Execution

```cmd
GetWindowsCredentials.exe
```

The program will:
1. Display a Windows credential dialog
2. Wait for the user to enter credentials
3. Validate the credentials
4. Save successful logins to `C:\Windows\Temp\creds.log`
5. Repeat if validation fails

### Customization

You can modify the following constants in the source code:

```cpp
// Dialog text
WCHAR baseCaption[] = L"Enter current user credentials:";
WCHAR pszCaptionText[] = L"Your screen has been locked for security";

// Output file location
WCHAR saveAs[] = L"C:\\Windows\\Temp\\creds.log";
```

## Code Structure

### Main Components

**WriteCred Function**
- Writes captured credentials to a log file
- Formats output with username and password

**WinMain Function**
- Main entry point
- Displays credential prompt
- Validates credentials
- Loops until valid credentials are provided

### Key Variables

- `username` - Captured username (max 514 characters)
- `password` - Captured password (max 256 characters)
- `domain` - Captured domain (max 337 characters)
- `bLoginStatus` - Validation result flag

### Why Credentials Are Validated

The tool validates credentials using `LogonUserW` to:
- Ensure captured credentials are legitimate
- Avoid logging incorrect passwords
- Simulate real-world attack scenarios
- Demonstrate the full credential harvesting process

### Loop Behavior

The program uses a `goto` statement to create a loop that continues prompting until valid credentials are provided. This simulates a locked screen scenario where the user must authenticate to proceed.

## References

- [CredUIPromptForWindowsCredentialsW API](https://learn.microsoft.com/en-us/windows/win32/api/wincred/nf-wincred-creduipromptforwindowscredentialsw)
- [CredUnPackAuthenticationBufferW API](https://learn.microsoft.com/en-us/windows/win32/api/wincred/nf-wincred-credunpackauthenticationbufferw)
- [LogonUserW API](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-logonuserw)


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.